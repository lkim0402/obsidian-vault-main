# AI (short)
#### What is RAG in Gen AI?
- Bad answer
	- RAG stands for Retrieval-Augmented Generation, which is a technique where a language model retrieves relevant info from external sources before generating a response. This helps the model provide more accurate answers, especially when it hasn't been trained on the latest data
	- this is *bare minimum stuff* - not show knowledge of SA
- Good answer
	- RAG stands for Retrieval-Augmented Generation. It's a technique that enhances a Large Language Model's (LLM) responses by grounding them in external, up-to-date, or proprietary knowledge.
	- Here is an example of how it works:
		1. **User Query:** A user asks the LLM a question, which the user query, to a chatbot service that a company made to help customers query specific information about their products.
		2. **Retrieve:** Instead of going directly to the LLM, the query first searches a private knowledge base (like a vector database, e.g., Amazon Kendra or Amazon OpenSearch, where the data is stored as embeddings or vectors) for relevant information. 
		3. **Augment:** The original query is combined with the retrieved context.
		4. **Generate:** This "augmented query" is then sent to the LLM (e.g., hosted on Amazon Bedrock), which generates a more accurate and context-aware answer.
#### What are some Gen AI use cases?
- Average answer
	- Chatbot, ChatGPT, Gen AI Coding
	- This is outdated lol, no association with enterprise use cases, no latest enhancements
		- everyone knows about this lmao
- Good answer
	- Generative AI use cases in the enterprise can be grouped into three main categories:
	- Enhancing customer experience
		- **Conversational AI:** Building intelligent chatbots and virtual assistants for instant customer support.
	    - **Agent Assistance:** Providing real-time suggestions and information to human support agents during calls.    
	    - **Personalization:** Generating personalized marketing copy, product recommendations, and user experiences.
	- Improve Employee productivity
		- **Content & Document Processing:** Summarizing long reports, generating drafts of documents, and extracting key information.
	    - **Code Generation:** Assisting developers by writing, documenting, and debugging code (e.g., Amazon CodeWhisperer), allowing devs to increase productivity by 10x or more  
	    - **Data Analysis/Creating metrics:** Allowing users to query complex datasets and generate business intelligence insights using natural language (e.g., Amazon Q in QuickSight).
	- Finally, there are some cutting edge cases that I personally have been having fun with
		- Exploring MCP (model context protocol)
#### What are you doing with Gen AI?
- BAD
	- Gen AI is mostly hype. I have used ChatGPT but ~~~ (lol)
- Good
	- I'm actively exploring Gen AI both personally and for practical applications. For daily tasks, I use tools like ChatGPT to help streamline research and learning.
	- More formally, I am deeply studying **Retrieval-Augmented Generation (RAG)** architecture. As part of Makeability Lab's project SonoCraftAR, I am exploring how to implement a RAG-based solution to build a specialized AI Agent, vectorizing documentation of a specific library to test if the results are more accurate. 
#### Give me an example of how you might use ML to enhance a product or bring benefit to a company.
- A classic and highly effective use of Machine Learning in e-commerce is creating a *product recommendation system*, similar to Amazon's "Customers who bought this also bought..." feature.
	-  **Collaborative Filtering Model:** This model doesn't need to know anything about the products themselves. Instead, it *analyzes user behavior*. It identifies customers with similar purchasing patterns and recommends products that "similar" shoppers have bought. For example, if Customer A and Customer B both buy a specific brand of coffee and a coffee grinder, but Customer B also buys a milk frother, the system will recommend the milk frother to Customer A.
	-  **Content-Based Filtering Model:** This model focuses on product attributes. It recommends items that are similar to what a user has *previously purchased or viewed*. For example, if a customer frequently buys science fiction books by a certain author, the system will recommend other sci-fi books, particularly from that same author or with similar themes.
# CS

#### What is the difference between a compiled and interpreted language ?
#### Tell me different git commands?

#### Describe a three tier web application/architecture? ⭐
- Average
	- Presentation layer is the Frontend, Next is the Application layer which is the backend, and the storage layer which is the database
	- EVERYONE says this lol
	- Show how they are actually implemented in the world
