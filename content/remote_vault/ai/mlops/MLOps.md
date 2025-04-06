> Ops in MLOps comes from DevOps, short for Developments and Operations. To operationalize something means to bring it into production, which includes deploying, monitoring, and maintaining it. MLOps is a set of tools and best practices for bringing ML into production.

# Considerations
- [[When to use ML systems]]
- [[ML in Research VS in Production]]
- [[ML systems VS Traditional Software]]
# The Process
![[Pasted image 20240829134345.png]]
![[Pasted image 20240823122537.png]]
 - After the initial deployment, maintenance will often mean going back to perform more error analysis and maybe retrain the model, or it might mean taking the data you get back. 
 - Now that the system is deployed and is running on live data, and feeding that back into your dataset to then potentially update your data, retrain the model, and so on until you can put an updated model into deployment.
 - Example (절망편)
	 1. Choose a metric to optimize. For example, you might want to optimize for impressions—the number of times an ad is shown.
	 2. Collect data and obtain labels.
	 3. Engineer features.
	 4. Train models.
	 5. During error analysis, you realize that errors are caused by the wrong labels, so you relabel the data.
	 6. Train the model again.
	 7. During error analysis, you realize that your model always predicts that an ad shouldn’t be shown, and the reason is because 99.99% of the data you have have NEGATIVE labels (ads that shouldn’t be shown). So you have to collect more data of ads that should be shown.
	 8. Train the model again.
	 9. The model performs well on your existing test data, which is by now two months old. However, it performs poorly on the data from yesterday. Your model is now stale, so you need to update it on more recent data.
	10. Train the model again.
	11. Deploy the model.
	12. The model seems to be performing well, but then the businesspeople come
	knocking on your door asking why the revenue is decreasing. It turns out the
	ads are being shown, but few people click on them. So you want to change your
	model to optimize for ad click-through rate instead.
	13. Go to step 1.
### Steps
1. [[Scoping]]
	- [[Business metrics VS ML metrics]]
	- [[Requirements for ML systems]]
	- [[Framing ML Problems]]
2. [[Data Engineering]]
3. [[ML model development]]
4. [[Deployment]]
5. [[Monitoring and continual learning]]
6. [[Business analysis]]


- Some challenges
	- Concept drift/data drift
	- Just mode than machine learning code
		![[Pasted image 20240823122124.png]]
- The requirements surrounding ML infrastructure
	![[Pasted image 20240823122157.png]]



### Sources
- Designing Machine Learning Systems book: https://drive.google.com/file/d/13dmlGavqr5XtZijQuwfcHAc7cY4k4-l8/view?pli=1
- Andrew Ng's Machine Learning in Production Course