- originally developed for procedural textures for 3D objects in computer graphics
- generates all the pixels that make texture (wood, metal, etc.)
- ultimately gives you **smooth** random numbers!
	- any number you pick at a time is related to the number you pick later/before
- lots of math on top of the actual noise() function

### How it works
https://youtu.be/Qf4dIN99e2w?si=MyGmgoxkv4HVeu6Z
- Interpolation function
1. **Base Noise:**
    - Start with a simple Perlin noise at a large scale (low frequency).
2. **Adding Layers (Octaves):**
    - Add more Perlin noise layers, each at a higher frequency (smaller scale) and lower amplitude (less intensity).
    - For example:
        - First octave: Big, smooth hills.
        - Second octave: Smaller hills added on top.
        - Third octave: Even smaller bumps on the smaller hills.
    - The layers combine to create a more complex, natural look.
3. **Weighting the Layers:**
    - Each octave contributes less to the final result. Typically, the amplitude is halved, and the frequency is doubled for each successive octave.

### Random vs Perlin
- `random(min, max)`
- `noise()`
	- The argument... it's like passing a particular point in time