- Good
	- A three-tier architecture separates an application into three logical and physical computing tiers:
	1. **Presentation Tier (Frontend):** The first layer is the *Presentation tier*, which is the is the frontend where customers interact with. An example is the website `amazon.com`, where you can browse different products, and its tech stack can be React or Angular. 
	2. **Application Tier (Backend/Logic):** This is the "brain" of the application where the business logic is processed. It handles user requests from the presentation tier, and an example could be adding an item to a cart for payment. The tech stack in this layer can be  Java, NodeJS, Python, etc.
	3. **Data Tier (Database):** This tier stores and manages the application's data. It's responsible for data persistence and retrieval. An example could be storing user profiles or order history, and the tech stack can vary from MySQL or NoSQL databases like Amazon DynamoDB. 
	- A sample design in AWS could be
		- Design the frontend using elastic load balancing with amazon EC2 running the webserver. Then the backend could be using elastic load balancing with amazon ec2 running the business logic. Then the database could be implemented using DynamoDB or RDS, whichever one fits your application more. 
		- >>> You can add how the groups of EC2s can be more scalable (auto scaling groups)
		- Diagram
			![[3-tier.png|400]]
#### What is EDA?
- Avg
	- EDA is event driven architecture. One such example is messages going to SQS and lambda processing them
	- most projects always synchronous architecture...
	- it takes some serious consideration and work to go to EDA
	- you NEED TO MENTION THE SUPERPOWERS OF EDA
- Good
	- Synchronous architecture
		- some challenges are that all components of synchronous architectures MUST scale together, and consumer needs to resend transaction for re-processing, and it's expensive
		- diagram
			![[synchronous.png]]
	- Event-Driven/Asynchronous architecture
		- You decouple the consumer and producer of the messages.
		- API Gateway -> SQS -> Lambda -> amazon dynamodb
		- More messages could come in, API Gateway can scale up, put all msgs in SQS, then the consumer/processor of the messages (lambda and dynamodb) can consume them at a rate that they can handle
		- Advantages
			- message producer and processor can scale independently
			- retry built in
			- cost effective than synchronous architecture
		- diagram
#### How do you scale your application for a big traffic day? ⭐
# CI/CD and Docker
#### What is CI/CD

#### What is the difference between CICD and GitOps?
- Bad answer traits
	- NO CLEAR UNDERSTANDING OF CI AND CD (they just clump them together)
	- CICD is NOT fully replaced by GitOps
- Good (give actual use case)
	- In traditional flow, you will push code and docker file to a git repo, which will trigger the CI tool like Jenkins and create container image. Then the CD tool will get triggered and will update the kubernetes manifest with the new container image tag. Then The CD tool will deploy this manifest file to a kubernetes cluster like Amazon EKS, then the comtainer image will be deployed to your kubernetes cluster
	- WIth GitOps, the CI parts stays the same. The part that changes is the CD part. The CD tool will update the manifest with the container image tag. GitOps tool is installed in the cluster. It constantly reconciles between the version running in cluster vs the one running in git.
#### Difference between docker and k8s
#### What are containers?
#### Why containers? How is it different from VM?
- BAD answer
	- Containers are scalable, lightweight, secure, and cost efficient with better resource utilization than VM. YOu don't need to manage infrastructure when you run containers. Kubernetes is becoming very popular because of these reasons.
	- DON"T SHIT ON VM LOL
- Good answer
	- Containers are very popular these days. Some of the biggest reasons are containers don't have host OS, so it's lightweight. And since the code, dependencies, and libraries are all packaged in the container, they are very portable. They can run in multiple clouds, on-prem, and even on the edge. 
	- However, container and VMs go hand in hand. For example, the most popular container orchestrator Kubernetes, the worker nodes actually run in VMs whereas the containers run within pods inside those VMs. Some latest CNCF tool, such as Karpenter helps run containers in VMs in cost optimized way.
		- Explain the why. Containers and VMs go hand in hand. Give an example.
			- *why is it lightweight and portable?*
# Networking
#### HTTP VS HTTPS

#### Process of TLS handshake
- 비대칭키의 역할

