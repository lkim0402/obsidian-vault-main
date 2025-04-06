복주머니 랜덤 위치
```C#
 // 새로운 랜덤 각도 생성
float randomAngle = Random.Range(0f, Mathf.PI * 2); // Full 360 degrees

// 랜덤 방향 설정 (cos: x, sin: z)
Vector3 randomDirection = new Vector3(Mathf.Cos(randomAngle), 0, Mathf.Sin(randomAngle)) * Random.Range(2.4f, 3 + spawnDistance);
// 2.4f랑 3은 그냥 설정한거

// y-axis 랜덤 설정
randomDirection += this.transform.up * Random.Range(-spawnRadius, spawnRadius);

Vector3 randomPosition = this.transform.position + randomDirection;

// Set the y-position to 0.5 (or any desired fixed height)
randomPosition.y = 5f;

```

GPS distance 값
```C#
public double getDistance(double lat1, double lon1, double lat2, double lon2, char unit)
	{
	// Return 0 if there is no distanec
	if ((lat1 == lat2) && (lon1 == lon2))
	{
		return 0;
	}
	else
	{
		// Getting the cosine of the central angle between the 2 locations
		double dist = 
			Math.Sin(deg2rad(lat1)) * Math.Sin(deg2rad(lat2)) 
			+ Math.Cos(deg2rad(lat1)) * Math.Cos(deg2rad(lat2)) * Math.Cos(deg2rad(lon1 - lon2));
		
		// Taking the arccosine (inverse cosine) to get the central angle in radians
		dist = Math.Acos(dist);
	
		// Convert the central angle from radians to degrees
		dist = rad2deg(dist);
	
		// Convert the distance from degrees to miles 
		// Earth's radius is 60 nautical miles per degree
		dist = dist * 60 * 1.1515;
		
		if (unit == 'K')
		{
			// miles to kilometers
			dist = dist * 1.609344;
		}
		else if (unit == 'N')
		{
			// miles to nautical miles
			dist = dist * 0.8684;
		} 
		else if (unit == 'M') 
		{
			// miles to meters
			dist = dist * 1609.34;
		} 
	
		// default is miles
		return dist; 
	}
```