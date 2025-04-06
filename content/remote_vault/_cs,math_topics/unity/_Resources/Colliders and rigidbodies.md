- Related: [[Triggers]]
### Colliders
> component that defines the shape of an object for the purposes of physical collisions
> https://docs.unity3d.com/ScriptReference/Collider.html
```C#
private void OnCollisionEnter(Collision other)
{
	Debug.Log("Bumped!");
	//component name in <>
	GetComponent<MeshRenderer>().material.color = Color.yellow;
}

// other functions
OnCollisionStay()
OnCollisionExit()
```
- Lots of colliders
	- Rigidbody is needed to be able to bump
		- Not all components need Rigidbody (ex. if there is a ball and a wall, only the ball can have rigidbody)
- It's a good idea to freeze components too so that it doesn't do crazy stuff
	- Freeze Position: `Y` (you don't want it flying)
	- Freeze  Rotation: `X,Y,Z` (you don't want player to rotate at all)
- `OnCollisionEnter` is a callback (used when 2 colliders collide)
- sphere, capsule, box
- For more complex objects
	- combine several primitive colliders to different parts of the game object
	- mesh collider (often gets too detailed, impacting efficiency)
	- combine shaders AND use mesh collider

### Collision detection messages
https://docs.unity3d.com/Manual/collider-types-interaction.html
When a pair of colliders make contact, they generate **collision detection** msgs if the following are both true:
- There is at least one dynamic collider.
- The other collider is a static collider, a kinematic collider, or another dynamic collider.![[Pasted image 20241031112753.png]]
	- static collider = collider
	- dynamic collider = collider + rigidbody
	- kinematic collider = collider + (is kinematic option) rigidbody
### Collision generates trigger messages
Trigger messages occur in the following circumstances:
- A dynamic or kinematic trigger collider collides with any collider type.
- A static trigger collider collides with any dynamic or Kinematic collider.
![[Pasted image 20241031112820.png]]
- So you can just make your ship have a dymanic trigger collider (doesn't matter if its kinematic or not) so it can collide with everything lol
#### Making a collider into a trigger
- We make a collider into a trigger by checking `isTrigger`
- Objects can pass **through the collider** and can be detected through code
- Make your trigger objects static
```C#
OnTriggerEnter(Collider other)
OnTriggerStay(Collider other)
OnTriggerExit(Collider other)
```

### Sample script
```C#
using UnityEngine;

// on the player ship
public class CollisionHandler : MonoBehaviour
{
    // Start is called before the first frame update
    void OnCollisionEnter(Collision other)
    {
        Debug.Log(this.name + " -- collided with -- " + other.gameObject.name);
    }

    void OnTriggerEnter(Collider other)
    {
        // OnTriggerEnter
        // String interpolation
        Debug.Log($"{this.name} ** triggered by ** {other.gameObject.name}");
    }
}
```
- For Argon Assault, put the rididbody and collider on the ship, and just put the colliders on the obstacles/enemies!

### Rigidbody
> component that gives an object the ability to move, fall, or be affected by physics (like gravity, forces, and collisions). 
- Adding a rigidbody to an object enables Unity’s physics engine to control the movement and collisions of that object.
- Without a rigidbody, an object will stay where it is, even if you apply forces or try to simulate gravity.
- As a general rule, moving objects should be **rigidbody objects**
```C#
public Rigidbody rb;

void Start()
{
	rb.AddForce
}
```
- must also have collider attached 
![[Pasted image 20240929225658.png]]
- Mass: how much force is required to move, stop, or change the motion of that object
- Drag: How quickly it will slow down (air resistance)
- Angular velocity: How quickly it will slow its rotation
- isKinematic: manual scripting (not affected by physics)
### Use case
> Box falling to ground
- **The Box** 
	- the box should have both a **collider** and a **rigidbody** if you want it to fall and interact with other objects physically
- **The Ground** 
	- the ground only needs a **collider**, not a rigidbody, because you generally don’t want the ground to move or be affected by gravity
	- In Unity, objects without a rigidbody but with a collider are considered **static** objects, which can be collided with but do not move themselves.
