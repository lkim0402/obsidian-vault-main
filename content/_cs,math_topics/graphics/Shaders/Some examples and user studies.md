
![[Pasted image 20250306144918.png|600]]
- shaders allow u to customize anything u want
	- rather than just having textures, u can also animate these, blend them with other things, multiply into the scene, etc 
- imagine the mesh (the structure) first, then the shaders on top of it

![[Pasted image 20250306152532.png|600]]
- different particles use different blending modes
	- "how should this combine with the background?"
- for example, the halo has a multiplicative effect around to make the background darker

![[Pasted image 20250306152839.png|300]]
- refraction
	- distorts the image

![[Pasted image 20250306153113.png|600]]
- the shader can then take lots of different textures and combine them in different ways
	- the pink smoke like effect -> most likely a texture that is mapped along the whole shape
- this is outside of particle effects, it more like, you have a mesh, and u want it to look a very specific way so u use shaders

![[Pasted image 20250306153630.png|600]]
- a more render pipeline related thing
	- shadows -> requires lots of processing on GPU. GPU needs to know about the world in order to cast shadows
	- most bg is mostly well lit
- you render the scene from the point of the view source, and u only render how far away the geometry is
	- and you can use that info in the usual frame to know where the shadow/light is hitting a certain point or not

![[Pasted image 20250306154245.png|300]]
- skill is special -> subsurface scattering
	- light enters something, and scatters under in the surface, then the light gets back out
	- some wavelengths are absorbed
- so in skin, white light goes to skin, some light is absorbed, and u have a red light going out
- different parts have diff properties
	- lips have glossy property
- all of these things are defined in the shader
	- where do we want it to be glossy? mat? subsurface scattering?
- hair rendering is surprisingly difficult

![[Pasted image 20250306155352.png|300]]![[Pasted image 20250306155358.png|300]]
- Fresnel
	- as something starts to face away from you, you get a stronger light
	- works best for relative smooth surfaces