#### Cache VS cookies
#### What is ***OSI model*** and briefly explain what each layer does / What is application layer of OSI and their signification?
- OSI (Open System Interconnection) is a reference model that defines how applications can communicate with each other over a networking system.
- It has 7 layers. If you start from the top, the acronym is All People Should Try New Dominoz Pizza, which comes from Application, Presentation, Sesion, Transport, Network, Data Link, Physical
- The Application layer is closest to end user, its where the layer interactions happens with the software applications (protocols include HTTP and SMTP).
- Presentation layer ensures that whatever data is coming is coming gets converted to human readable format. This is why there is data encryption/decryption in this layer. (SSL/TLS)
- Session layer maintains the connection and the session between apps.
- Transport layer provides reliable data transfer using TCP and UDP.
- Network layer makes suer that your IP packets are routed by the routers.
- Data Link layer handles the error detection in the frame level.
- Physical Layer is where the physical data is transferred over the network
#### Difference between ***TCP and UDP***
- TCP - Transmission Control Protocol
- UDP - User Datagram Protocol
- They're both in the transport layer.
- TCP
	- A connection oriented protocol. Before sending anything to the receiver, it will first establish the connection to the source.
	- Ensures that data is delivered correctly in sequence/order
	- Error checking is built within the TCP protocol
	- use cases - logging in, web applications
		- 90% of whatever we do
- UDP
	- It's connectionless. It doesn't care about connections and it just starts sending the data
	- UDP does not guarantee perfect data delivery/order
	- UDP - Basic error checking, no recovery
	- use cases - very specific use cases
		- live stream / gaming on YT 
#### IP Address VS Mac address
- IP
	- IP Address is a logical address which is assigned to a particular device over the network
	- identifies a device uniquely over a network
	- Your ISP (internet service provider) or your network provides
	- Can be changeable
	- Works at layer 3 (from bottom up - network layer, where IP packets are)
- MAC
	- Mac address is a physical address that is built into the device when it's manufactured.
	- identifies a unique Network Interface Card (NIC). Every device has a NIC card which contains the MAC address for that device
	- NIC manufacturer assigns it
	- Cannot be changed (permanent)
	- layer 2 data link layer
#### Unicasting vs Anycasting vs Multicasting vs Broadcasting
- 4 different ways how you can send data over a network
- unicasting - 1 sender 1 receiver (web application usually)
- anycasting - data is sent from 1 sender to nearest/best receiver 
	- CDN - content delivery network
	- edge locations - cached
- multicasting
	- data sent from 1 sender to group of interested receiver
- broadcasting 
	- data is sent from 1 sender to "ALL" receivers in a network
	- DHCP<<<<<<< ???
#### Terminologies
- What is a switch?
- What is a router?
- What is a subnet?
- What is a router gateway?
- ipv4 vs ipv6?
#### (Scenario) What happens when you type `www.google.com` in your browser for the *1st time*? ⭐
- checking your internal understanding
- 1st time - caching mechanism is not considered
1. DNS Resolution -> browser sends DNS request to the DNS server to get IP address for google.com
2. TCP connection -> browser has the IP address, so it will try to establish a TCP connection with it
	- TCP 3-way Handshake (SYN, SYN-ACK, ACK) will happen
	- then the connection gets established
3. TLS/SSL Handshake
	- since we're typing https, it's secure website
	- browser and server perform TLS/SSL handshake to establish secure connection
	- session is established
4. HTTP request
	- finally gets triggered
	- browser sends HTTP GET request to get Google's web page
5. Server processing
	- Google server will process the client request and potentially involve application logic & server side scripting
6. HTTP response
	- Google sends the webpage to browser
7. Rendering
	- Browser gets response and parse & render the content
8. Caching
	- the browser will cache whatever it can


- the difference between crypto asymmetric and symmetric

- The technical assessment tested knowledge of cloud products and how they worked. It mostly involves configurations of different cloud products and how you'd think about and advise customers on how to manage them.
- Out of a range of different technical topics, including databases, development, web architecture, etc., I was asked to pick two that I wanted to be asked about.

- general computer science knowledge, databases, docker, networking
- Basic questions in networking , security , storage , database , computing, OS
- know basic networking, cloud, and IT concepts like OSI,  etc



