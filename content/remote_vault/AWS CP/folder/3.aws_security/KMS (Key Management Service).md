
>[!KMS]
>- A fully managed service that helps you create, store, and manage encryption keys for your data.
>- Designed to work with many AWS services to provide encryption and key management across your infrastructure.

- Automatic decryption & encryption like [[CloudHSM]]
- **Automatic Encryption and Decryption**:
	- KMS simplifies encryption and decryption by **managing encryption keys**. Many AWS services integrate with KMS, allowing you to easily enable encryption.
	- When encryption is enabled for a service (e.g., [[S3 (Simple Storage Service)|S3]], [[RDS (Relational database service)|RDS]], [[EBS (Elastic Block Store)|EBS]]), data is **automatically encrypted** when stored and **decrypted automatically** when accessed. This is done with KMS keys.
	- You only need to **enable encryption**, and KMS takes care of key management in the background.
- can automate rotation for encryption keys
	- offering both automatic and on-demand rotation for customer-managed keys
