- This is through using Unity's *old* Input system (Input Manager)
- We use `Input.GetAxis`

- `Edit> Project Settings > Input Manager`
	![[Pasted image 20241023120652.png|400]]
	![[Pasted image 20241023120834.png]]
	- There will be 2 sets of Horizontal and  Vertical -> one for key/mouse and the other for gamepad/joystick
	- `Dead` -> has meaning on controller not keyboard/mouse
	- Sensitivity -> speed to move target values (how fast the object moves)
### Example 1
```C#
public class Mover : MonoBehaviour
{
	//You can use serialized field to see in the unity editor
	//The value in the editor overrides this
	[SerializeField] float yValue = 0.01f

    // Start is called before the first frame update
    void Start()
    {
    }

    // Update is called once per frame
    void Update()
    {
	    // We define these there since we have to update it every frame
        float xValue = Input.GetAxis("Horizontal");
        float zValue = Input.GetAxis("Vertical");
        transform.Translate(xValue, yValue, zValue);
        
    }
}
```

### Example 2
```C#
public class PlayerController : MonoBehaviour
{
    [SerializeField] float speedFactor = 1f;

    void Update()
    {
        // ==== OLD INPUT MANAGER ====
        float xOffset = Input.GetAxis("Horizontal");
        float yOffset = Input.GetAxis("Vertical");

        float xPosition = transform.localPosition.x + (xOffset * Time.deltaTime * speedFactor);
        float yPosition = transform.localPosition.y + (yOffset * Time.deltaTime * speedFactor);

        transform.localPosition = new Vector3(
            xPosition, 
            yPosition, 
            transform.localPosition.z
            );
    }
}

```