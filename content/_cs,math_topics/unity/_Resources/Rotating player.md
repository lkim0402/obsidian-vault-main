### Rotating the rocket ship (Project Boost)
```C#
private void ApplyRotation(float rotationSpeed)
{
	rb.freezeRotation = true; // freezing rotation so we can manually rotate
	transform.Rotate(Vector3.forward * rotationSpeed * Time.deltaTime);
	rb.freezeRotation = false; //un-freezing rotation
}
```

### Rotating the local ship (Assault Argon)
```C#
void ProcessRotation()
{
	// x,y,z = pitch, yaw, and roll
	// when we go up/down the y axis, we rotate by the x-axis
	float pitchDueToPosition = transform.localPosition.y * -positionPitch; // position impacting pitch
	float pitchDueToControlThrow = yThrow * controlPitchFactor; // control impacting pitch
	float pitch = pitchDueToPosition + pitchDueToControlThrow; 

	// the yaw changes based on the position on screen (left/right)
	float yaw = transform.localPosition.x * positionYaw;

	// when we move right/left the z axis, the roll changes
	float roll = xThrow * controlRollFactor;

	transform.localRotation = Quaternion.Euler(pitch, yaw, roll);
}
```