- why do you want to work at amazon
Leadership principles, cloud concepts, basic CS principles

What is a leadership principle that didn't make sense to you?

# Security
- NACL
- Security group
- VPC
# Cloud & SAA
#### What is cloud computing? What are the benefits? ⭐
- variants
	- explain what is cloud to a grandmother
	- what is the AWS cloud in your own words?
- Cloud computing is the on-demand delivery of IT resources over the internet with a pay-as-you-go model.
- The benefits include agility, elasticity, cost savings, and being able to deploy globally within minutes.
#### What is the responsibility of a Solution Architect? Why are you interested in it? ⭐
- Define SAA in your own words, and define how ur skills & interests align
- A SAA is a trusted technical advisor for customers as they go along their journey using the cloud. It's a role that needs both technical and customer facing skills in order to help customers come up with flexible, scalable, and resilient architectures. This role aligns with my interest because I like to dive deep into technical problems and have a passion for helping others.
	- Mention any customer-facing experience roles
	- Mention AWS certifications
#### What is the difference between IaaS, PaaS, and SaaS?
#### What is a microservice architecture?
#### Tell me a microservice design in AWS/ Can you tell me a microservice design with AWS?
- Avg answer
	- I will use ALB with EC2 or EKS, or API Gateway with Lambda
	- Bad coz you didn't explain how the design actually works!
	- Highlight microservice characteristics in the design
- Good answer
	- Let me explain a microservice design with an Application Load Balancer.
	- diagram
		![[microservice example.png|800]]
	- So first I would have Route 53 so that I will have a proper DNS name, like `www.store.com`. Any traffic going to this domain will be forwarded to this load balancer. 
	- Based on the URL path, the application load balancer can do path based routing and send the traffic to different target groups.
	- For example, with `store.com/browse`, it can send the traffic to a target group with Amazon EC2s, and maybe it has Amazon DynamoDB as storage, which is Amazon's flagship NoSQL database. This creates a microservice.
	- For `store.com/purchase`, i can send the traffic to another group which handles complex transactions and complex queries and joins, which is connected by Amazon Aurora. 
	- And you can do repeat that process for the other paths.
	- Each microservice is independent and isolated, so you can have different tech stacks and languages in each. For example, `store.com/browse` can be handled by containers running in Kubernetes with NodeJS, while `store.com/return` could be handled by Amazon Lambda in Go.
- Popular followup: How do you pick one service VS another as a solutions architect?
	- You mentioned EC2, container, lambda
	- Ask interviewer about system requirements
	- You just gotta study for this one!!!!
	- Load Balancer VS API Gateway, SQL vs NoSQL, etc
#### What is your favorite AWS service? How will you improve it further? Have you faced any challenges?
- DEPTH - Don't just give any cutting edge tech
- Mention one challenge you have fixed, and another that requires workaround or no solution exist
- Example
	- EC2
		- AMI rehydration is a challenge -> then you can talk about how u automated it
		- Selecting proper instance types is hard -> use compute Optimizer
		- Shared responsibility model
			- we the customer are responsible for handling, wish there was an easy button where you do not have to orchestrate all of it
	- Lambda
		- selecting memory is hard (X know how much your application require) -> use Lambda insights (utilization vs allocation of memory, based on this u optimize)
		- Running beyond 15 mins for batch processes
	- RDS
		- cost was very high -> use reserved instances to fix (in return of u commiting for 1-3 yrs, you enjoy up to 70% discount )
		- scaling writer instances is hard
#### What is AWS Service X? ⭐
- Say the official definition, then add your own words..
	- ppl fumble soooo much!!!!
	- MEMORIZE
- EC2
- Lambda
	- AWS Lamba is a serverless compute service that lets you run code without provisioning or managing servers, .... 
	- you can then expand with ur own words -> it has some other advantages, it's pay as you go, it scales automatically and inherently available under the hood. And based on project xyz this is good 
	- SAY THE INITIAL DESCRIPTION
	- creating workload-aware cluster scaling logic, maintaining event integrations, or managing runtimes
- S3
	- Amazon Simple Storage Service is an object storage service that offers industry leading scalability, data availability, security, and performance.
