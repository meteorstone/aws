# SAA-Others-1

>其他零杂都服务列到这里，虽然零碎，但是基本都是必考，和其他不同的是，一个服务基本出现一个考点就差不多了，难度中等

## 知识点(中)

- CloudWatch & CloudTrail
  - 首先要区分2者的应用场景
  - 多和其他服务一起使用
- SQS & SNS
  - 最重要的结偶工具
  - 注意分辨2者的区别
- CloudFormation
  - 常和SWF、Beanstalk一起出现
- API Gateway
  - 和Lambda一起

## 要点记录

- Cloud watch
	- monitor AWS resources and applications in real-time
	- collect and track metrics
	- alarms can be configured
		- to send notifications
		- to automatically make changes to the resources based on defined rules
	- custom metrics can also be monitored
	- Enhanced Monitoring is a feature of RDS and not of CloudWatch
	- stores the log data indefinitely
	- Alarm
		- Alarms invoke actions: SNS notification, Auto Scaling policies, EC2 action – stop or terminate EC2 instances
		- Alarm history is stored for only 14 days
	- Metrics
		- time-ordered set of data
		- exist only in the region in which they are created
		- stores the metric data for two weeks
		- Metrics cannot be deleted
		- Each metric data point must be associated with a time stamp. The time stamp can be up to two weeks in the past and up to two hours into the future
	- EC2 Metrics
		- available: CPU Utilization, Network Utilization, Disk Reads
		- not track memory and disk space/swap
- Cloud Trail
	- Log files from all the regions can be delivered to a single S3 bucket and are encrypted, by default, using S3 server-side encryption
	- get a history of AWS API calls and related events for the AWS account.
	- CloudTrail reports on WHO made the change, WHEN, and from WHICH location
	- CloudTrail focuses on the events, or API calls, that drive those changes
	- For most services, events are sent to the region where the action happened
	- For global services such as IAM, AWS STS, and Amazon CloudFront, events are delivered to any trail that has the Include global services option enabled.
- SQS
	- message queuing
	- provide at-least-once delivery
	- visibility
	- long/short poll
		- long polling helps reduce the cost of using SQS by eliminating the number of empty responses (when there are no messages available for a ReceiveMessage request) and false empty responses (when messages are available but aren't included in a response).
	- decoupling usage
	- SQS FIFO
		- provide exactly-once processing
		- The name of a FIFO queue must end with the .fifo suffix
		- The order in which messages are sent and received is strictly preserved and a message is delivered once and remains available until a consumer processes and deletes it
		- Duplicates aren't introduced into the queue
		- messages are ordered based on message group ID. Each message group ID represents a distinct ordered message group.
	- SQS queue will continue to exist even after the EC2 instance has processed it, until you delete that message
	- you can configure the message retention period to a value from 1 minute to 14 days
- SNS
	- suitable as a pub/sub messaging service instead of a message broker service
- Amazon API Gatewavy
	- integrate with Lambda
	- provides throttling at multiple levels including global and by a service call
- CloudFormation
	- CloudFormation is a low level service and provides granular control to provision and manage stacks of AWS resources based on templates
	- CloudFormation templates enables version control of the infrastructure and makes deployment of environments easy and repeatable
	- CloudFormation is designed to complement both Elastic Beanstalk and OpsWorks
	- CloudFormation supports Elastic Beanstalk application environments as one of the AWS resource types
	- CloudFormation also supports OpsWorks
