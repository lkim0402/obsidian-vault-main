- [[NumPy]]
### Basic random functions
```python
# Creates a 2x3 array of random numbers between 0 and 1
np.random.rand(2,3) 

# Creates a 2x3 array of random numbers from a standard normal distribution
# [0, 1)
np.random.randn(2, 3) 
np.random.randn(3) #creates 1D array with 3 elements

# # Creates a 3x4 array of random integers between 0 and 10
np.random.randint(0, 10, size=(3, 4))
```
 - `np.random.rand()`
	 - Generates random numbers from a uniform distribution over `[0, 1)` for the given shape (dimensions).
 - `np.random.randn()`
	- `np.random.randn()` generates random numbers **drawn from the [[standard normal distribution]]**, meaning the numbers will be centered around 0, with a standard deviation of 1.
	- The function `np.random.randn(100, 2)` will generate a 2D array with 100 rows and 2 columns. Each value in this array is a random number drawn from the standard normal distribution.
- `np.random.randint(low,high=None,size=None)`
	- Generates random integers from the discrete uniform distribution in the specified range `[low, high)`.
### Distributions
```python
"""Continuous (generate floats)"""
## Creates a 3x4 array of random numbers between 0 and 10
np.random.uniform(0,10, size=(3,4))

# Creates a 2x3 array of random numbers from a normal distribution with mean 0 and std deviation 1
np.random.normal(0, 1, size=(2, 3))  

"""Discrete (generate ints)"""
# Simulates 100 trials of 10 coin flips with a probability of 0.5 for heads
np.random.binomial(10, 0.5, size=100)  

# Generates 100 samples from a Poisson distribution with an average of 5 events
np.random.poisson(5, size=100)  

```
- `np.random.uniform(low=0.0, high=1.0, size=None)`
	- Generates random numbers from a uniform distribution over the interval `[low, high)`
- `np.random.normal(loc=0.0, scale=1.0,size=None)`
	- Generates random numbers from a normal (Gaussian) distribution with mean `loc` and standard deviation `scale`