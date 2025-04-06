
>[!CDN]
>A network of [[AWS Infrastructure (Regions, AZs, Edge locations)#Edge locations|edge locations]] that work together to cache and deliver content closer to users.
>- AWS [[CloudFront]]

- controls which content should be managed, takes care of routing customers to the closest [[AWS Infrastructure (Regions, AZs, Edge locations)#Edge locations|edge location]]
- Scenario
	- Say you have a website up and running in the USA. If you have a customer from Asia/India etc, the response will take longer, leading to higher latency. 
	- To fix this, we want to serve our website from different points in the world so that it's always close to our customers. 
	- But, starting multiple [[EC2|EC2 instances]] and then trying to route customers to the right instance that's close to them would be inefficient
	- So we use [[AWS Infrastructure (Regions, AZs, Edge locations)#Edge locations|edge locations]]