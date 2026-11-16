# Stability Matrix Setup

[Stability Matrix](https://github.com/LykosAI/StabilityMatrix) installs and manages
image generation packages such as Forge Neo and ComfyUI. All packages share one
Models folder, so each model file is downloaded only once.

The guides in this repository use `<StabilityMatrix>` for the folder where Stability
Matrix is installed.

## 1. Install Stability Matrix

1. Download the latest release from the
   [releases page](https://github.com/LykosAI/StabilityMatrix/releases).
2. Extract it to a folder on a drive with plenty of free space. Model files take up
   a lot of space.
3. Start Stability Matrix and select **Portable Mode** when asked where to store data.

In Portable Mode, all data and settings are stored in a `Data` folder next to the
application, so you can move the whole folder to another drive or PC. The paths in
these guides assume Portable Mode.

## 2. Shared Models Folder

Models are stored in `<StabilityMatrix>\Data\Models`. Every package installed in
Stability Matrix reads models from here, so put model files in this folder, not in
the package's own folder.

The folders used in these guides:

| Folder | Contents |
|---|---|
| `StableDiffusion` | Checkpoints |
| `DiffusionModels` | Diffusion models without a VAE or text encoder (UNet-only) |
| `Lora` | LoRA files |
| `VAE` | VAE files |
| `TextEncoders` | Text encoders |
| `ESRGAN` | Upscalers, such as `4x-UltraSharp` |

The other folders, such as `ControlNet` and `Embeddings`, are for other model types.
Stability Matrix creates all of them automatically.

## 3. Add Packages

1. Open **Packages** and select **Add Package**.
2. Select the package and install it.

The packages used in these guides:

| Package | Used for | Guide |
|---|---|---|
| Stable Diffusion WebUI Forge - Neo | Image generation | [Krea2 setup](../krea2/setup.md) |
| ComfyUI | Captioning training images | [Captioning training images](../krea2/captioning.md) |

## 4. Update

- **Stability Matrix**: When an update is available, Stability Matrix shows a
  notification. Follow it to update.
- **Packages**: Open **Packages** and select **Update** on the package.

Updates can change how a package works or break its extensions and custom nodes. Avoid
updating in the middle of a project, and check that generation still works after an
update.
