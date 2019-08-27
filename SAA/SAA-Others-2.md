# SAA-Others-2

>这些服务是在做题过程中出现的，了解这些服务有助于排除错误选型，对于SAA考试来说，做到了解接口，没有时间的可以不用深究，前面6个产品可重点了解下：MQ、Cognito、SWF、AWS Config、Athena、AWS WAF

- MQ
	- If you're using messaging with existing applications and want to move your messaging service to the cloud quickly and easily, it is recommended that you consider Amazon MQ
	- supports industry-standard APIs and protocols so you can switch from any standards-based message broker to Amazon MQ without rewriting the messaging code in your applications
- Cognito
	- Cognito service is primarily used for user authentication and not for providing access to your AWS resources
	- can use Amazon Cognito to deliver temporary, limited-privilege credentials to your application so that your users can access AWS resources
	- You can retrieve a unique Amazon Cognito identifier (identity ID) for your end user immediately if you're allowing unauthenticated users or after you've set the login tokens in the credentials provider if you're authenticating users.
- SWF
	- Amazon Simple Workflow Service
	- build, run, and scale background jobs that have parallel or sequential steps.
	- a fully-managed state tracker and task coordinator in the Cloud.
	- use for creating a decoupled architecture

- AWS Config
	- AWS Config reports on WHAT has changed
	- AWS Config focuses on the configuration of the AWS resources and reports with detailed snapshots on HOW the resources have
- Athena
	- Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to manage, and you pay only for the queries that you run.
	- Athena is out-of-the-box integrated with AWS Glue Data Catalog, allowing you to create a unified metadata repository across various services, crawl data sources to discover schemas and populate your Catalog with new and modified table and partition definitions, and maintain schema versioning.
- AWS WAF
	- AWS WAF is a web application firewall that is deployed on either Amazon CloudFront or Application Load Balancer to help protect your web applications from common web exploits.
- DevOps
	- AWS OpsWorks is an application management service that simplifies software configuration, application deployment, scaling, and monitoring
	- OpsWorks is recommended if you want to manage your infrastructure with a configuration management system such as Chef
- AWS Budgets
	- gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount
- AWS Glue
	- AWS Glue is a fully-managed, pay-as-you-go, extract, transform, and load (ETL) service that automates the time-consuming steps of data preparation for analytics. AWS Glue automatically discovers and profiles your data via the Glue Data Catalog, recommends and generates ETL code to transform your source data into target schemas, and runs the ETL jobs on a fully managed, scale-out Apache Spark environment to load your data into its destination.
	- automatically discover both structured and semi-structured data stored in your data lake on Amazon S3, data warehouse in Amazon Redshift, and various databases running on AWS.
- Data Pipline
- Amazon Data Lifecycle Manager (DLM)
	- supports Amazon EBS volumes and snapshots
- AWS X-Ray
	- collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization
- AWS Trusted Advisor
	- analyzes your AWS environment and provides best practice recommendations in these five categories: Cost Optimization, Performance, Fault Tolerance, Security, and Service Limits. You can use a mnemonic, such as CPFSS, to memorize these five categories.
- AWS Systems Manager
	- AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances
	- Run Command enables you to automate common administrative tasks and perform ad hoc configuration changes at scale.
- Amazon WorkSpaces
	- a managed, secure cloud desktop service
	- You can use Amazon WorkSpaces to provision either Windows or Linux desktops in just a few minutes and quickly scale to provide thousands of desktops to workers across the globe
- Lightsail
- QuickSight
