### Multiple ways of framing a problem
- One method: ![[Pasted image 20240829142904.png]]
	- This is a bad approach because whenever a new app is added, you might have to retrain your model from scratch, or at least retrain all the components of your model whose number of parameters depends on N (number of apps).
- Better method:![[Pasted image 20240829142934.png]]
	- In this new framing, whenever there’s a new app you want to consider recommending to a user, you simply need to use new inputs with this new app’s feature instead of having to retrain your model or part of your model from scratch.

### Decoupling objectives
- What if you have 2 objectives: quality and engagement?
	- Approach 1
		- loss = ɑ quality_loss + β engagement_loss
		- A problem with this approach is that each time you tune α and β—for example, if the quality of your users’ newsfeeds goes up but users’ engagement goes down, you might want to decrease α and increase β—you’ll have to retrain your model.
	- Approach 2
		- 2 models optimizing for each loss
		- You can combine the models’ outputs and rank posts by their combined scores: ɑ quality_score + β engagement_score
- In general, when there are multiple objectives, it’s a good idea to decouple them first because it makes model development and maintenance easier.