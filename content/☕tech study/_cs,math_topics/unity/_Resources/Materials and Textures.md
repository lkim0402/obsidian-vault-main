- There are lots of shaders in unity
	![[Pasted image 20240925215428.png|500]]
- There lots of different options
	![[Pasted image 20240925215527.png|500]]
	- Albedo is the main color you want to set
	- If you're using a texture file, the Albedo will be replaced 
- Just **DRAG AND DROP THE MATERIAL** to the object you want to have that material. Then it will replace the current default material. 

#### Textures
- https://polyhaven.com/
- PBR Texturing
	- the process of creating digital two-dimensional images which store surface and color information which will be projected onto a 3D object
- PBR map (texture map)
	- Used in Physically-Based Rendering to define surface properties and simulate the interaction of light with materials, ultimately impacting the final visual result
	- Different ways of how a material is "shown" in real world
		![[Pasted image 20240925221142.png|300]]
		- `The diffuse file`: The map we're loading to be the texture image file applied to the actual objects
		- `The normal map`: Tells the engine where things should look bumpy!
			- This requires the texture type to be `Normal Map` (make sure of this in the `Inspector`)
		- `The roughness map`: How much things are reflected in different areas
			- "Metallic"
- You can click the tiny circle to choose the diffuse file for the Albedo
	![[Pasted image 20240925222327.png|400]]
	- Tiling: You can adjust how much the texture can repeat itself

Three main ways to create materials
- Any time you create a 3D object using Unity's built in types, it automatically assigns a default materials that is NOT EDITABLE.
- To create an editable material, you should create material or import 3d models.


- Bump map
	- texture with info on how light catches a shape
	- Height map
		- In the bump map
		- uses white to black scale to show height
	- Normal map
		- In the bump map
		- uses RGB values to indicate `x,y,z` facing direction