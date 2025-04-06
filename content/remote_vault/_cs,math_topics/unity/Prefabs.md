- Generally as a rule of thumb, DON'T scale anything within your parent prefab (it will create monstrous effects later when you create child prefabs under it)
- scaling of the parent is applied to the children

- Usually just make an empty game object (then prefab it)
	- leave the scale up to 1 1 1
- `Unpack Prefab` 
	- Basically breaking the connection between the prefab and the instance