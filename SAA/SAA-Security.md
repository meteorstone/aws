# SAA-Security

>security相关的概念主要是IAM和KMS，另外还有网络层和应用层安全相关的知识，这些知识点已经分散到各自的专题中了，这里重点留下IAM和KMS单独列出来。这些题目也比较容易混淆，在实践中容易忽略这部分，但是相对少的知识点却占不小的比重，可见重要性，总体难度在中等偏上

## 知识点(中难)

- IAM role
  - 很多情况下IAM role都是合适的选择
- IAM User & Group
  - 和IAM role对比区分
- Fedration
  - 结合STS & SSO & SAML出现，一旦出现就属于比较难的题目
- MFA
  - 也是经常出现，出现时基本都是合适的选择
- Encryption
  - KMS经常出现
  - 常和S3和EBS一起使用
  - CloudHSM有时也作为选型出现，但基本都不选

## 要点记录

- IAM
	- Identity and Access Management
	- IAM roles are global service that are available to all regions
	- grant unique security credentials to users and groups to specify which AWS service APIs and resources they can access
	- IAM role
		- a role does not have any credentials (password or access keys) associated with it
		- allow you to delegate access to users or services that normally don't have access to your organization's AWS resources.
		- obtain temporary security credentials by AWS Security Token Service (AWS STS)
		- don't have to share long-term credentials or define permissions
		- IAM role can be assigned to a running EC2 instance
		- IAM roles enable several scenarios to delegate access to your resources, and cross-account access is one of the key scenarios
		- use case:
			- Granting applications that run on Amazon EC2 instances access to AWS resources
			- Cross-account access: access to AWS resources in different account, instead of having to create users
			- Granting permissions to AWS services
	- can assign a range of security credentials
		- passwords
		- access keys
		- amazon cloudfront key pairs
		- ssh public keys
		- x.509 certificates
	- MFA
		- Multi-Factor Authentication
		- used in accessing to aws account; can also be used to control access to AWS service APIs
	- IAM identity providers and federation
		- You can also integrate IAM policies and permissions with directories that you already manage
			- Microsoft Active Directory
			- AWS Directory Service
			- OpenID Connect provider
		- use SSO to access AWS account with open standards, such as SAML2.0 or OpenID Connect
		- AWS Cognito helps to add support for federation to your web and mobile apps
		- with cognito you can also authenticate with social identity providers, e.g. Facebook, Twitter
	- Instance Profile
		- An instance profile is a container for an IAM role that you can use to pass role information to an EC2 instance when the instance starts
		- AWS creates a Instance profile automatically with the same name as the Role
	- You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MySQL and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token
	- [Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
		- It's a best practice to grant least privilege for only the permissions required to perform a task
		-   

- Encryption
	- [KMS](https://docs.aws.amazon.com/kms/latest/developerguide/service-integration.html)
		- create and manage keys and control the use of encryption across AWS services
		- integrate with CloudTrail to logging key using
		- customer master keys (CMKs)
		- keys can be generated or imported from your key management infrastructure
		- KMS Keys are only stored and used in the region in which they are created. They cannot be transferred to another region
		- Temporarily disable keys so they cannot be used by anyone
	- S3 uses KMS
		- server-sider encryption
			- SSE-S3: S3 manages the data and master encryption keys
			- SSE-C: you manages the encryption key. with the encryption key as part of your request when uploading or downloading. only support https.
			- SSE-KMS: AWS manages the data key but you manages the customer master key in AWS KMS
		- using S3 encryption client
	- EBS uses KMS
	- RDS uses KMS
	- DynamoDB uses KMS
	- SQS uses SSE
	- CloudHSM
		- CloudHSM is a cloud-based hardware security module (HSM) that enables you to easily generate and use your own encryption keys on the AWS Cloud
		- KMS provides an easy, cost-effective way to manage encryption keys on AWS that meets the security needs for the majority of customer data. CloudHSM offers customers the option of single-tenant access and control over their HSMs.
		- CloudHSM must be provisioned inside an Amazon VPC
		- AWS CloudHSM runs in your own Amazon Virtual Private Cloud (VPC), enabling you to easily use your HSMs with applications running on your Amazon EC2 instances. With CloudHSM, you can use standard VPC security controls to manage access to your HSMs.
		- You should consider using AWS CloudHSM instead of KMS in case of
			- Keys stored in dedicated, third-party validated hardware security modules under your exclusive control
			- FIPS 140-2 compliance
			- Integration with applications using PKCS#11, Java JCE, or Microsoft CNG interfaces
			- High-performance in-VPC cryptographic acceleration (bulk crypto)
- Disaster recovery (DR)
	- Recovery Time Objective (RTO)
	- Recovery Point Objective (RPO)
	- RTO from longest to shortest
		- Backup and Resotre
		- Pilot Light
		- Warm Standby
		- Multi-Site
- Aws Certificate Manager
	- Centrally manage certificates on the AWS Cloud
	- provides you a highly-available private CA service
	- Integrated with other AWS cloud services
		- ELB
		- CloudFront distribution
		- API Gateway
		- Beanstalk
		- CloudFormation
