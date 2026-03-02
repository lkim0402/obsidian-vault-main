- We can refer to them as levels
- Add The scenes in Build Settings
	- `Ctrl + Shift + B` > `Add Open Scenes`
		![[Pasted image 20241031122826.png]]
		- We can see the indexes of the scenes
- SceneManager (class in Unity)
	- https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.html
### Project Boost
- When we hit an abstacle, we can "respawn" back to the starting by stopping the scene and loading the current scene again.
```C#
void NextScene(){
	// levelling up until the last scene (then we repeat)
	int nextLevelIndex = SceneManager.GetActiveScene().buildIndex + 1;
	int indexToLoad;

	if (nextLevelIndex == SceneManager.sceneCountInBuildSettings)
	{
		indexToLoad = 0;
	} 
	else {
		indexToLoad = nextLevelIndex;
	}
	SceneManager.LoadScene(indexToLoad);
}

void ReloadScene(){
	// Calling this function when we hit an obstacle 
	int currentIndex = SceneManager.GetActiveScene().buildIndex;
	SceneManager.LoadScene(currentIndex);
}
```


```C#
void NextSceneSequence() 
{
	// because this is set to true, even though OnCollisionEnter is 
	// called again it will just return
	isTransitioning = true;
	audioSource.Stop();

	// effects
	audioSource.PlayOneShot(success);
	successParticle.Play();

	GetComponent<Movement>().enabled = false;
	Invoke("LoadNextScene", delaySeconds);
}
void CrashSequence() 
{
	isTransitioning = true;
	audioSource.Stop();

	// effects
	audioSource.PlayOneShot(collision);
	crashParticle.Play();

	// "Movement" is a script I made for the Rocket
	GetComponent<Movement>().enabled = false;
	Invoke("ReloadScene", delaySeconds);

	// GetComponent<Movement>().enabled = true;
	// No need to include this line because the ReloadScene overwrites this
}
```

### Argon Assault
```C#
using UnityEngine.SceneManagement;

public class CollisionHandler : MonoBehaviour
{

    //Our ship is a trigger so we don't need OnCollisionEnter
    void OnTriggerEnter(Collider other)
    {
        // String interpolation
        Debug.Log($"{this.name} ** triggered by ** {other.gameObject.name}");

        //disable player movement and wait for 1 sec
        GetComponent<PlayerController>().enabled = false;
        Invoke("ResetScene", 1f);
    }

    void ResetScene(){
        int currentIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentIndex);
    }
}

```