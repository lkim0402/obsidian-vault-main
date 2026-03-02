# Bulb
Given `N` bulbs, either on (1) or off (0).
Turning on `ith` bulb causes all remaining bulbs on the right to flip.
Find the min number of switches to turn all the bulbs on.

#### Naive solution
Logic:
```
if bulb == 1
	continue
else
	cost++
	flip the rest
```
- Time: `O(N*N)`
#### Optimized solution
```
[0,1,0,1,1,0,1,1]

```