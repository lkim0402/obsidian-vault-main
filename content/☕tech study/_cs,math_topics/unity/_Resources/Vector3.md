> A structure that represents a point or direction in 3D space using three components: **X**, **Y**, and **Z**. It is a fundamental data type used throughout Unity to describe positions, directions, and other 3D vectors.

```C#
Vector3 position = new Vector3(1, 2, 3); transform.position = position;
```

- BOTH direction and magnitude (so it's different from position)
	- How fast we're going + what direction we're going in
- Shortcuts
	- `Vector3.up` : (0,1,0)