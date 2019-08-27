# SAA Foreword

## SAA考察的技能

Section             | %
--------------------| -----------------
Design Resilient Architectures | 34%
Define Performant Architectures | 24%
Specify Secure Applications and Architectures | %16
Design Cost-Optimized Architectures | 10%
Define Operationally Excellent Architectures | 6%

## SAA涉及的主要领域包括

- 网络：VPC、CloudFront、Rout53
- 计算：EC2、ECS、Lambda、Beanstalk
- 存储：S3、EBS、EFS
- 数据库和数据分析：RDS、DynamoDB、Redshift、Elasticache
- 安全：IAM、KMS
- 其他：SQS、SNS、CloudWatch、CluodTrail、CloudFormation

## 学习曲线（适合于自学，非培训途径）

- 掌握必备的网络、存储、数据库基础知识，如果这3大领域基础薄弱或者项目经验不够的话，会稍微吃力点，不过勤能补拙，下一步就多花点时间记忆吧
- 在第三方网站找到合适的知识点总结，一边顺知识点，一边对照aws文档理解，覆盖所有重要的模块。有的知识点如果已经有所了解，aws文档可以适当跳着看，如果是新的知识点，稍微放慢速度，重要的是理解。看文档的同时，根据自己掌握情况，记录和标示知识点。
- 网上有一些第三方网站总结的知识点，但是SAA题目和考试范围变更比较快，很多过时的，如果看的话，找新的来看，最近1年内更新的最好。推荐一个网站：https://tutorialsdojo.com/aws-cheat-sheets/
- 针对基本概念，找些题目来强化和矫正；针对场景，找些题目来做，提高阅读理解的能力。网上的资料比较杂，有的所谓题库都是18年之前的，答案对错不一，但是也有些参考价值，需要自己有一定甄别能力，对照题目思考和举一反三。
- 我在网上主要找了2份题目，一个是《AWS Certified Solutions Architect Practice Tests Associate SAA-C01 Exam》，是19年出版的，上面有1000道题，属于基础性的，用来巩固基本概念比较好；还有一个是6份考题的资料，来源不详，可能是tutorialsdojo网站的，也是从这份资料中看到了上面提到的aws cheatsheet总结的知识点，不过我也没来得及看cheatsheet上面总结的内容，大概阅了几个页面，总体感觉挺细也挺全面。另外看了别人总结的经验，都提到了Udemy的一个6套题的付费题库，我是没有去购买，虽然价格也不贵。这几套题库，可以类比正式考试来对待，重点关注里面的场景题，正式考试的场景题和题库中的难度基本相当。
- 考前留点时间复习一遍错题

## Tips

- 凡是涉及到availability, scalability, durability, liability, effectiveness的地方，可稍加留意，考试题目中很多涉及到这些字眼的考题
- aws文档中Tips的部分要关注下
- 很多参数等数据，有个印象即可，题目中偶尔会涉及个别，但是都记住也很难，看运气吧
- 题目太长，对题意理解不透的时候，从题目中提炼关键词，根据关键词来作答。AWS的出题有套路，答案同样也有套路，从关键词基本就可以联系到可能的答案是什么了，可以说是标准做法，比如涉及cost的，很多都和spot、reserved、cold、infrequent等相关，涉及scalability的很多都和dynamodb、ELB相关，涉及performance的，很多都和cache、dynamodb、aurora相关，涉及安全的，很容易想到iam role、KMS，涉及到定时的就和scheduled有关，等等诸如此类的还有很多，看的时候横向对比下
- 正式考试时间是130分钟65道题，平均1题2分钟，应该足够了，把握不准的标记下最后再核对，不宜标注太多，不然最后核对时也容易乱，标注10个以内差不多了
