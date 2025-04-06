- `[SerializeField]` is an attribute

Before adding attributes
```C#
[SerializeField] float speedFactor = 1f;

// Clamp values
[SerializeField] float xRange = 10f;
[SerializeField] float yRange = 5f;

// Pitch values
[SerializeField] float positionPitch = 2f;
[SerializeField] float positionYaw = 2f;
[SerializeField] float controlPitchFactor = -10f;
[SerializeField] float controlRollFactor = -10f;
[SerializeField] GameObject[] lasers;

float xThrow;
float yThrow;
```

![[Pasted image 20241030154636.png]]

### After
```C#
[Header("General control/movement setup")]
[SerializeField] [Tooltip("The speed of ship movement")]  float speedFactor = 1f;

// Clamp values
[SerializeField] float xRange = 10f;
[SerializeField] float yRange = 5f;


[Header("General rotation setup")]
[SerializeField] 
[Tooltip("How much the position impacts the pitch")] float positionPitch = 2f;
[SerializeField] 
[Tooltip("How much the control impacts the pitch")]float controlPitchFactor = -10f;

[SerializeField] 
[Tooltip("How much the position impacts the yaw (ship head pointing left/right)")] float positionYaw = 2f;
[SerializeField] 
[Tooltip("How much the control impacts the yaw (ship body tilting left/right)")] float controlRollFactor = -10f;

[Header("Lasers for the ship")]
[SerializeField] GameObject[] lasers;
```
![[Pasted image 20241030155937.png]]
- ToolTip 
	- Additional description messages for the user
- Header
	- "Headers" that allows you to group and organize stuff