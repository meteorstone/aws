# SAA-Compute

>该部分主要涉及EC2、ECS、Lambda、Auto Scaling、Beanstalk等概念，占比较大，对阅读理解考验比较大，题目很多模棱两可的，所以比较容易出错，难度就划成中等吧

## 知识点(中+难)

- CE2 purchase type
  - 几种实例类型要掌握好，根据关键词选择spot、reserved、scheduled类型
  - 本来题目是不难，但是很容易一选就错，最主要还是做题人理解的和出题人考察的点没对上
- Instance lifecycle & cost
  - 偶尔会出现，频率不高，只要记住关键点，题目不难
- Auto scaling
  - 常和ELB一起使用，要重点掌握
  - scaling的策略也经常考到
- Lambda
  - SAA也比较喜欢考Lambda相关的题
  - 常和API Gateway一起用
  - 和ALB、ECS等方案一起出现时，往往比较难
- Beanstalk
  - 常和SWF、ECS等一起出现，考察点比较单一，了解后并不难
- ECS
  - ECS往往有比较明显的提示词，比如container、docker等，比较容易排除错项目

## 要点记录
- EC2
	- Purchase type
		- On-demand instances
			- Pay, by the second, for the instances that you launch
		- Reserved instances
			- available, for a term from one to three years
		- scheduled instances
            - available on the specified recurring schedule, for a one-year term.
			- Charges are incurred for the time that the instances are scheduled, even if they are not used
			- for workloads that do not run continuously, but do run on a regular schedule for e.g. weekly or monthly batch jobs
		- Spot instances
			- Spot Instances are a cost-effective choice if you can be flexible about when your applications run and if they can be interrupted
		- Dedicated Hosts
			- physical host
		- Dedicated Instances
			- Pay, by the hour, for instances that run on single-tenant hardware
		- Capacity Reservations
			- Reserve capacity for your EC2 instances in a specific Availability Zone for any duration
	- Reserved type can be scheduled or converted
	- Transferring data from an EC2 instance to Amazon S3, Amazon Glacier, Amazon DynamoDB, Amazon SES, Amazon SQS, or Amazon SimpleDB in the same AWS Region has no cost at all.
	- Storage for the Root Device
		- You can specify instance store volumes for an instance only when you launch it. You can't detach an instance store volume from one instance and attach it to a different instance.
	- Spot Instances
		- well-suited for data analysis, batch jobs, background processing, and optional tasks
		- EC2 terminates, stops, or hibernates your Spot Instance when the Spot price exceeds the maximum price for your request or capacity is no longer available
		- Amazon EC2 provides a Spot Instance interruption notice, which gives the instance a two-minute warning before it is interrupted.
		- One strategy to maintain a minimum level of guaranteed compute resources for your applications is to launch a core group of On-Demand Instances, and supplement them with Spot Instances when the opportunity arises.
	- EC2 Fleet
		- An EC2 Fleet contains the configuration information to launch a fleet—or group—of instances. In a single API call, a fleet can launch multiple instance types across multiple Availability Zones, using the On-Demand Instance, Reserved Instance, and Spot Instance purchasing options together
		- EC2 Fleet is available only through the API or AWS CLI
	- Instance lifecycle
		- you can't stop and start an instance store-backed instance
		- Rebooting an instance doesn't start a new instance billing period because the instance stays in the running state.
		- Reserved Instances that applied to terminated instances are billed until the end of their term according to their payment option.
		- instance remains on the same host computer and maintains its public DNS name, private IP address, and any data on its instance store volumes after rebooting
		- You can't enable termination protection for Spot instances
		- The DisableApiTermination attribute does not prevent Amazon EC2 Auto Scaling from terminating an instance
		- To prevent instances that are part of an Auto Scaling group from terminating on scale in, use instance protection
		- billing
			- you will not be billed if your instance is in pending state
			- you will not be billed if your instance is preparing to stop with a stopping state
			- when the instance state is stopping, you will not billed if it is preparing to stop however, you will still be billed if it is just preparing to hibernate
			- Reserved Instances that applied to terminated instances are still billed until the end of their term according to their payment option
	- Placement group
		- determines how instances are placed on underlying hardware
		- 2 strategies
			- Cluster - clusters instances into a low-latency group in a single Availability Zone
			- Spread - spreads instances across underlying hardware
