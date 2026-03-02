- `public static float Clamp(float value, float min, float max);`
- Parameters
	- `value` - the floating point value to restrict inside the range defined by the `min` and `max` values
		- basically the thing we're restricting
	- `min` - The minimum floating point value to compare against
	- `max` - The maximum floating point value to compare against

```C#
public class PlayerController : MonoBehaviour
{
    [SerializeField] float speedFactor = 1f;
    
    // Clamp values
    [SerializeField] float xRange = 10f;
    [SerializeField] float yRange = 5f;
    

    void Update()
    {
        // ==== OLD INPUT MANAGER ====
        float xOffset = Input.GetAxis("Horizontal");
        float yOffset = Input.GetAxis("Vertical");

        float xPosition = transform.localPosition.x + (xOffset * Time.deltaTime * speedFactor);
        float yPosition = transform.localPosition.y + (yOffset * Time.deltaTime * speedFactor);

        // constraining x values so that it doesn't move out the screen
        float clampedXPos = Mathf.Clamp(xPosition, -xRange, xRange);
        float clampedYPos = Mathf.Clamp(yPosition, -yRange + 3, yRange + 3);

        transform.localPosition = new Vector3(
            clampedXPos, 
            clampedYPos, 
            transform.localPosition.z
            );
    }
}

```
- I just added the  +3 in the `yRange` to adjust