- EKS
	- Amazon Elastic Kubernetes Service is a managed service that you can use to run Kubernetes on AWS without needing to install, operate, and maintain your own Kubernetes control plane or nodes.
- CloudFormation
	- CloudFormation gives you an easy way to model a collection of related AWS and 3rd party resources, provision them quickly and consistently, and manage them throughout their lifecycles, by treating infrastructure as code.
- RDS
- ELB
- Route 53
- SQS
#### How do you pick one service vs another as a solutions architect?
- Ask interviewer about system requirements (functional, non-functional)
- no shortcut for this
- just study for this.......(search x vs y with clod with raj)

#### What is RTO and RPO?
- Average
	- RTO = Recovery Time Objective, RPO = Recovery Point Objective
	- RTO stand for Recovery Time Objective, and it signifies how much time the application can be down in case of a disaster
	- RPO stands for Recovery Point Objective, and it measures how much data can you lose in case of disaster
	- very vague and high level
	- most ppl mess with *unit of measurement*!-> "RTO is measured with time and RPO is measured with data (wrong lol)" -> you have to show interviewer you know this
	- always think question behind the question
- Good answer
	- RTO = Recovery Time Objective, RPO = Recovery Point Objective
	- RTO signifies the maximum acceptable time it should take to restore the application after a disaster or disruption, and RPO measures maximum acceptable amount of data loss after the failure. Both RTO and RPO are measured in *time*.
		- How is RPO measured in time? If you backed up at 1pm and if it backs up every 1 hour, if there is a disaster you lose up to 1 hour of data max. But if you back up every 10 minutes, the maximum about of data is 10 minutes worth.
		- Reduce RPO -> frequent backups, Eliminate RPO -> real time backup
	- Based on RTO and RPO, we can select one of the four AWS Disaster Recovery Strategies
		![[RPO_RTO.png]]
	- Backup & Restore has the lowest RPO/RTO but it has least cost
	- Multi-site active/active has the highest RPO/RTO but it is most expensive
	- A follow up can be "Can you tell me one of the disaster recovery strategies you are familiar with?"
		- Multi-site active/active >>> study one in depth
			- shows knowledge in multiple areas
#### How did you /will you do DR (disaster recover) for your cloud application?
- Average answer
	- I will replicate to another region -> veeeeery vague. Does not show the depth knowledge that an architect needs
- Good answer
	- There are different options to choose depending on RTO and RPO
	- For lower priority use cases, the RTO and RPO could be longer, so I will use backup and restore. 
	- For higher priority cases, the RTO and RPO is higher, so I will use multi-site active/active.
	- So for example, with a multi-site active/active, i'll have the same architecture running in different regions. And there will be route 53 that redirects the traffic to the either of the load balancers of the architectures depending on latency. So if one region goes down totally, it will automatically switch the traffic to the other region.
	- diagram
		![[disaster_recovery_example.png|900]]
#### How do you migrate an application from on-premises to the cloud?
#### How do you secure your application on the cloud?
- Average
	- Use KMS, IAM, and firewalls for security.
	- Explain what they do rather than just saying the service names..
	- Take one app (such as three tier app with EC2, or microservice running on Kubernetes, or Serverless.. & explain in detail!)
- Good
	- Assuming my application is in a serverless manner (hosted in API Gateway), and the APIs are handled by Lambda and that lambda is going to different dbs.
	- On the user side, I will implement Authentication for login, and secure data at transit using SSL/TLS. You can increase security for data at rest using KMS.
	- For the security of the application, maybe we'll have an application container image running within a pod. And that pod is running in an Amazon EC2. 
		- So application security for Kubernetes will look like:
			- use namespaces to divide the cluster
				- separate *resource quota* and *access* for each namespace
				- By default, all pods can talk to each other
			- use network policy to control traffic
				- control traffic by IP, label, or namespace
			- Implement RBAC (role based access control)
				- specify separate roles for separate groups (admin, developer, tester, etc)
			- Do not allow privileged esacalation
			- Use OPA to enforce restrictions
				- Images from approved repo, namespace with label with point of contact
		- this will prob get most questions


