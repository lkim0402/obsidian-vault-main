- Useful links
	- [AWS Pricing Calculator](https://calculator.aws/)
	- [AWS Pricing](https://aws.amazon.com/pricing/?aws-products-pricing.sort-by=item.additionalFields.productNameLowercase&aws-products-pricing.sort-order=asc&awsf.Free%20Tier%20Type=*all&awsf.tech-category=*all)
	- [AWS Pricing Free Tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
		- Great feature as AWS allows you to use many services for free
		- Free trials / 12 months free / Always free
		- Pretty much exists for almost all AWS services
		- allows new AWS users to experiment and use for free with limitations
- Variety of pricing and payment models (Diagram)
	![[Pasted image 20250315061131.png]]
## Cost management & budget 
- Monitor so that you're not paying for unexpected costs 
	- You should check on a regular basis
	- Account > - [Billing and Cost Management](https://us-east-1.console.aws.amazon.com/costmanagement/home?region=us-east-1#/)
- Check Bills tab
- Check Budgets tab
	- You can set a cost budget
	- Doesn't really limit your cost, but you can set an email so that once you're close to the budget it sends the email (alert)
- Check Cost Explorer tab
	- can group by region, etc
	- visualizes cost and usage data historically for up to 13 months
	- useful for analyzing cost from aws management console
	- can forecast future spend based on historical usage
- Check Service Quotas
	- Check different services that have certain limits
	- You can request for increase for limits (if possible) if you need
	- Search `service quota` in the search bar in the management console
- Check AWS Cost Anomaly Detection
	- uses ML to identify out-of-pattern spending for each service or usage dimension
- Check Cost & Usage Reports
	- the most detailed cost data feed in AWS
		- breaks down usage and cost by hour/day, service, usage type, region, or tags, producing large CSV or Parquet files in S3.
	- generating a report for you (generated on a regular basis)
	- CSV file to a [[S3 (Simple Storage Service)|S3]] bucket you specify
	- then you can use this result with other tools (ex. [[QuickSight]])
## AWS Support
- Diagram
	![[Pasted image 20250315070311.png]]
- AWS Support
	- The plan you chose when you first created your AWS Account
- Service Health Dashboard / Personal Health Dashboard
	- Notification icon > See all Health events
	- Shows problems w/ your current AWS services if any
	- Check to see if there is any internal problem with AWS itself
	- Service History
		- View the current and historical status of all AWS services world-wide!!
- Official Service Documentation
	- [doc link](https://docs.aws.amazon.com/?nc2=h_ql_doc_do)
## Tips
- regularly check your monthly bill and also consider using Budgets (with alerts)
- always check the pricing pages of a service before you start using it & consult the Free Tier page to find out whether a service is included
- Different services can also have different prices for different Regions. Therefore, you should make sure that you select an affordable Region in the top right corner.
## Tags
- Not a service
- Pieces of metadata that can be attached to your various resources
- All resources that you can create in your account can be tagged, like an [[ec2|EC2 instance]]
- helps in organize cloud resources
- can be used in the cost management tools to filter by tag
- useful for cost management
	- Tagging resources helps track cost drivers and identify who owns them
