- Each object have tags
```C#
private void OnCollisionEnter (Collision other) {
	if (other.gameObject.tag != "Hit") {
		hits++;
		Debug.Log("Current number of bumps: " + hits);
	}
}
```