# stable-imagegen-env

Setup guides for a stable local image generation environment, from installation
to LoRA training. Each model has its own directory with the same set of guides.

## Tested Environment

| Item | Value |
|---|---|
| OS | Windows 11 |
| GPU | NVIDIA GeForce RTX 4090 (24 GB VRAM) |
| RAM | 64 GB |
| Package manager | [Stability Matrix](https://github.com/LykosAI/StabilityMatrix) (latest) |
| Image generation | Stable Diffusion WebUI Forge - Neo |
| Captioning | ComfyUI + JoyCaption |
| LoRA training | [ai-toolkit](https://github.com/ostris/ai-toolkit) |

## Getting Started

Set up Stability Matrix first. All model guides below assume it is installed.

- [Stability Matrix setup](./stability-matrix/setup.md)

## Krea2

1. [Krea2 setup (Forge Neo)](./krea2/setup.md)
2. [Captioning training images (ComfyUI)](./krea2/captioning.md)
3. [LoRA training (ai-toolkit)](./krea2/lora-training.md)

## Repository Layout

Each model directory uses the same file names, so new models can be added the same way.

```
README.md                 # This index
stability-matrix/
  setup.md                # Set up first; used by all models
<model>/                  # For example krea2/, flux1/, sdxl-illustrious/
  setup.md                # Install the model and generate images
  captioning.md           # Caption training images
  lora-training.md        # Train a LoRA
  workflows/              # ComfyUI workflows and other config files
```

Placeholders such as `triggerword` and `C:\path\to\ai-toolkit` mark values
you need to change for your own environment.
