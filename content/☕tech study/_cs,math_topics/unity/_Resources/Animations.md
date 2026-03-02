### Downloading from mixamo
- https://www.mixamo.com/#/
- Download settings
	- Format
		- `FBX for Unity` is more optimized for unity
	- Frames per Second
		- All 3 are ok since unity uses interpolation (explained below), but 30 is enough as a high FPS results in a very big file size
	- Keyframe reduction
		- reduction will simply go through all key frames in your animation, and evaluate the animation curves with and without that key, and if the difference is smaller then some defined delta, the key is removed
		- remove frames in the animations which have values that aren't different enough from its surrounding frames
		- Can lead to inaccurate animations
- In unity editor
	![[Pasted image 20240919174540.png]]
	- 2 joints and 2 surfaces
	- The first 2 (with black background): skinned versions of the meshes (combinations of their respective meshes, materials, and mixamo_rig).

Interpolation in animation
- Estimating the animation values between frames