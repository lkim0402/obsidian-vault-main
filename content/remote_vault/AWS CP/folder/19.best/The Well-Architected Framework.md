
>[!Definition]
>- Gives you a framework for looking at your cloud architecture and identifying ways of improving it
>- Developed by AWS, continuously improving
>
- [amazon docs](https://aws.amazon.com/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&wa-lens-whitepapers.sort-order=desc&wa-guidance-whitepapers.sort-by=item.additionalFields.sortDate&wa-guidance-whitepapers.sort-order=desc)
# Six pillars
- Helps you analyze your cloud infrastructure with these 6 pillars
- As you work with the cloud, you will work with these pillars time to time and see if there is room for improvement
#### Operational Excellence
- Make sure workloads & the cloud run effectively
- Gain insights (monitoring) & continuously improve processes
	- [[Monitoring workloads|Monitoring is important]]
- Want to be able to measure whether new solution is better than old solution
- Using [[CloudFormation & CDK#CloudFormation|CloudFormation]] for describing your cloud architecture instead of manually creating all resources in the management console 
#### Security
- Use cloud technologies to make sure everything is secure - protect accounts, data, workloads
- [[Security matters]], [[Permissions & Access Control (IAM)]]
#### Reliability
- Ensure consistent workload functionality
	- Ex) webserver should be up and running even if demand increases
- Workloads should function under different circumstances
	- [[EC2 Auto Scaling]], [[Elastic Load Balancer (ELB)]], [[Lambda]], etc
#### Performance Efficiency
- Use computing resources efficiently
- Embrace & utilize demand and technology changes
	- don't be scared to switch to another service if it's better
#### Cost Optimization
- Optimize cost & keep track of spending - minimize costs
- Always challenge whether you really need to use a service
- See if there are other alternatives
#### Sustainability
- Optimize energy consumption & improving efficiency
- No wasting energy, see if there's a more sustainable alternative