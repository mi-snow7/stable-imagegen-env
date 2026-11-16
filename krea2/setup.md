# Krea2 Setup (Forge Neo)

## Requirements

<!-- TODO -->

## 1. Install Forge Neo

<!-- TODO: add the package in Stability Matrix -->

## 2. Download the Model Files

Krea2 needs a checkpoint plus a separate VAE and text encoder. Download each file
yourself; they are not installed with Forge Neo.

| Type | File | Source | Location in the shared Models folder |
|---|---|---|---|
| Checkpoint | Any Krea2 checkpoint | <!-- TODO --> | <!-- TODO --> |
| VAE | `qwen_image_vae.safetensors` | Hugging Face (search for the file name) | <!-- TODO --> |
| Text encoder | `qwen3vl_4b_bf16.safetensors` | Hugging Face (search for the file name) | <!-- TODO --> |
| Upscaler | `4x-UltraSharp` | Hugging Face (search for the file name) | <!-- TODO --> |

## 3. Launch Options

<!-- TODO -->

## 4. Extensions

<!-- TODO: table of extension, purpose, required or optional -->

## 5. WebUI Settings

<!-- TODO -->

## 6. Generation Settings

### Model

| Setting | Value | Description |
|---|---|---|
| UI Preset | `krea` | Switches the WebUI to the options used by Krea2. Select it first; it changes which settings are shown. |
| Checkpoint | Any Krea2 checkpoint | The base model or a Krea2 fine-tune. |
| VAE / Text Encoder | `qwen_image_vae.safetensors`, `qwen3vl_4b_bf16.safetensors` | Select both files. Krea2 does not work without them. |

### Generation Tab

| Setting | Value | Description |
|---|---|---|
| Sampling method | `Euler` | |
| Schedule type | `Simple` | |
| Sampling steps | `8` | |
| CFG Scale | `1` | |
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

## Troubleshooting

<!-- TODO -->
