```C#
[SerializeField] float timeToWait = 2f;
MeshRenderer renderer; // renderer is cached

void Start()
{
	renderer = GetComponent<MeshRenderer>();
	renderer.enabled = false;
}
```
- Caching
	- A technique of storing frequently used data/info in memory, so it can be easily accessed
- when you use `GetComponent<T>()`, you're fetching a component that is attached to the same GameObject that the script is attached to
	- If the GameObject has the component, it returns a reference to it. 
	- If it doesn't, it returns `null`