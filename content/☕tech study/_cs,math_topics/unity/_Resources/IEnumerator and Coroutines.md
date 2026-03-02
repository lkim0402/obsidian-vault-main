- `Coroutine`
	- special methods in Unity that allow you to pause execution and resume later
	- useful when you want to delay actions or spread them out over multiple frames without blocking the game loop
		- ex) performing actions over time but don't need them to happen every frame.
	- unlike `Update()`, coroutines are not tied to every frame's execution
		- They execute until they hit a `yield` statement and then resume after the condition is met (e.g., time delay, or waiting for a condition).
	- written with the `IEnumerator` return type
	- No need to explicitly stop a coroutine as it stops at the end of the function (unless you want to end it early/cancel it)
	- Can't return data
- Stopping a coroutine
	- `yield break;`
	- `StopCoroutine(myCoroutine);`  -> outside of `myCoroutine`
### Update vs Coroutine
- Update
	![[Pasted image 20240925230609.png|300]]
- Coroutine
	![[Pasted image 20240925230635.png|300]]
	- Allows you to execute a game logic *over a number of frames*

### `yield` statements
```C#
yield return null
yield return new WaitForSeconds(1f);
yield return new waitForEndOfFrame(); // pauses the execution of a coroutine until **the end of the current frame
yield return new WaitUntil(another_function());
yield return StartCoroutine(OtherCoroutine()); // another coroutine

yield break; //ends it on that line
```

### Example scenarios

|                         | Update                                             | Coroutine                                                         |
| ----------------------- | -------------------------------------------------- | ----------------------------------------------------------------- |
| Stamina regeneration    | stamina bar decreases as player sprints or attacks | player has to wait few secs after doing a special attack          |
| Health regeneration     | health decreases as player is attacked             | player has to wait for few secs until health starts to regenerate |
| Enemy movement + attack | Enemy constantly moves toward the player           | Enemy switches between 3 attack forms every 2 secs                |

### Example code
> Player has a 1-sec cooldown after double jump
```C#

public class Player : MonoBehaviour
{
    public int jumpCount = 0;
    public int maxJumps = 2;  // Allow double jumps (2 jumps)
    private bool canJump = true;  // Controls whether the player can jump
    private bool isGrounded = false;  // Check if the player is on the ground

    void Update()
    {
        // Allow the player to jump only if they haven't reached max jumps and are allowed to jump
        if (Input.GetKeyDown(KeyCode.Space) && canJump && jumpCount < maxJumps)
        {
            Jump();
            jumpCount++;
            
            // If player has used up all jumps, start cooldown
            if (jumpCount == maxJumps)
            {
                StartCoroutine(JumpCooldown());
            }
        }

        // Reset jump count when the player lands on the ground
        if (isGrounded)
        {
            jumpCount = 0;  // Reset jumps
        }
    }

    void Jump()
    {
        // Implement actual jump logic (e.g., apply upward force)
        Debug.Log("Player jumped!");
    }

    IEnumerator JumpCooldown()
    {
        canJump = false;  // Disable jumping
        Debug.Log("Jump cooldown started");

        // Wait for the cooldown period (1 second)
        yield return new WaitForSeconds(1f);

        Debug.Log("Jump cooldown ended");
        canJump = true;  // Allow jumping again
    }

    // Simulate landing on the ground
    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.tag == "Ground")
        {
            isGrounded = true;  // Set grounded state when player hits the ground
        }
    }

    private void OnCollisionExit(Collision collision)
    {
        if (collision.gameObject.tag == "Ground")
        {
            isGrounded = false;  // Set grounded state to false when player leaves the ground
        }
    }
}

```