- iac
- IAM
- ECS vs EKS

Modern data platform Cloud compute Migration
# Operating System (OS)

#### What is the Linus Booting Process?
- Init 프로세스에 대해 설명

# Database
#### What is the difference between SQL and NoSQL? ⭐
- Bad Answer
	- SQL holds structured data, No SQL handles semi-structured or unstructured data.
	- SQL needs a schema, NoSQL is schemaless.
	- This is BARE MINIMUM. Interviewer is looking how this impact different design factors and how one is chosen VS another. 
- Good answer
	- SQL holds structured data, No SQL handles semi-structured or unstructured data.
	- SQL needs a schema, NoSQL is schemaless.
	- SQL databases need to scale vertically for the writer instances and NoSQL databases scale horizontally.
	- SQL databases are better for multi-row transactions, and NoSQL is better for unstructured data like documents or JSON.
	- SQL databases support complex queries and joins. NoSQL databases do not support complex queries and joins.
	- SQL follows ACID and NoSQL follows CAP theorem.
	- NoSQL db is inherently highly available, but in SQL you are responsible for that part
	- Add any personal experience!
#### Do you know when to use SQL and when to use NoSQL 
- Difference between relational and non-relational database?

#### Sql basics

#### DBMS commit and rollback difference
#### What's a transaction

#### ACID

#### What is sql injection
#### What is index in SQL?

#### What is the N + 1 query problem?

#### Data Normalization


# LP / Personal
Memorize the amazon leadership principles, Know about the projects on your resume, and have an in-depth understanding of all of the technologies uses in those projects
- Tell me about the most technical class project you have worked on.
- What was one of your most complex technical achievements?
- Tell me about a time you took a risk 
- Tell me about a time you had to solve a problem with limited resources
- Tell me about a time where you had to search a lot to find an answer.
- Do you remember a time in a project where you couldn't advance further with your current knowledge, how did you cope and what did you learn? 
- Do you remember a time when you made a mistake in a project, how did you cope and what would you do differently nowadays?
- What were the innovative things you built that pushed you out of your comfortable zone?
- Tell me about a time when you failed and what did you learn from it?
- Tell me about a time which was challenging, difficult and how you overcame the difficulties.
	- When was a time you faced a challenge that you didn't know how to solve? Expand on your thought process.
- Tell me about yourself? What was one time you face a roadblock with a deadline and how did you work through it?
- How did you prove (insert leadership principles)
- Tell me about a time when you dealt with an employee with poor performance
Describe a time you had a conflict at work, how did you solve it? What did you learn?


- Scenario Based questions - MCQ (Networking, Computing, AI/ML/Databases, OS)
- There is 1 Stage of Interview. Its a 1 hour interview consisting of both Technical and Leadership principles. Basic cloud services like Networking, Security Group, NACL. Database. Questions on the past project you have done. Leadership Questions such as "Tell me a time when...."


---
- [random redditor](https://www.reddit.com/r/ITCareerQuestions/comments/ekeo32/amazon_solutions_architect_intern_interview/)
	- Congratulations! I have worked with ASA's, SA's and senior SA's from AWS as part of my job. You should expect the bread and butter solutions architect questions like "whiteboard me a three tier app" and then expect optimization questions like "how would you control costs in this section of the architecture you drew out" (ASG's, CloudWatch alerts). The more experienced answer would include Technology Partner vendors, for example, you might push back on the interviewer and say, "well it depends on the client, if they're multicloud and are already on StackDriver, I might use their existing StackDriver implementation to trigger alerts for their AWS resources since StackDriver has native integration with AWS despite being a GCP product" -- or something else along the lines of "Before committing the customer to this architecture I'd want to have a discussion with them about their refactoring strategy for 2020 and see if they want to get away from traditional EC2's, and if not, then see if they're open to things like Spot instances or (formerly) Reserved Instance pricing. I'd get in touch with a Partner Development Manager to see if we could leverage a system integration partner of AWS depending on the environment complexity and/or spend that the customer expects to have with AWS."
	- Either way, best of luck. It's an honor to be interviewing with AWS for an Associate Solutions Architect or SA Intern role. Good luck!
