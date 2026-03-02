
>[!definition]
>**Resource Analysis** is the specific activity of measuring the resources (like time and memory) that an algorithm uses. 
>- **Identify a function `f(n)`** which maps the algorithm’s input size to a measure of resources used
>- It's a **key component of Algorithm Analysis**, the entire field of studying algorithms

- When we're looking at algorithms, we want to know how does the **efficiency of that algorithm scale with the input size**
	- When comparing 2 algorithms, as the input size gets bigger and bigger, which one is going to become the better one?
- Why do resource analysis?
	- Allows us to compare algorithms, not implementations
		- If my implementation on my computer takes more time than your implementation on your computer, we still can’t conclude your algorithm is better
	- We can predict an algorithm’s running time before implementing
	- We can understand where the bottlenecks are in our algorithm (it makes us understand the algorithm more)
- We achieve this by creating a mathematical function `f(n)` that models this relationship
# `f(n)`
> If my input has size `n`, how much is the work/resource usage of my algorithm?
- In order to really evaluate how efficient an algorithm is, we need a unbiased, clear way to measure its efficiency
- Input (`n`) - The domain
	- `n` = **size** of the input
	- Ex) Number of characters in a string, number of items in a list, number of pixels in an image
- Output (`f(n)`) - The codomain
	- `f(n)` = **counts** of resources used
	- Ex) The output of our function represents a **count of the resources used** by the algorithm for an input of size `n`
	- Usually some discrete quantity
- `f(n) = n`
	-  indicates that the amount of work we have to do roughly matches the input size
# time vs memory
The "resources" we measure typically fall into two main categories: **Time** and **Space**.
- Important note: Make sure you know the “units” of your domain and codomain!
#### **Time Complexity (running time)**
- Unit of measurement  → **number of operations**
- Examples of operations:
	- Number of comparisons (e.g., x > y)
	- Number of additions or multiplications
#### **Space Complexity (running space)**
- Unit of measurement  → **memory**
- maximum number of bytes of memory the algorithm uses at any time

