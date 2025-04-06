Attach AudioSource component
- Bring `mainEngine` audio  ![[Pasted image 20240927114737.png]]
```C#
// Movement.cs
[SerializeField] AudioClip mainEngine;
AudioSource audioSource;
    
void Start()
{
	audioSource = GetComponent<AudioSource>();
}
    
void ProcessThrust() {
	if (Input.GetKey(KeyCode.Space))
	{
		rb.AddRelativeForce(Vector3.up * rocketSpeed * Time.deltaTime);
		// We only play when sound is not already playing to ensure there is no overlap of sound
		if (!audioSource.isPlaying) 
		{
			audioSource.PlayOneShot(mainEngine);
		}  
	} 
	else 
	{
	// We pause all sound when input key (space) is not pressed
		audioSource.Pause();
	}
}
```