![[Pasted image 20241003234822.png]]
- Movement vector
	- If we're applying 0, we're multiplying by 0 so we're not moving.
	- If we;re applying 1, we're multiplying by 1 so we're moving
- So our distance (offset) is our movement vector * movement factor

```C#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Oscillator : MonoBehaviour
{

    Vector3 startingPosition;
    [SerializeField] Vector3 movementVector; //(10,0,0)
    float movementFactor; //number between 0 and 1

    [SerializeField] float period = 2f; //the point where it repeats itself
    const float tau = Mathf.PI * 2; //6.28, or one full oscillation
     
    void Start()
    {
        // startingPosition = GetComponent<Transform>().position;
        startingPosition = transform.position;
    }

    void Update()
    {
		// if (period == 0) return;
        if (period <= Mathf.Epsilon) return;
        
        // increases linearly
        float cycle = Time.time / period;
        
        // Sin function expects radians
        // whatever is outputted by Sin is between -1 and 1
        float rawSinWave = Mathf.Sin(cycle * tau); // we get the number of full oscillations

        // make range be from 0 to 2, then divide by 2 (so the maximum is 1 and  minimum is 0)
        movementFactor  = (rawSinWave + 1f) / 2f; 
        
        Vector3 offset = movementVector * movementFactor;
        transform.position = startingPosition + offset;
    }
}

```
- `rawSinWave = Mathf.Sin(cycle * tau)`
	- By multiplying the number of cycles to `tau`, we get a range in between 
- `if (period == 0) return;`
	- for safety 
	- some versions of unity doesn't allow dividing by 0
- `if (period <= Mathf.Epsilon) return;`
	- Instead of checking for exact equality to zero, you check whether a number is smaller than or equal to Mathf.Epsilon to account for very small differences that may still behave like zero in practical terms
	- **In general don't compare flots to each other!**