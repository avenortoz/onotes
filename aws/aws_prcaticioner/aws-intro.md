#aws
# Introduction

## Type of services
- SaaS - software as a service (Google Disk, Google Documents, Drobox and so on)

- PaaS - platform as a service (heroku, github, gitlab)
    remove need to manage underlying infrastructure

- IaaS - infrastructure as a service (EC2, AWS, rackspace, azure) 
    Provides networking, computers, data storage space

- DaaS - database as a service

## Types of clouds

-   Private Cloud - the organization hold the cloud and maintain it (provides a
    high level of security)
-   Public Cloud - the third party organization hold the cloud and you just use
    it (you have pay-as-you-use, toolkit, and so on, but has less security)
-   Hybrid Cloud - Private + Public cloud
-   Community Cloud - several organization hold the cloud

### Six advantage of cloud computing
- Trade capital expense (CAPEX) for operational expense (OPEX)
- Benefit from massive economies of scale
- Stop guessing capacity
- Increase speed and agility
- stop spending money running and maintaining data centers
- go global in minutes

## IAM

## EC2
IOPS - input output operation for second (a characteristic of the storage device)

Purchasing options:
- on-demand - pay per second 
- spot - used to run just some work and after that will be disposed of. You'll lose the instance at any point if your max price is less than the current spot price (workloads resilient to failars)
- reserved (Standard + convertible + scheduled) 1 - 3 years
- dedicated host (you own a hardware)
- dedicated instance (your instances will not share a hardware but are not
  attached to some specific hardware and if stopped can be run on different
  hardware but will not share it with other someone's instances)

## EC2 instance storage
- EBS volumes 
    - network drives attached to one EC2 instance ata a time
    - mapped to an availabilty zones
- AMI: create ready-to-use EC2 isntances with our customizations
- EC2 image image builder: automatically build, test and distrubute amis
- EC2 instance store
    - high performance hardware disk attached to our ec2 instance
    - lsot if our instance is stopped/terminated - use for temp file aka cache
- EFS: network file system can be attached to 100s of instances in a region very prisy
- EFS-IA: cost-optimized storage calss for infrequent accessed files

## Chapter 7
### Scalability, High availability, elasticity
- vertical - increase the size of the intance (for a database for example)
- horizontal - increase the number of instances ()


ELB - elastic load balancer

4 kinds of load balancers offred by aws
- application load balancer - Layer 7 http/https/gRPC
- network load balancer - layer 4 TCP
- gateway load balancer - layer 3
- classic load balancer (retired)

Auto Scaling Group

Scaling strategies
- Manual Scaling
- Dynamic Scaling
    - Simpe/Step Scaling. Exaple: when cloudwatch alarm is triggered whe cpu > 70% add 2 units
    - Target Tracking Scaling (keep average cpu at 40%)
    - Scheduled scaling (based on usage pattern: increase min capacity to 5 at friday )
- Predict Scaling (Machine Learning)

## Chapter 8
- Backup and storage
- disaster recovery
- archive
- hybrid cloud storage
- application hosting
- media hosting
- data lakes & big data anylytics
- static websites

**S3 buckets are bound to the region**

### s3 objects
- the key is the full path
- the key is composed of **prefix** + **objects**
- max object size is 5TB
- if uploading more than 5 gb you must use multi part upload

### Storage classes
- frequent access
    - S3 general purpose
        - 99,99 availabilty
        - used for frequently accessed data
        - low latency and high throughput
        - sustain 2 oncurrent facility failures
        - **Use cases**: big data anylytics, mobile, content distribution

- infrequent access
    - s3 standard-infrequent access (standard IA)
        - 99.9 availability
        - **Use cases**: disaster Recovery, backups

    - s3 one zone-infrequent access (S3 one zone-IA)
        - high durability 99.999999999 in a single az; data lost when AZ is destroyed
        - 99.5 availablity

- glacier storage classes
    low cost object storage meant for archiving/bakup
    pricing: price for storage + object retrieval cost

    - Amazon s3 glacier instant retrieval
        - milisecond retrieval, greate for data accessed once a quarter
        - minimun storage duration of 90 days

    - Amazon s3 glacier flexible retrieval
        - min 90 days
        - epedited (1 to 5 minute),  standard (3 - 5 hours), bulk(5 - 12 hours) - free

    - Amazon s3 glacier deep archive
        - min 180 days
        - standard (12 hours), bulk (48 hours)
    
- s3 intelligent tiering (do automatic move)
    

### Snow family (devices)
- Data data migration
    - snowcone, snowball edge, snowmobile

- Edge computing
    - snowcone, snowball edge

#### Snowball
Get a snowball device - install snowball client - upload data to the device (TB - PB) - send device back - data will be laoded into s3 - showball is completely wiped

#### Snowcone
- small computing device, withstand hard environemnt
- light: 2.1 kg
- device used for edge computing, storage, and data trasfer

## Chapter 9

## Chapter 10

### Docker aws infrastructure
- **ECS** - elastic container service
- launch docker containers on aws
- you must provision and maintain the infrastructure ec2
- aws takes care of starting stopping containers

- **Farget** do the same but you dont need to provision th infrastructure

- **ECR** - container registry
- were you store docker images

### What is serverless
- initially serverless means FaaS (function as a service)
- something that are already managed (like database, amazon s3, dynamo db, fargate, lambda)

### AWS Lambda
Labmda
- virtual functions - no servers to manage
- limited by time 
- run on demand
- sacale automatically

- pay per request and compute time
- event drive
- very cheap

cloud watch events evertbridge - trigger - aws labmda

### Amazon API Gateway
### AWS batch
### Lightsail - meant for very simple application, very simplified aws

## Chapter 11
AWS cloud fromation
- declarative way of outlining your aws infrastructure

- AWS code beans (paas)
- AWS code deploy (hybrid service manage versios of the applications)
- AWS CodeCommit - code repository
- AWS code build - build code in the cloud into packages
- AWS code pipeline - ci/cd
- AWS code artifact - manage code artifacts (code dependencies, etc)
- AWS code star - facade for the service above
- cloud9 - IDE

## Chapter 12
- AWS Route 53 - DNS 
- AWS cloud front (CDN)
- AWS global accelerator

## Chapter 13
- SQS - simple queue service
- Keasis
- SNS
- amazon mq - managed message broker service for RabbitMQ and activeMQ

## Chapter 14
- Amazon CloudWatch 

## Chapter 15
Elastic IP - a public IP though which access is provided to an instance. Is static and does not change if i terminate or suspend the instance, but is charged when you don't use it

- VPC - virtual private cloud
- Private subnet - cannot be accessed from within local network, but can accesses
the resources on the Internet by the means of NAT
- Public subnet - can be accessed within the network by the means of Internet Gateway
- NACL - Network ACL
    - firewall which controls traffic from and to subnet
    - are attached at the subnet level
    - rules only include ip addresss

VPC peering, VPC flow logs

- VPC endpoints gateway is used to comminicate with the services without the
  need to use public network for DynamoDB and S3
- Cloud Watch 
- vpc endpoints network interface for other services

- private link to allow your vpc access third party services in theiry own vpc privately

Site to Site vpn & direct connect
- site to site vpn
    - vpn overt the public network
    Need cgw (customer gateway), aws must use a vgw (virtual private gateway)
- direct connect 
    - phisical connection

- transit gateway
    for having thousands of vpc and on-premises, hub-and-spoke (star) connection

## Chapter 16
## Chapter 17
- aws shield standard: free, protects from ddos
- aws shield adwanced: 24/7 premium ddos protection
- aws waf: filter requsest based on rules
- kms: manages encription keys
- cloudHSM: hardaware encryption
- aws certificate manager: provistion, manage, and deploy ssl/tls certificates
- artifact: get access to compliance reports such as pci, iso, etc...
- guardduty: find malicious behavious with vpc, dns & cloudtrail logs using ml
- inspector: find vulnerabilities in ec2, ecr images and lambda
- config
- macie
- cloudtrail
- aws securtiy hub
- detective
- abuse
- access analyzer

## Chapter 18
- Amazon rekognision - recognize features, objects, text, etc
- Amazon Transcribe - speech - to text, filter, etc
- Amazon Polly - text to speech

## Chapter 19
AWS Organization
- Global service
- Allows to manage multiple aws accounts
- The main account is the master account
- Cost benefits
    - consolidated billing across all acounts
    - pricing benefits form aggragated usage
    - pooling of reserved ec2 instances for optimal servings
- API is available to automate aws account creation
- Restrict account privileges using service control policies (SCP)

AWS Control tower
AWS Service Catalog

### Billing and costing tools
- Estimating costs
    - pricing culculator
- Rracking costs in the cloud
    - billing dashborard
    - cost allocation tags
    - cost and usage reports - the most comprehensive report
    - cost explorer - visualize costs, forecast 12 months
- Monitoring against costs plans
    - billing alaarms
    - budgets


### Trusted Advisor
Analyze your AWS accounts and provides recommendation on 5 categories:
- Cost Optimizations
- Performance
- Security
- Fault tolerance
- Service limits

7 core checks (basic and developer support plan)
- s3 bucket permissions
- security gourps - specific ports unrestricted
- iam use
- mfa on root account
- ebs public snapshots
- efs public snapshots
- service limits

AWS support plans pricing
- **Basic** - free
    - customer service & comminities - 24 * 7 access to customer service,
      documentation, whitepapers and support forums

    - aws trusted advisor - access to the 7 core Trusted Advisort checks and
      guidance to provision your resources following best practices to increase
      perfomance and imporve security

    - aws personal health dashboard

- **Developer**
    - all basic support plan +
    - business hours email access to cloud support associates
    - unlimited cassees / 1 primary contact
    - case severty / response times:
        - general guidance: < 24 hours
        - systerm impaired: < 12 hours

- **Business Support Plan**
    - trusted advisor - full set of checks
    - 24*7 phone, email, and chat access
    - case severity / response times:
        - general guidance: < 24 hours
        - systerm impaired: < 12 hours
        - production system impared: < 4 hours
        - production system down: < 1 hours

- **AWS Enterprise on-ramp support plan**
    - intendted to be used if you have production or business critical workloads

- **AWS Enterprise support plan**
    - intended to be used if you have mission critical workloads
    - designated TAM
    - business-critical system down: < 15 minutes


## Chapter 21
Well architect framework
1. Operatinal excellence
2. Security
3. Reliablity