- Auto Scaling
	- achieve better fault tolerance, better availability and cost management
	- distribute instances evenly between AZs by attempting to launch new instances in the AZ with the fewest instances.
	- When rebalancing, Amazon EC2 Auto Scaling launches new instances before terminating the old ones
	- ASG
		- Launch configuration
			- similar to EC2 configuration and involves selection of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. 
			- can be associated multiple ASG.
			- can’t be modified once created
		- Minimum & Maximum & Desired capacity
		- Availability Zones or Subnets
		- Metrics & Health Checks
		- distributing instances across AZs
		- cannot span multiple Regions
	- ASG with multiple instance types and purchase options
	- scaling options
		- Maintain current instance levels at all times
			- When it finds that an instance is unhealthy, it terminates that instance and subsequently launches a new one.
		- manual scaling
		- scale by schedule
		- scale based on demand
	- Scaling Policy Types
		- Target tracking scaling
		- Step scaling
		- Simple scaling
	- lifecycle
		- scale out
		- scale in
		- default termination policy
			- Determine which Availability Zone(s) have the most instances, and at least one instance that is not protected from scale in.
			- Determine which instance to terminate so as to align the remaining instances to the allocation strategy for the On-Demand or Spot Instance that is terminating, your current selection of instance types, and distribution across your N lowest priced Spot pools.
			- Determine whether any of the instances use the oldest launch template
			- Determine whether any of the instances use the oldest launch configuration
			- Determine which instances are closest to the next billing hour
			- choose at random
		- Instance Protection
			- Instance protection does not protect for the below cases
				- Manual termination through the EC2 console, the terminate-instances command, or the TerminateInstances API
				- Termination if it fails health checks and must be replaced
				- Spot instances in an Auto Scaling group from interruption
		- Suspension
			- Auto Scaling processes can be suspended and then resumed
			- Amazon EC2 Auto Scaling can also suspend processes for Auto Scaling groups that repeatedly fail to launch instances. This is known as an administrative suspension.
	- scalable resources
		- EC2
			- EC2 Spot Fleets
			- ECS
			- DynamoDB
			- Aurora
	- ELB with ASG
		- default health checks for an ASG are EC2 status checks only
		- can configure ASG to use ELB health check
		- If you configure the ASG to use ELB health checks, it considers the instance unhealthy if it fails either the EC2 status checks or the load balancer health checks.
- Lambda
	- Serverless architecture
	- stateless, event-driven
	- Lambda runs your code on high-availability compute infrastructure and performs all the administration of the compute resources
	- After you upload your code to AWS Lambda, you can associate your function with specific AWS resources (e.g. a particular Amazon S3 bucket, Amazon DynamoDB table, Amazon Kinesis stream, or Amazon SNS notification). Then, when the resource changes, Lambda will execute your function and manage the compute resources as needed in order to keep up with incoming requests.
	- supports synchronous and asynchronous invocation of a Lambda function
	- Built-in fault tolerance
		- provide high availability for both the service itself and for the functions it operates
	- Automatic scaling
		- no limit to the number of requests your code can handle.
	- AWS Lambda automatically monitors Lambda functions on your behalf, reporting metrics through Amazon CloudWatch
	- use versioning and aliases to deploy your Lambda functions when building applications with multiple dependencies and developers involved
	- Blue/Green Deployment
		- Canary
		- Linear
		- All-at-once
- ECS
	- micro services architecture that supports docker container
	- high scalable and high performance
	- Amazon ECS has two modes: Fargate launch type and EC2 launch type. With Fargate launch type, all you have to do is package your application in containers, specify the CPU and memory requirements, define networking and IAM policies, and launch the application. EC2 launch type allows you to have server-level, more granular control over the infrastructure that runs your container applications.
	- ECS enables you to inject sensitive data into your containers by storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters and then referencing them in your container definition. This feature is supported by tasks using both the EC2 and Fargate launch types.
- Elastic Beanstallk
	- an easy-to-use service for deploying and scaling web applications and services and Docker on familiar servers
	- automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring
	- uses core AWS services such as Amazon EC2, Amazon Elastic Container Service (Amazon ECS), Auto Scaling, and Elastic Load Balancing to easily support applications that need to scale to serve millions of users
	- ideal if you have a PHP, Java, Python, Ruby, Node.js, .NET, Go, or Docker web application
	- An environment runs a single application version at a time, but same application version can be deployed across multiple environments
	- Applications can have many versions and each application version is unique and points to an S3 object. Multiple versions can be deployed for an Application for testing differences and helps rollback to any version if case of issues
	-   

