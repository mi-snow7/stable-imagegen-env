# Krea2 Setup (Forge Neo)

## Requirements

- Stability Matrix with the shared Models folder
  (see [Stability Matrix setup](../stability-matrix/setup.md))

## 1. Install Forge Neo

1. Open **Packages** in Stability Matrix and select **Add Package**.
2. Select **Stable Diffusion WebUI Forge - Neo** and install it.

The default launch options work. No extensions or WebUI settings changes are needed
for Krea2.

## 2. Download the Model Files

Krea2 needs a checkpoint plus a separate VAE and text encoder. Download each file
yourself; they are not installed with Forge Neo.

`<StabilityMatrix>` is the folder where Stability Matrix is installed. LoRA files go in
`<StabilityMatrix>\Data\Models\Lora`.

| Type | File | Source | Location in the shared Models folder |
|---|---|---|---|
| Checkpoint | Any Krea2 checkpoint | Hugging Face or Civitai | `<StabilityMatrix>\Data\Models\StableDiffusion` |
| VAE | `qwen_image_vae.safetensors` | Hugging Face (search for the file name) | `<StabilityMatrix>\Data\Models\VAE` |
| Text encoder | `qwen3vl_4b_bf16.safetensors` | Hugging Face (search for the file name) | `<StabilityMatrix>\Data\Models\TextEncoders` |
| Upscaler | `4x-UltraSharp` | Hugging Face (search for the file name) | `<StabilityMatrix>\Data\Models\ESRGAN` |

Notes on the text encoder:

- If you run out of VRAM, `qwen3vl_4b_fp8_scaled.safetensors` is an option. It is the
  same model compressed to about half the size, with a possible small loss in quality.
- Do not use `qwen_3_4b.safetensors` or other files without `vl` in the name. They are a
  different model (Qwen3 without vision) used by other image models, and Krea2 does not
  work correctly with them.

## 3. Generation Settings

### Model

| Setting | Value | Description |
|---|---|---|
| UI Preset | `krea` | Switches the WebUI to the options used by Krea2. Select it first; it changes which settings are shown. |
| Checkpoint | Any Krea2 checkpoint | The base model or a Krea2 fine-tune. |
| VAE / Text Encoder | `qwen_image_vae.safetensors`, `qwen3vl_4b_bf16.safetensors` | Select both files. Krea2 does not work without them. |

### Generation Tab

Recommended values differ between checkpoints. Check the checkpoint's model page
first, and use the values below as a starting point.

The values below are for a distilled (Turbo) checkpoint. Distilled checkpoints are
trained to produce in a few steps what the original model needs many steps for, so
8 steps and CFG Scale `1` are enough. Higher values make generation slower without
improving the image, and a higher CFG Scale can make colors too strong. Non-distilled
checkpoints need more steps and a higher CFG Scale.

| Setting | Value | Description |
|---|---|---|
| Sampling method | `Euler` | |
| Schedule type | `Simple` | |
| Sampling steps | `8` | Enough for a distilled (Turbo) checkpoint. |
| CFG Scale | `1` | Keep it at `1` for a distilled (Turbo) checkpoint. |
| Width / Height | Any (example: `1024` × `1536`) | Base resolution before Hires. fix. |

### Hires. fix

Enabled. Hires. fix generates the image at the base resolution, upscales it, then
redraws details at the larger size.

| Setting | Value | Description |
|---|---|---|
| Upscaler | `4x-UltraSharp` | Upscaler model used before the second pass. |
| Hires steps | `2` | Steps for the second pass. |
| Hires CFG Scale | `1` | CFG Scale for the second pass. Keep it the same as CFG Scale. |
| Denoising strength | `0.35` | How much the second pass may change the image. Higher values add more detail but can change faces and composition. |
| Upscale by | `1.5` | Final size is the base resolution × 1.5. For example, `1024` × `1536` becomes `1536` × `2304`. |
