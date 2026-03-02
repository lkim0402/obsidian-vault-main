- `Right click > Effects > Particle system`
	- It can also be added to a Game Object as a component
- We use `Modules` to control behavior (under particle system)
	- The renderer contains the material
- You can loop or play on awake (depends on your needs)

use case
- We have the particle effects attached on the rocket, and have it turned on based on the things we collided into

```C#
[SerializeField] ParticleSystem successParticle;
[SerializeField] ParticleSystem crashParticle;

void OnCollisionEnter(Collision other)
{
	crashParticle.Play();
}
```

### Laser beam (Argon Assault)
![[Pasted image 20241029134052.png]]
![[Pasted image 20241029134100.png]]

- Change Simulation space to world so that the laser can shoot off to a wider area
	![[Pasted image 20241029135151.png]]
- Adjust rate over time to shoot more over time
	![[Pasted image 20241029141016.png]]
- Just generally tweak the settings 
	![[Pasted image 20241030112155.png]]
	- Start Lifetime - how long each individual particle will exist before disappearing
		- Start Lifetime = 1 -> each particle disappears after 1 second
	- Start Speed - how fast particles move when they're first emitted
		- higher values make particles shoot out faster
	![[Pasted image 20241030112204.png]]
	- Lifetime
- If you want the trails only and not the particles, then
	- Under `Trails` turn off `Size affects width`
	- Under `Particle System` set `Start Size = 0`

#### Using the laser as bullets
1. Ensure particle **collision** and **message** **sending** is on
	- Looks like this
		![[Pasted image 20241031114839.png]]
		- Making the `Type` to World allows the lasers to bounce
		- If you want the lasers to not bounce, set `Min Kill Speed` to 1
2. Add a non-trigger collider to the target (enemy/obstacle)
3. Use `OnParticleCollision()` on the target

```C#
public class DestroyEnemy : MonoBehaviour
{
    // Start is called before the first frame update
    void OnParticleCollision(GameObject other)
    {
        // Debug.Log("Particle hit: " + other.name);
        if (other.CompareTag("enemy"))
        {
            Debug.Log("Im Here!");
            Destroy(other);
        }
    }
}
```
- Make sure that this script is attached to where the particlesystem is

### Explosion particle
- Change to `Particles > Standard Unlit` so that it won't use the legacy one 
	![[Pasted image 20241031133553.png]]
- Change shape to `Cone`
	![[Pasted image 20241031134013.png]]
- Make` Particle System > Duration` between 0.3 and 1 (so explosion is quick) and turn off looping
- `Particle System > Start color` -> explore with diff colors
- You can add `Trails` to make it look more like an explosion
- Adding some randomness
	- Select the `Noise` module

### Instantiating particles and destroying them
- Basically just destroy gameobjects only if they're manually instantiated like this
```C#
public class DestroyEnemy : MonoBehaviour
{
    [SerializeField] GameObject deathParticles;
    void OnParticleCollision(GameObject other)
    {
        // Debug.Log("Particle hit: " + other.name);
        if (other.CompareTag("enemy"))
        {
            Debug.Log("Im Here!");

            // Instantiate and get reference to the new particles
            GameObject newParticles = Instantiate(deathParticles, other.transform.position, Quaternion.identity);
            // Explicitly play the particle system
            newParticles.GetComponent<ParticleSystem>().Play();
            
            // get particle system duration
            float duration = newParticles.GetComponent<ParticleSystem>().main.duration;
            
            Destroy(newParticles, duration + 3f);
            Destroy(other);
        }
    }
}
```
- If "Play on Awake" is clicked, you don't even need the `.Play()` part because it will already be played when instanstiated!