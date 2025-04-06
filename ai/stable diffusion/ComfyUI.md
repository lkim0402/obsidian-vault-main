- ComfyUI 설치
- ComfyUIManager 설치
- 7zip 사용해서 extract하기
	- `README_VERY_IMPORTANT.txt` 읽기
	- `ComfyUI_windows_portable_nvidia\ComfyUI_windows_portable\ComfyUI`
		- `extra_model_paths.yaml.example` 에서 example 지우기
		- vscode로 열어서 base path 바꾸기
			- `base_path: C:\Users\artygen\Desktop\ComfyUI_windows_portable_nvidia\ComfyUI_windows_portable\ComfyUI`

기본 Workflow
![[Pasted image 20241211150943.png]]
- Increasing batch size gives more output pictures

### Core Process
1. `CLIP Text Encode node`: Process your text prompt
2. `Empty Latent Image node`: Starts with random noise in "latent space" (think of this as a compressed, abstract version of an image)
3. `KSampler node`: Gradually denoises this random noise, guided by the prompt, until it forms a coherent image
4. `VAE Decode node`: converts the final latent image to pixels
### Nodes
- Load Checkpoint
	- 시작하는 node
	- If you click on it you can see what checkpoints you have
- Nodes
	- change colors/names
	- right click > Properties Panel
- Empty Latent Image
	- Latent Image: intermediate representation used by Stable Diffusion 
		- Pixel Image: the image output
	- `batch_size`: num of final output images
	- 이 노드를 빼고 이미지를 넣으면 img2img
		- Add Node > image > Load image
		- 이 pixel image를 latent image로 바꿔주기
- General process of images
	1. Random noise/empty latent space
	2. Latent image (transformed through the diffusion process)
	3. Final pixel image (decoded from latent space)
- KSampler
	- `control_after_generate`: 시드값 조정 가능
	- `denoise`: 결과 이미지에 얼마나 많은 변화를 줄건가 (img2img)

### Using LoRas
- think of [[ai/stable diffusion/Models#LoRas|LoRas]] as specialized "add-ons" that can enhance your generations in specific ways
- Purpose
	- Add specific art styles (like anime, watercolor, etc.)
	- Add specific characters or people
	- Add specific objects or concepts
	- Modify the model's style in particular ways (like making things more detailed or cartoonish)
- Basic example
	- `Load Checkpoint (base model) → Load LoRA → CLIP Text Encode → KSampler → VAE Decode`
- Can be chained together
	![[{597CB136-93A1-482F-B687-8F8320FFF071}.png]]
- `strength_model`
	- affects how strongly LoRa affects the actual generated image
- `strength_clip`
	- affects how strongly LoRa influences the prompt
	- modifies the CLIP part of Stable Diffusion, which is responsible for understanding text prompts

### Improving quality
- Pixel Image
	- Use `UpscaleImageUsingModel` Node
		- Put it between VAE decode and Save image
			![[{AD094127-36F5-4BB0-968F-7DC712028594}.png]]
	- Use `LoadUpscaleModel` Node 
- Latent Image
	- https://youtu.be/QFeUSU02gho?si=mLMaq6xPokrtbInJ
	- Upscale Latent Node