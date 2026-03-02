1. Download thru package manager
	![[Pasted image 20241023122120.png]]
	- Also go here and set to whatever settings you want 

### One way to do it
1. Add Player Input as component
2. Add Input Actions in Assets
3. Add controle scheme
		![[Pasted image 20241016144529.png]]
	- The requirement is that you have a keyboard
5. Add an Action Map
	- You create one for each thing, like if you're walking, driving a car, etc
6. Like this
	![[Pasted image 20241016145305.png]]![[Pasted image 20241016145312.png]]
	- You can "Listen" to input the `Path` by pressing the desired key
7. Movement
	- Action Type - Passthrough: We can map easily a vector2 to movement
		![[Pasted image 20241016145903.png]]
		![[Pasted image 20241016145946.png]]
8. Check in components
	![[Pasted image 20241016150135.png]]

#### Example script 1 (new)
```C#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerMovement : MonoBehaviour
{
    public Rigidbody2D rb;
    public float moveSpeed;
    private Vector2 _moveDirection;

    // reference to the action we want to use
    public InputActionReference move;
    public InputActionReference fire;

    [SerializeField] ParticleSystem fireParticles;

    void Start()
    {
        fireParticles.Stop();
    }

    void Update()
    {
        _moveDirection = move.action.ReadValue<Vector2>();
    }
    void FixedUpdate()
    {
        rb.velocity = new Vector2(_moveDirection.x * moveSpeed, _moveDirection.y * moveSpeed);
    }

	//called automatically if object is enabled
    void OnEnable()
    {
        // registering the event listener for the fire action
        // += means  you are "subscribing" to an event => "When the started event of fire.action occurs,
        // execute the Fire method
        fire.action.started += Fire;
    }

    void OnDisable()
    {
        fire.action.started -= Fire;
    }

    private void Fire(InputAction.CallbackContext obj)
    {
        Debug.Log("Fire!");
        fireParticles.Play();
    }
}

```

### Another way to do it
- Add `[SerializeField] InputAction movement;` to the script, then the gameobject will have this
	![[Pasted image 20241023122740.png]]
- If you press the `+` button:
	![[Pasted image 20241023122924.png]]
	![[Pasted image 20241023122934.png]]
- We bind them with buttons (paths > listen)
	![[Pasted image 20241023123126.png]]
- If I want to do something fire, for example, then just `[SerializeField] InputAction fire;` and repeat
#### Example script
```C#
public class PlayerController : MonoBehaviour
{
    [SerializeField] InputAction movement;

    private void OnEnable()
    {
        movement.Enable();
    }
    
    private void OnDisable()
    {
        movement.Disable();
    }

    // Update is called once per frame
    void Update()
    {
        // ==== OLD INPUT MANAGER ====
        // float horizontal = Input.GetAxis("Horizontal");
        // Debug.Log(horizontal);
        // float vertical = Input.GetAxis("Vertical");
        // Debug.Log(vertical);

        float horizontal = movement.ReadValue<Vector2>().x;
        float vertical = movement.ReadValue<Vector2>().y;

    }
}
```
- `OnEnable`
	-  This is during the initialization for the project
	- You actually have to enable `InputAction`
	- The very 1st thing that happens is `Awake`, then `OnEnable`
	- https://docs.unity3d.com/Manual/execution-order.html


- We can also use [[Mathf.Clamp]]