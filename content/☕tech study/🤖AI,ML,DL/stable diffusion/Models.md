### Base Models
> [!Base Model]
> A base model in Stable Diffusion refers to the original model trained by StabilityAI before any additional fine-tuning or specialization.
> - Think of it like a foundation or starting point that can create a wide variety of images but isn't specialized in any particular style.

- SD1.5 base model
	- The oldest but most popular version of stable diffusion
	- The base model. All other versions are fine-tuned versions of it.
- SDXL base model
	- Newer, trained on larger images and is better at following the prompt
- Stable Cascade
	- Not much resources, uses different architecture to SD1.5 and SDXL.
- SD3
	- The upcoming newest version that has not been released

### Variations
- LCM (Latent Consistency Model)
	- Type of sampler that makes images much quicker with fewer steps but worse quality
- SDXL Turbo/Lightning
	- Like "fast drafts" but better quality than LCM
	- Regular SD might take 20-50 steps
	- These methods can do it in 3-8 steps

### LoRas
>[!LoRa: Low-Rank Adaptation]
>A method for fine-tuning AI models that helps add new styles or concepts without retraining the entire model

- Smaller models that attach to an existing model and alter it
- LoRa: 50-300MB vs main models: multiple GBs
- LoRa: trained on 10s-1000s images vs main models: millions of images
- Example - Kirby
	- A base model can generate kirby but details won't be right. You can train a LoRa on 20is images of kirby, attach it to a model, and the model can make kirby images
### Sources
- [this reddit comment](https://www.reddit.com/r/comfyui/comments/1brfzqh/comment/kxa599e/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)