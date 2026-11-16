# Krea2 LoRA Training (ai-toolkit)

This guide trains a character LoRA for Krea2 with [ai-toolkit](https://github.com/ostris/ai-toolkit).
ai-toolkit is installed on its own, outside Stability Matrix.

## Before You Start

- **Prepare the captions first.** Follow [Captioning training images](./captioning.md).
  The captions decide what the trigger word learns.
- **The trigger word comes from the captions.** The captioning workflow writes the trigger
  word into every caption, so the `trigger_word` setting in ai-toolkit stays empty.
- **ai-toolkit counts steps, not epochs.** You set the total number of steps directly.
  With `batch_size: 1`, each step trains on one image. For example, 2000 steps on a
  50-image dataset shows each image about 40 times.

## 1. Install ai-toolkit

The only requirement is [Git](https://git-scm.com/).

```bash
git clone https://github.com/ostris/ai-toolkit.git
```

Open the `ai-toolkit` folder and run `run_windows.bat`. On the first run, it sets up
everything inside the `ai-toolkit` folder:

| Folder | Contents |
|---|---|
| `.uv` | [uv](https://docs.astral.sh/uv/), and Python 3.12 if no Python is installed |
| `.venv` | Python environment with PyTorch for your GPU |
| `.node` | Node.js for the Web UI |
| `.ffmpeg` | FFmpeg |

Nothing is installed system-wide, so it does not affect Stability Matrix or other
Python installs.

When setup finishes, the Web UI opens at `http://localhost:8675`. Use `run_windows.bat`
every time you start ai-toolkit; it also checks for updates.

## 2. Model Storage

ai-toolkit downloads the model files from Hugging Face the first time you train. It
does not use the files in the Stability Matrix Models folder.

| Part | Hugging Face repository |
|---|---|
| Krea2 model | [`krea/Krea-2-Raw`](https://huggingface.co/krea/Krea-2-Raw) |
| VAE | [`Qwen/Qwen-Image`](https://huggingface.co/Qwen/Qwen-Image) |
| Text encoder | [`Qwen/Qwen3-VL-4B-Instruct`](https://huggingface.co/Qwen/Qwen3-VL-4B-Instruct) |

The files are saved in the Hugging Face cache, which is on the C drive by default:

```
%USERPROFILE%\.cache\huggingface\hub
```

The files are large. To store them on another drive, set the `HF_HOME` environment
variable before you start ai-toolkit, then restart the terminal:

```bash
setx HF_HOME "D:\huggingface"
```

If a download fails because the repository requires you to log in or accept its terms,
create an access token on Hugging Face and enter it in the ai-toolkit **Settings** page.

## 3. Prepare the Dataset

Put the images and their caption files in one folder under `ai-toolkit\datasets`. Each
image needs a `.txt` file with the same name.

```
ai-toolkit\
  datasets\
    your_dataset\
      001.png
      001.txt
      002.png
      002.txt
      ...
```

## 4. Create a Training Job

The settings used for this guide are in
[`workflows/ai-toolkit-character-lora.yaml`](./workflows/ai-toolkit-character-lora.yaml).
Create a new job in the Web UI, open the advanced (YAML) view, and paste the file.
Then change these values:

| Setting | Placeholder | Set to |
|---|---|---|
| `config.name`, `meta.name` | `your_lora_name_v1` | Name of the LoRA. Also used for the output folder. |
| `training_folder` | `C:\path\to\ai-toolkit\output` | `output` folder in your ai-toolkit folder |
| `sqlite_db_path` | `C:\path\to\ai-toolkit\aitk_db.db` | `aitk_db.db` in your ai-toolkit folder |
| `folder_path` | `C:\path\to\ai-toolkit\datasets/your_dataset` | Your dataset folder |

The values below are the ones in the bundled file, tested on an RTX 4090 (24 GB VRAM).
Settings that are not listed can stay as they are.

### Settings You May Adjust

| Setting | Value | Description | When to change |
|---|---|---|---|
| `steps` | `2000` | Total training steps. See [Before You Start](#before-you-start). | Lower it if the LoRA overfits, raise it if the character does not look right yet. Compare the saved files first; an earlier save may already be good. |
| `lr` | `0.0001` | Learning rate. | Lower it, for example to `0.00005`, if the LoRA copies poses or backgrounds from the dataset. |
| `linear` | `32` | LoRA rank. Higher values store more detail, but also learn more unwanted detail and make the file larger. | Lower it, for example to `16`, for a smaller file or if the LoRA picks up too much from the dataset. |
| `linear_alpha` | `32` | Scales the LoRA's effect. With alpha equal to rank, the learning rate is used as is. | Change it together with `linear`. Do not set it higher than `linear`. |
| `resolution` | `512`, `768`, `1024` | Trains at three resolutions, so the LoRA works at different image sizes. | Remove `512` if the dataset has mostly large images and you only generate at high resolution. |
| `flip_x` | `false` | Mirrors images during training. | Keep it off for characters with one-sided features, such as a hair part or an accessory on one side. |
| `save_every` | `200` | Saves a LoRA file every 200 steps. | Lower it to compare more steps, raise it to save disk space. |
| `disable_sampling` | `true` | No preview images during training, which saves time. The `sample` section is not used. | Turn it off and add prompts to `samples` to watch progress during training. |
| `low_vram` | `false` | Lowers VRAM use at the cost of speed. | Turn it on if training runs out of VRAM. |

### Settings to Leave as They Are

| Setting | Value | Description |
|---|---|---|
| `name_or_path` | `krea/Krea-2-Raw` | Base model to train on. |
| `arch` | `krea2` | Model type. Must match the base model. |
| `quantize`, `qtype` | `true`, `qfloat8` | Loads the Krea2 model in 8-bit to reduce VRAM use. |
| `quantize_te`, `qtype_te` | `true`, `qfloat8` | Same for the text encoder. |
| `type` | `lora` | Trains a standard LoRA. |
| `optimizer` | `adamw8bit` | AdamW in 8-bit, which uses less VRAM. |
| `batch_size` | `1` | Images per step. |
| `gradient_checkpointing` | `true` | Saves VRAM at the cost of some speed. |
| `content_or_style` | `balanced` | Balances learning the subject and learning fine detail. |
| `train_text_encoder` | `false` | Trains only the image model. |
| `cache_text_embeddings` | `true` | Encodes the captions once at the start. Restart the job if you edit captions. |
| `trigger_word` | `null` | Empty, because the captions already contain the trigger word. |
| `num_repeats` | `1` | Not needed; set the amount of training with `steps`. |
| `caption_dropout_rate` | `0` | Always uses the captions. |
| `cache_latents_to_disk` | `true` | Saves the encoded images to disk, so they are not encoded again on the next run. |
| `max_step_saves_to_keep` | `12` | Keeps up to 12 saved files. With 2000 steps, all 10 saves are kept. |
| `dtype` (save) | `bf16` | Precision of the saved LoRA file. |

## 5. Use the LoRA in Forge Neo

The LoRA files are saved in `ai-toolkit\output\<LoRA name>\`, one file per save.

1. Copy the files you want to test to `<StabilityMatrix>\Data\Models\Lora`.
2. In Forge Neo, generate with the same prompt and seed using files from different
   steps, and pick the one that looks best. Later steps are not always better.
3. Write the trigger word in the prompt.

When writing prompts, describe what you want in positive terms. For example, write
`dry and matte skin` instead of `no shine`.
