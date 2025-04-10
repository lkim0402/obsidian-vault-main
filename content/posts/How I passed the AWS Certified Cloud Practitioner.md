One of my friends, pursuing to be a cloud engineer, was prepping for the AWS Solutions Architect certificate. That was actually enough for me to get interested. I've heard vaguely about EC2 instances, Fargate, etc, but I never knew them well enough to actually use / talk about them. After it piqued my interest, I researched more about cloud and realize the importance of cloud computing. The more I read articles about cloud, I began to understand that it's actually an advantage for developers to learn cloud computing.
(get trend)
I wanted to get a step into the world of cloud, and start with the basics. Many people online mentioned to just skip the CCP and go for the Solution Architect role right away, but I was certain that I wouldn't survive it as I didn't even know the basics of the AWS core services. I prepped for around ~2 weeks for the AWS Cloud Practitioner exam, took it, and passed. I learned a TON in the process.

Here are the the materials I used to prep:
- [Udemy course - AWS Certified Cloud Practitioner CLF-C02](https://www.udemy.com/course/aws-cloud-practitioner-complete-aws-introduction/?couponCode=ST15MT20425G1) - Unfortunately outdated, but I had access to this course for free so I just decided to go with it. But still, the majority of services mentioned have been relevant, and there is also 1 free practice test along with it.
- [[2025 Practice Exam]AWS Certified Cloud Practitioner CLF-C02](https://www.udemy.com/course/practice-exam-aws-certified-cloud-practitioner-clf-c01/?couponCode=ST15MT20425G1) - Got access to this free sets of practice exams. Many questions were repetitive, but regardless gave me a good idea and the scope of the exam.
- [AWS Certified Cloud Practitioner Certification Course (CLF-C02) - Pass the Exam!](https://youtu.be/NhDYbskXRgc?si=llbtaBDcU5G4xJAe) - Watched / skimmed in 2x speed after the udemy course.
# Tips

#### Drawing diagrams
(insert iPad picture)
Because AWS services have lots of interdependencies & relationships, personally, I found drawing diagrams and connections really helpful in reinforcing the core concepts and ideas. For example, drawing the structure of VPC & subnets with NAT Gateways helped the concepts to stick better.
#### Obsidian
This is kind of related to the previous tip, but Obsidian immensely helped me with reinforcing core services thanks to the linking feature. It helped me to backtrack related concepts/links, and form connections. I'd recommend using Obsidian for almost any learning subject really, personally I find it extremely useful.
- (picture of notes, graph)
#### Using AI
(picture of GPT)
I used Chat GPT to reinforce a lot of concepts, especially those that were confusing for me. I used it especially to ask about similar services, for example CloudTrail VS GuardDuty or EBS VS EFS. Also, I found it very helpful to ask it different scenarios of fictional companies that pick different pricings for core services like EC2 or S3. 
#### Practice Exams
Classic (and obvious) tip. There are tons of resources out there, for example this [github repo](https://github.com/kananinirav/AWS-Certified-Cloud-Practitioner-Notes/blob/master/practice-exam/exams.md) that contains a lot of practice exams. Unfortunately I knew this resource existed after I took this exam, but it seems to have good reviews.
#### Testing your environment
Okay so, this normally shouldn't be a problem, but even though I had tested my environment the day before I still had tons of issues. For example, the testing system (OnVUE) requires that there should be no running tasks in the background, but on my laptop, there were start up applications that randomly started during check-in. I had to manually disable all of them and restart the whole process, which wasted about 30 minutes of my time.
#### Other useful links
- [A overview whitepaper from AWS]( [https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html)) 
- [Core Cloud Concepts](https://aws.amazon.com/getting-started/cloud-essentials/)
- [AWS Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
- [AWS Benefits](https://aws.amazon.com/application-hosting/benefits/)
- [Cloud Computing Advantages](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)
- [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&wa-lens-whitepapers.sort-order=desc)
# What's next?
I'm hoping to dive deeper into more hands-on projects with AWS, more likely a personal project. The AWS CPP exam certainly helped me to gain the confidence to use AWS services more efficiently. 
(badge/certification picture)
Also, I know CPP is just the beginning, but studying cloud computing unexpectedly forced me to think in terms of components and scalability, and modularity. Thinking about connecting different AWS services made me grasp the concept of flow more clearly, like how data moves between services (like API Gateway -> Lambda -> DynamoDB). Now, I have some questions I would ask more often when doing projects:
- What are the modular components?
- How do they interact?
- What are the bottlenecks?

Ironically learning cloud computing & architecture has made me think better. It got me interested in designing systems, which I think is a good stepping stone for the next cert Solutions Architect, which I am considering to take :D

If you're planning to take this certificate, I wish you luck! 
Thanks for reading! 
