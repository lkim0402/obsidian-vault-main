Things we need for sound
- Audio listener
	- To "hear" the audio
	- Almost always on the main camera
	- https://docs.unity3d.com/ScriptReference/AudioListener.html
	- There always has to be an Audio Listener somewhere in your scene
- Audio Source
	- To "play" the  audio
	- On the source that is generating the sound
		- Audio source component on rocket making thrusting sounds
	- https://docs.unity3d.com/ScriptReference/AudioSource.html
- Audio file
- Some sources
	- https://freesound.org/search/?q=rocket+sound
	- For recording: https://www.audacityteam.org/download/
- Make sure `Project Settings > Audio > System Sample Rate` is set to 1 not 0 (if it's 0 it won't play)

### Multiple sounds
![[Pasted image 20240919115338.png|400]]
- If we wanted to play different sounds from the same game object, we serialize it so that we attach it to our scripts
```C#
[SerializeField] AudioClip mainEngine;
AudioSource audioSource;

// in some function
audioSource.PlayOneShot(mainEngine);
```
- Like this:
	![[Pasted image 20240919121258.png]]