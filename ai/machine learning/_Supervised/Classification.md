> Predicts categories (numeric or non numeric)
#### Classification: Breast cancer detection
- One input
	![[Pasted image 20241108115322.png|500]]
	- We only predict a finite set of outputs/categories/classes (in this case, `0` or `1`)
	- If I have a data in that `?`, would it be benign (`0`) or malignant (`1`)?
- 2 or more inputs
	- You can use more than 1 input value to predict the output
	- Example: `Age` and `tumor size`
		![[Pasted image 20241108115806.png|500]]
	- The learning algorithm might find some boundary that separates the benign and malignant -> it has to decide how to fit a boundary line through this data
	- So in out case with the yellow patient, the tumor is likely to be benign

Classification algorithms
- Logistic regression
- Decision tree/random forest
- neural networks
- naive bayes
- k-nearest neighbors
- support vector machines