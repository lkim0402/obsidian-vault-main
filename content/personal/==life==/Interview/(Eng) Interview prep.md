- In general there are 4 different types of questions
	- Programming, data science, behavioral, coding challenges

### Questions
1. [[Bias-Variance tradeoff]]
2. Difference between training set, validation set, test set
	- Training data is the actual data used to train the model itself and learn patterns. 
	- Validation data is used during the training process, and it is used to **tune hyperparameters**, select the best model architecture, and prevent overfitting. The model's performance on the validation set helps to decide when to stop training (e.g., early stopping) and which model configuration is optimal. Importantly, the validation data is used multiple times during the training phase as you tweak and refine your model. 
	- The test data is used only once to evaluate the final performance of the model.
3. What would you choose between a NN with 94% accuracy and a decision tree with 91% accuracy? (Interpretability vs Accuracy)
	- No right or wrong. 
	- Decision tree is easier to understand but the NN may perform better. It may be to better choose the decision tree for better explainability for clients.
4. How would you approach a problem with no labels?
	- They are testing you if you know unsupervised vs supervised learning
	- Answer depends on the industry
5. What is pep8?
	- Programming specific language question (this case python)
	- Most common python programming guideline (style of how you write python)
6. What is a [[confusion matrix]] (error matrix)?
7. What are some of the differences between [[Batch gradient descent]], [[Mini-batch gradient descent]], and [[Stochastic gradient descent]]?
	- Gradient descent is an optimization technique that is used to find the minimum of a loss function. It can be calculated by taking the derivative of the loss respect to the parameters of a particular algorithm.
	- They are different in how they divide the training set, performing the actual gradient, and performing actual updates (open the notes)
	- mini batch: Because of memory, people divide into batches so it can fit into RAM
	- stochastic: When you don't want your data to be order specific
8. What is the importance of the preprocessing steps [[feature scaling (normalization)]] in machine learning?
	- Often features have different scales of magnitude, so the derivatives of the loss with respect to those input parameters will be on different scales as well. Then, gradient descent becomes unstable. 
9. What are the differences between [[Classification]] and [[Regression]] in machine learning?
	- They are both predicted by a supervised ml algorithm.
	- Classification predicts a category, regression predicts a numerical/continuous value
	- Can it be both?
		- If the outcome is a number, you could use regression, but you could also bin those outcomes into different categories
		- Like in the case of height you can bin them based on ranges
10. What models can you use for [[Classification]]?
	- PCA
11. How can you tell if a model needs to be refreshed?
	- When there is a degrading in the performance of the algorithm. Generally you'll benchmark the performance, and at some point your data and production will not match
	- No one best way, case by case, domain specific
12. Cases of model performance differing in production vs deployment
	- [[Deployment|Concept drift]]: Relationship between the input features and the actual outcome variable changes
13. How would you handle exploding gradient?
	- One way is to clip the gradients at a certain threshold (brute force)
	- Batch normalization: normalization after a layer/activation, this helps scale the gradients to a more reasonable and stable values
	- Change architecture to mitigate (reduce number of layers -> reduce multiplications of chain rule)

**Easy questions**
1. What are the different types of ML? Explain each.
	- [[Supervised Learning]]
	- [[Unsupervised Learning]]
	- [[Reinforcement Learning]]
2. What is a [[model]] in ML?
3. What is [[Overfitting]] and [[underfitting]]
4. What is [[Cross-Validation (교차검증)]]?

**Intermediate**
1. What is  ROC curve?
2. What is precision and recall?
3. What is the F1 score?
4. What is regularization
5. What is feature engineering?
6. What is gradient descent?
7. Difference between bagging and boosting?
8. What is a decision tree?
9. What is random forest?
**Advanced**
1. What is SVM?
2. What is PCA?
3. What is CNN?
4. What is RNN?
5. What is droupout in NNs?
6. What is transfer learning?
7. What is GAN?
8. 

- Anomaly detection
- Grid search
- SQL questions
- Behavioral questions

### Tips
- REALLY PREPARE FOR BEHAVIORIAL
	- 3-4 stories, especially at work position (STAR format)
- prepare introduction section
### Resources
- https://www.youtube.com/watch?v=n8NgELVMRS0
- https://www.youtube.com/watch?v=7tLMslk1Zm8
- https://www.youtube.com/watch?v=Ghvahod4op8
	- PCA
	- Logistic regression
	- Clustering
	- K-nearest algorithm
	- When to stop training?
	- Decision tree
	- Ensemble training
	- boosting, bagging
	- overfitting, how to prevent
	- how to deal with unbalanced datasets
	- L1 and L2 norm
- cheatsheet
	- https://stanford.edu/~shervine/teaching/cs-229/cheatsheet-machine-learning-tips-and-tricks
- Demo interview
	- https://www.practiceml.co/demo