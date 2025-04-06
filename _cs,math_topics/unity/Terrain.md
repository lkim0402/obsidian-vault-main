- A 3D object
	- Right click > 3D object > Terrain
- You can change the terrain width and length
	![[Pasted image 20241022134355.png]]
	![[Pasted image 20241022134407.png]]
- Paint terrain
	![[Pasted image 20241022134704.png]]
	![[Pasted image 20241022134925.png]]
	- Left click mouse
	- Press `Shift` while left clicking to reduce the terrain
	- You can change the brush size through the inspector, or by pressing `[` or `]`
		![[Pasted image 20241022135538.png|300]]
		- The opacity - the lower, the more slower the brush moves
- You can set height
	![[Pasted image 20241022140136.png]]
	![[Pasted image 20241022140125.png]]
	- It creates a plateau
	- If you `Flatten All` everything is set to the height you set it 
- If your terrain plane is at 0,0,0 and you have it flatten all to height 100, you can set your transform position to -100 so that it's actually on evey level while letting you be able to have valleys
- You can move around while pressing right click + WASD and uncheck camera acceleration under the camera icon
- Jitter
	![[Pasted image 20241022145615.png]]
	- Jitter is basically the randomness	

### Texture
- Paint Terrain > Paint Texture >  Layer > Add layer
	![[Pasted image 20241022171918.png]]
- Choose layer
	![[Pasted image 20241022174008.png]]
	![[Pasted image 20241022174018.png]]
- If it seems like it's not showing very well, increase the size (X and Y was 2 x 2, but now changed to 20x20)
	![[Pasted image 20241022173953.png]]
- 
### Terrain tool package
>  Terrain tools > additional unity package that lets you do more stuff
- Just test with things 
	- Erosion -> wind
		- creates path as if it's eroded by wind
	- Sculpt -> Clone, Sculpt -> Bridge
	- Transform -> pinch
		- you can "pinch" your terrains
	- Transform -> Smudge
		- smudging everything (supder handy)
	- Paint hole
		- you can ctrl and click and fill in hole again
		- caves, pass throughs, and other useful features

### Scene view hotkeys
- `A` - Brush strengh (opacity)
- `S` - Brush size
- `D` - Controls Brush rotation
- `Control` - Inverts the Brush effect for most Brushes; acts as a modifier for others
- `Shift` - Temporarily enables the smoothing Brush