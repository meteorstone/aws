# SAA-Networking-2

>该部分主要涉及CloudFront、Rout53、ELB，其中CloudFront和ELB是最重要的几个服务，难度都在中等以上

## 知识点(中+难)

- CloudFront Origin & Edge
  - 常和S3、EC2等一起使用
- cache & expiration
-distribution
  - 常和Elasticache、EFS、Lambda、Rout53等放在一起考
- security
  - 和S3一起使用
- route53主要了解alias和路由策略
- ELB
  - ALB和ELB是重点，CLB作为旧的服务，也会出现，原则是推荐使用新服务
  - ALB使用非常广，几乎所有地方都可以插一脚进去，重点结合ASG和EC2来理解

## 要点记录

- VPC Interface Endpoint & VPC Gateway Endpoint
- CloudFront
	- an easy and cost effective way to distribute content with low latency and high data transfer speed
	- route request to the edge location with best service
	- suitable for frequently accessed static content e.g. images, videos, media files or software download
	- collapse simultaneous request into one to the origin server refusing the load on the origin
	- integrate with AWS WAF
		- web application firewall
		- allow rules configured based on IP address, HTTP header, and custom URI string
	- an origin server can be S3, EC2 or an on premise server
	- CloudFront sends distribution configuration to all edge locations
	- web distribution
		- support both static and dynamic content
		- support multimedia and HLS
		- support live event, such as meeting, conference or concert in real time.
		- origin server can be S3 or an HTTP server, e.g. a web server or an AWS ELB
	- RTMP distribution
		- must use S3 as the origin
		- provided media files and media player. media player is distributed by web
	- Origin
		- S3 origin can be configured to restrict access using Origin Access Identity to prevent direct access to S3
		- distribution can have multiple origins
	- Cache behaviors
		- can apply path pattern for request
	- Viewer Protocol Policy
		- access protocol allows HTTP/HTTPS
		- neither self signed certificate nor Cloufront certificate can not be used in origin side. if origin is not ELB, the certificate must be issued by trusted CA
		- trusted CA issued certificate and ACM certificate and self signed certificate can be used in cloudfront
	- Allowed HTTP methods
		- GET/HEAD/OPTION/PUT/POST/PATCH/DELETE
	- Edge Cache
		- for web distribution based on query parameters: case sensitive, order sensitive
		- for web distribution based on cookie values: specific cookies, case sensitive, only for dynamic content
		- for web distribution based on request headers: specific headers, case insensitive for header name
		- for RTMP distribution query parameters will be removed
		- for RTMP distribution can not be configured to process cookies
		- for RTMP distribution can not be configured to cache based on headers
	- Expiration
		- After expiration
			- if cache version is latest, origin returns 304
			- if cache version is not latest, origin return 200 and the latest object
		- if object is not frequently requested, cloudfront might remove it before expiration
		- by default object is expired after 24 hours
			- AWS recommends using Cache-Control max-age to control caching behavior
	- Serving Private Content
		- using special cloudfront signed URLs or signed cookies
		- using only cloudfront URLs, not S3 URLs to access S3 content
		- for S3 origin using Origin Access Identity to grant only cloudfront access, using bucket policy or ACL to content
		- for http server verify custom header added by cloudfront
		- trusted signer
		- using signed URLs: for RTMP distribution; to restrict access to individual files; user client does not support cookies
		- using signed cookies: provide access to multiple files; do not want to change the current URLs
	- CloudFront by default assign a domain name for the distribution that can be used as an CNAME
	-  CloudFront support Geo restriction at the country level 
	- CloudFront with S3
		- benefits: cost effective if objects are frequently accessed; downloads are faster
		- OAI can be used to prevent direct accessing S3 objects; S3 bucket/object permissions need to be configured only provide access to OAI
	- Objects can be updated by: overwriting origin; creating a different version and updating the link (recommended)
	- CloudFront can be configured to create access log periodically
	- Lambda@Edge
	- 
- Route53
	- a highly available and scalable DNS web service
	- DNS service
		- A/AAAA/CNAME/MX/NS/PTR/SOA/SPF/SRV/TXT
		- can route traffic to CloudFront, Elastic Beanstalk, ELB, or S3 by alias resource
		- alias record is similar to CNAME but can used for both root domain or apex domain and subdomains
		- automatically recognize changes in the referred resources, e.g. elb changes its
	- route policy
		- simple routing policy
		- weighted routing policy: load balancing; A/B testing
		- latency-based routing policy
		- failover routing policy
		- geolocation routing policy
	- health check
		- can monitor health of resources such as web and email servers
		- can be configured with CloudWatch to send notification
		- can be configured to route internet away from unavailable resources
- ELB
	- ALB
		- works in layer 7 with HTTP/HTTPS/Websockets
		- support for path-based routing, e.g. example.com/order example.com/image 
		- support for host-based routing, e.g. order.example.com image.example.com
		- support for routing to multiple services on a single instance with different ports, e.g. port 8001 for image and port 8002 for order; however one classic load balancer can only route to one service.
		- support ECS by dynamic port map
		- support for health check at target group level
		- automatically scale traffic handling capacity
		- provides high availability by allowing specifying multiple AZs
		- integrate with ACM to provision and bind SSL/TLS certificate
		- support security group
		- provide access log to S3
		- integrate with CloudWatch to provide metrics
		- integrate with AWS WAF
		- integrate with CloudTrail
		- target type could be instance, ip, lambda function
		- can attach target group to an auto scaling group
		- should specify at least 2 subnets in at least 2 AZs
		- stick sessions feather enables the elb to bind users’ session to a specific instance
		- support SNI
	- NLB
		- works in layer 4 with TCP; support long-lived TCP connection
		- connection-based load balancing
		- is highly available
		- is designed with high throughput and also to handle high sudden volatile traffic
		- offers extremely low latency
		- automatically provides a static IP per AZ; also can be assigned with an elastic IP
		- provide network-level health check and application-level health check
		- use flow log to record all requests
	- Classic Load Balancer
		- enable cross zone load balancing manually
