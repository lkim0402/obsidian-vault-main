> Predicting a number from infinitely many possible outputs
- How do get an algorithm to systematically choose the most appropriate line/curve/ or anything to fit to the data
- Two types
	- Linear Regression
	- Non-linear Regression
## Linear Regression
> Fitting a straight line into your data
#### How it works
- Steps
	- Feed your training set to your supervised learning algorithm
	- Your algorithm will produce some function `f`: a model
	- `f` takes new input feature $x$ and produces an estimate/prediction $\hat{y}$
		- $\hat{y}$ may or may not be the actual true value $y$ (the output variable/"target") for the training example
		- ex) If you're helping the client to sell the house, the true price of the house is unknown until you sell it
- Representing `f`
	- Univariate linear regression
		- linear regression w/ 1 input variable $x$
		- $f_{w,b}(x) = wx + b$
		- $f(x)=wx+b$
- Examples: Housing Price Prediction
		![[Pasted image 20241108114115.png]]
	- This is a linear regression algorithm in supervised learning
		- This algorithm fits a straight line. When your friends ask you the price for house size `750 ft^2`, the algorithm gives you `150k`.
- Another way to look at the data: tables!
		![[Pasted image 20241108134038.png]]

#### More on Parameters
- The line "fits" differently based on $w$ and $b$
	![[Pasted image 20241108152400.png|550]]
	- The value of $w$ gives you the slope of the line
- How do you find values for $w$ and $b$ so that $\hat{y}^{(i)}$ is close to $y^{(i)}$ for all $(x^{(i)}, y^{(i)})$?
	![[Pasted image 20241108153754.png]]
	- There is a gap between $y^{(i)}$ and $\hat{y}^{(i)}$
 - Use a cost function!
#### Cost function
> Measures how well a line fits the training data

- Squared error cost function
	$$ \begin{align} J(w,b) &= \frac{1}{2m}\sum_{i=1}^{m}(\hat{y}^{(i)}-y^{(i)})^2\\
	&=\frac{1}{2m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})^2 & \text{since } \hat{y}^{(i)}=f_{w,b}(x^{(i)})
	 \end{align}$$
	- $m$ = the number of training examples
	- $\frac{1}{m}$ to get the *average* squared error
		- $\frac{1}{2m}$ for convention in ml 
	- the most commonly used cost function for linear regression
- Mean absolute error (MAE)
- Mean squared error (MSE)
- Root mean squared error (RMSE)
- Linear regression tries to find values for $w$ and $b$ that makes $J(w,b)$ as small as possible!
	- Goal: $\text{minimize}_{w,b}\, J(w,b)$

#### Visualization 1: $f_w(x)=wx$
> We will set $b=0$ to better understand intuitively
- function: $f_w(x)=wx$
- parameter: $w$
- cost function: $J(w,b) = \frac{1}{2m}\sum_{i=1}^{m}(f_{w}(x^{(i)})-y^{(i)})^2$
- goal: $\text{minimize}_{w}\, J(w)$
- Example $w=1$
	![[Pasted image 20241108162830.png|500]]
	- When $w=1$ then $J(w) = 0$ 
	- $w$ becomes a parameter in $J(w)$
- Example $w=0.5$
	![[Pasted image 20241108163527.png|500]]
- Example $w=0$
	![[Pasted image 20241108163713.png|500]]
- For lots of different values for $w$, you can get how the cost function $J$ looks like!
	![[Pasted image 20241108163833.png]]
	- Each value of $w$ corresponds to a different straight line fit

#### Visualization 2: $f_{w,b}(x)=wx+b$
> We will set $b=0$ to better understand intuitively
- function: $f_{w,b}(x)=wx+b$
- parameter: $w$
- cost function: $J(w,b) = \frac{1}{2m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})^2$
- goal: $\text{minimize}_{w,b}\, J(w,b)$
- The fact that the cost function squares the loss ensures that the 'error surface' is convex like a soup bowl. It will always have a minimum that can be reached by following the gradient in all dimensions.
- The cost function visualization now:
	![[Pasted image 20241108174135.png|400]]
	- Also curved under, except in 3 dimensions!
	- It's a 3D surface plot where the axes are labeled $w$ and $b$
	- As you vary the parameters in the cost function ($w$ and $b$), you get different values in the surface function!
		- You get the height
			![[Pasted image 20241108174430.png|500]]
- Using contour plot
	- A topological map shows high different mountains are. The contours are basically the horizontal slices of the mountain 
		![[Pasted image 20241108174856.png|400]]
		![[Pasted image 20241108174921.png|400]]
	- Contour plot of the cost function
		![[Pasted image 20241108175533.png]]
		- Bottom
			- The same bowl, just veeeeery stretched
		- Up right - contour plot of cost function
			- each axis is $b$ and $w$
			- each ovals (ellipses) shows the center points on the 3D surface which are at the exact same height, so the same value for $J_{w,b}$ 
			- You "slice" the 3D surface plot horizontally
			- contour plots are convenient to visualize the 3D cost function $J$
- Examples
	- Example $f_{w,b}(x)=-0.15x+800$
		![[Pasted image 20241108180508.png]]
		- You can see that the cost function value is far from the minima (the smallest circle)
		- It's generall not a good fit lol
	- Example $f_{w,b}(x)=0x+360$
		![[Pasted image 20241108180632.png]]
	- Example $f_{w,b}(x)=0.13x+71$
		![[Pasted image 20241108180813.png]]
		- Manually looking at the contour map to find the best parameters is not recommended (also it will get more and more complex with  complex algorithms)
		- Instead write code that automatically finds the best parameter values
			- Gradient descent!!!
#### Code (Google Colab)
- [Model representation](https://colab.research.google.com/drive/1I1_7PMqCEDaGV-xwKDft4yRt7NN2AaEc?usp=sharing)
- [Cost function](https://colab.research.google.com/drive/1WDkngwqlokAFOpirPWQZI1ZSH9SJz6Se?usp=sharing)
## Non-linear regression
> Fitting a *curve* (more complex than straight line)
