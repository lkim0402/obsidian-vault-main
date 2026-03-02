
>[!ACM]
>- simplifies the process of obtaining, managing, and deploying SSL/TLS certificates for your websites and applications.
>- helps with encrypting network traffic

- Free SSL/TLS Certificates
	- ACM allows you to provision trusted SSL/TLS certificates for free. 
	- These certificates enable **encrypted connections (HTTPS) to your websites and applications**
- Easy Integration
	- You can set up and manage ACM certificates directly within the configuration interfaces of services like  [[Elastic Load Balancer (ELB)]], [[CloudFront]], and API Gateway
	- For example, when you're setting up a [[CloudFront]] distribution, in the general settings of the distribution, you can request / select a ACM certificate from your account, and more

# Example
- You're using [[CloudFront]] to deliver content in a [[S3 (Simple Storage Service)|S3 bucket]]
	- 2 connections involved
		- From the user's browser to CloudFront
		- From CloudFront to your S3 bucket
- What happens
	- CloudFront uses your ACM certificate to establish a secure HTTPS connection with your users' browsers. This encrypts all data traveling between users and CloudFront.
	- After CloudFront receives the encrypted request, it "terminates" (or ends) the SSL/TLS encryption process by decrypting the request.
	- CloudFront then forwards the now-unencrypted request to your S3 bucket over AWS's internal network, which is already secure.
- Example of **SSL termination**
	1.  A user makes an HTTPS request to your website (which is served via CloudFront)
	2.  The connection between the user's browser and CloudFront is fully encrypted using the SSL/TLS certificate from ACM
	3.  When the request reaches CloudFront, it "terminates" the SSL/TLS connection by decrypting the request
	4.  CloudFront then forwards the now-decrypted request to your S3 bucket over AWS's private network
	5.  S3 sends the response back to CloudFront (unencrypted)
	6. CloudFront re-encrypts the response and sends it back to the user over HTTPS