### Common operations
- Vectors and matrices
	- In NumPy, vectors will be 1D (most of the time)
		- In other conventions a vector is treated as 2D
- Common operations in vectors and matrices
	- **Dot Product/Inner product**
		- $a \cdot b = a^T b = \sum^D_{d=1}a_db_d$
		- $a\cdot b = a_1b_1 + a_2b_2 + a_3b_3$
		- Takes 2 vectors, multiplies them element wise, then add those products together
		- Both input vectors must have the same shape
	- **Matrix multiplication**
		![[Pasted image 20240904154018.png]]
		- Just doing a bunch of mini dot products
		- Dot row 1 of $a$ and column 1 of $b$ and so on
		- Number of columns of a = Number of rows of b
			- Inner dimensions must match
	- **Element-wise product**
		![[Pasted image 20240904165243.png]]
		 - Not so common in linear algebra, but very common in ML
	 - We can solve linear systems (solving for `x`)
		 - $Ax=b$
	 - Inverse: $A^{-1}$
	 - Determinant: $|A|$
	 - Choosing random number/matrix from distribution (eg. Uniform, Gaussian)
 
### Applications of linear algebra (and numpy)
 - Linear regression
 - Logistic regression
 - Deep NNs (everything in deep learning)
 - K-meansclustering
 - density estimation
 - PCA
 - Matrix factorization (recommender systems)
 - SVM
 - Markov models
 - Control systems
 - game theory
 - operations research
 - portfolio optimization