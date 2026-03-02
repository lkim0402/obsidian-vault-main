- Manual: https://docs.unity3d.com/Manual/class-Quaternion.html
- Scripting: https://docs.unity3d.com/ScriptReference/Quaternion.html

- Unity uses the Quaternion class to store the 3 dimensional rotation of gameobjects
	- The Euler angles - the XYZ values you see in the inspector
	- The Quaternion values - the underlying value which Unity stores the *actual* rotation of GameObjects
- When handling rotations, you should use the Quaternion class and its functions to create and modify rotational values
	- You should use the Quaternion Class functions that deal with Euler angles - Retrieving, modifying, and re-applying Euler values from a rotation can cause unintentional side-effects


- Orientation
	- Where the object is pointing at
- Rotation
	- Changing the orientation

Eulers
- Rotation order matters (XYZ, YZX Eulers results differently with the same values!)
- Gimbal locks
	![[Pasted image 20241008174629.png|300]]
	- It overlaps! The blue and green rotation represents the same axis
	- Quaternions remove this issue!
### Quaternions
- used to represent rotations
- four-tuple of real numbers `{x,y,z,w}`
	- x,y,z aren't an axis!!
![[Pasted image 20241008174844.png]]
![[Pasted image 20241008175355.png]]

- After normalization we have a "Unit" Quaternion
	- Divide each components by its length
	- $U_q=q/||q||$
	- It's recommended to mormalize your quaternions




https://youtu.be/1yoFjjJRnLY?si=042uY6ao__8gbbRt