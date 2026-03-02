```C#
using System.Collections;
using System.Collections.Generic;
// using System.Numerics;
using JetBrains.Annotations;
using UnityEngine;

public class Fallslowly : MonoBehaviour
{
    public Rigidbody rb;
    public float gravityScaling = 0.1f;
    public float upScaling = 3f;
    public float forwardForce = 0.38f;
    void Start()
    {
        // Setting slow gravity
        Physics.gravity = new Vector3(0, -9.81f * gravityScaling, 0);

        // Setting the forces
        StartCoroutine(ApplyForceDelay());
    }

    IEnumerator ApplyForceDelay()
    {
        // Upwards
        Vector3 upDirection = new Vector3(0, upScaling, 0);
        rb.AddForce(upDirection, ForceMode.Impulse);

        yield return new WaitForSeconds(1f);

        // Random towards player
        // float xRange = Random.Range(-1f, 1f);
        Vector3 directionToPlayer = new Vector3(-1, 0, -forwardForce);
        rb.AddForce(directionToPlayer, ForceMode.Impulse);

        //optional?
        // rb.AddForce(new Vector3(0, upScaling * 0.5f, 0), ForceMode.Impulse);

    }
}

```