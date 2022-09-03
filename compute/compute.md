# Compute

#AWS/SolutionsArchitect

## _Amazon Elastic Compute Cloud (EC2)_

### _AMI's & Instance Types_

---

- AMI's are digital templates of pre-configured EC2 instances allowing you to quickly launch a new instance
- You have the choice of AWS managed, custom, marketplace, and community AMIs

#### Instance Types

- Instance types are defined by different parameters and their values, such as the amount of memory, network performance, storage, etc
- As your instance size gets bigger, the parameter values (vCPU, storage, network performance) also increase
- Some instances only support EBS and do not have support for ephemeral storage
- General purpose instances give a good balance of compute, memory and networking resources, so it's great for lots of different use cases
- Compute optimized instances are designed for compute intensive workloads requiring high performance processors
- Memory optimized instances help you deliver super fast performance for processing data sets in memory
- Accelerated computing instance types use hardware accelerators which are great for graphics and data pattern matching
- Storage optimized instances maintain low latency IOPS and sequential read and write access to massive data sets locally

#### Example Question

Q. You are setting up your company's Virtual Private Cloud (VPC). It is time to select the virtual hardware and the software to be provisioned for the instances you will launch within the VPC. You will do this by selecting the Instance Types and Amazon Machine Images (AMI). Which item is not defined by the AMI?

A. Operating System  
B. **Virtual CPUs**  
C. Application or system software  
D. The initial state of patches

### _EC2 Purchase Options_

---

#### On-demand instances

- Can be launched at any time
- Provisioned and available within minutes
- No duration limit
- A flat cost rate paid by the second
- Suitable for short term workloads
- Used for irregular and interruptible workloads
- Stop paying when the resource is stopped or terminated

#### Spot instances

- Bid for unused EC2 compute resources
- Huge cost savings can be achieved
- There is no assurance that you will have spot instances for a fixed period of time
- The instance is purchased when your bid price is higher then the fluctuating spot price
- Spot price fluctuates depending on supply and demand of the unused resource
- If your bid price drops below spot price a two-minute warning is issued before the instance automatically terminates
- Useful for workloads that can be suddenly interrupted

#### Reserved instances

- Purchase instances at a discounted rate over a 1 or 3 year period
- Savings can be as much as 75% compared to on-demand
- All Upfront payment allows you to pay the entire resource for the single or 3 year period, and provides the biggest discount
- Partial Upfront payment allows you to pay a smaller portion upfront and then a discount is applied to any hours used by the instance
- No Upfront payment gives the smallest discount of the 3 options
- Used for long-term predictable workloads

#### Scheduled instances

- Pay for the reservation of an instance on a recurring schedule, either daily, weekly, or monthly
- You are charged for the instance even if you don't use it
- Used to run scheduled workloads that are not continuously running

#### Capacity reservations

- Reserve capacity for EC2 instances based on attributes such as instance type, platform and tenancy, etc
- Reserved within a particular availability zone for any period of time
- Ensures you have the capacity when its required
- Can also be used in conjunction with your reserved instances discount providing you additional savings

#### Example Question

Q. A company wants to implement a low-cost CI/CD solution for its development team. They will use Jenkins as the CI/CD software and an EC2 instance to run it. The company's CI/CD pipeline is configured to tolerate intermittent outages while processing Jenkins jobs. What EC2 instance purchase option would be the most appropriate and cost-effective choice?

A. On-Demand instances  
B. **Spot instances**  
C. Reserved instances  
D. Scheduled instances

### _EC2 Tenancy_

---

- Tenancy relates to how many customer are using the underlying hardware that your EC2 instance is running on
- By default, your instance will be configured to run on shared tenancy
- Dedicated tenancy ensures that your AWS account is the only customer to run EC2 instances on a single host
- Dedicated tenancy can be used to meet security compliance controls
- Dedicated tenancy comes at an increased cost
- Dedicated hosts have the same principle as the dedicated instance, but provides additional flexibility and control
- Dedicated hosts provide EC2 instance placement control and host recovery

#### Example Question

Q. You are leading the design of your company's new AWS cloud environment. Your team is focusing on the options for instances within a VPC. You present the concept of tenancy options for instances to the team. It is important to understand tenancy options and use cases for each option. What tenancy options are provided for Amazon EC2 instances? (Choose 3 answers)

A. **Shared Tenancy**  
B. Dual Tenancy  
C. **Dedicated Instances**  
D. **Dedicated Hosts**  
E. Multi-Tenancy

### _User Data & Metadata_

---

#### User Data

- User data allows you to run commands during the first boot cycle of your new instance
- Useful to install software from a repository or perform OS updates

#### Metadata

- Used to gather and query instance data that is running, such as the host name, events, and security groups etc

### _EC2 Storage_

---

- 2 types of storage, Ephemeral and EBS
- Ephemeral storage physically resides on the same host as the EC2 instance
- Ephemeral storage is temporary, if the instance stops or terminates then the data will be lost. If it just restarts, the data is retained
- EBS provides persistent storage as volumes
- If your instance is stopped or terminated, EBS will retain your data
- EBS volumes can be encrypted if you are storing sensitive data
- EBS backups can be taken as snapshots and stored on Amazon S3

#### Example Question

Q. In which of the following scenarios will data be lost from an EC2 instance store? (Choose 2 answers)

A. **The instance stops**  
B. The instance reboots  
C. **Disk Drive failure**  
D. Network failure

### _EC2 Security_

---

- Key pairs are used to encrypt the credentials to the instance
- Being a key pair, there is a public key and private key
- The public key encrypts the username and password for Windows instances
- The private key is used to decrypt this data
- Connectivity for Windows instances is managed over RDP (Port 3389)
- For Linux instances the private key is used in conjunction with the public key to remotely connect onto the instance via SSH (Port 22)
- The public key is held and kept by AWS
- The private key is kept by you and you only have one opportunity to download it when it's created
- You can use the same key pair for multiple instances

#### Example Question

Q. With regards to Amazon EC2 instances, what is the function of key pairs?

A. To encrypt data held on EBS volumes using AES-256 cryptography and then decrypt the data to be read again  
B. To encrypt and decrypt passwords for AWS IAM account on EC2 resources  
C. **To encrypt the login information for Linux and Windows EC2 instances, and then decrypt the same information allowing you to authenticate into the instance**  
D. To safely make programmatic API calls over an encrypted channel

### _EC2 Auto Scaling_

---

- Allows you to automatically increase or decrease your EC2 resources to meet the demands of your applications based on defined metrics and thresholds
- Optimizes the cost of your EC2 fleet by scaling in resources that are no longer required
- **Scaling Up**: Increases the compute power of your instance, for example going from a M3-large to a M3-Xlarge
- **Scaling Out**: Adds more instances of the same instance type, for example going from two M3 large instances to four M3 large instances

![scaling.png](https://i.imgur.com/41gUuUf.png)

- Launch templates outline the AMI, instance type, IP requirements, storage, etc when launching a new instance
- Auto scaling groups define the desired capacity of the group using scaling policies, and which AZs should be used

### _Elastic Load Balancers_

- ELBs help to control the flow of inbound requests destined to a group of targets
- Requests are distributed evenly across the targeted resource group
- Targets can be a fleet of EC2 instances, Lambda functions, a range of IP addresses, or even containers split across different Availability Zones or in a single AZ
- Internet facing ELBs are accessible via the internet and so have a public DNS name that can be resolved with a public IP address
- Internal ELB only has an internal IP address. It will only serve requests that originate from within your VPC itself
- ELBs will automatically scale to meet your incoming traffic
- To receive encrypted traffic over HTTPS your ELB needs a server certificate
- The certificate must be issued by a certificate authority (CA) which could be **AWS Certificate Manager** (ACM)
- ELB allows you to manage loads across your target groups whereas EC2 auto scaling allows you to elastically scale those target groups based upon the demand
- There are 3 types of ELB:
  - Application
  - Network
  - Classic

#### Load Balancer Types

- Application ELB
  - Operated at the application layer
  - When it receives a request, it used the configured listeners and rules to determine which target group to direct traffic to
  - Offers a flexible feature set for web apps running HTTP/HTTPS
  - Offers TLS termination
  - Cross-zone load balancing is always enabled
- Network Load Balancer
  - Operates at Layer 4 of the OSI model
  - Balance requests purely based on the TCP and UDP protocols
  - Ultra-high performance with low latency
  - Capable of handling millions of requests a second
  - Cross-zone load balancing on the NLB can be enabled or disabled
- Classic Load Balancer
  - Minimal feature set
  - It is considered best practice to use the ALB over the classic load balancer
  - Used for applications running in the EC2-Classic network

#### Example Question

Q. A telecommunications company is developing an AWS cloud data bridge solution to process large amounts of data in real-time from millions of IoT devices. The IoT devices communicate with the data bridge using UDP. The company has deployed a fleet of EC2 instances to handle the incoming traffic but needs to choose the right Elastic Load Balancer to distribute traffic between the EC2 instance.  
Which Amazon Elastic Load Balancer is the appropriate choice in this scenario?

A. **Network Load Balancer**  
B. Application Load Balancer  
C. Classic Load Balancer  
D. Gateway Load Balancer

### _AWS Lambda_

---

- AWS Lambda is a serverless compute service designed to run application code without having to manage and provision your own EC2 instances
- You only pay for compute power when Lambda functions are invoked
- In addition to compute power, you are also charged based on the number of times your code runs
- You can also upload your code using the Management Console, AWS CLI, or the SDK
- Lambda functions execute your code upon specific triggers from supported event sources such as S3
- Event sources are AWS services that can be used to trigger your Lambda functions
- Downstream resources are resources that are required during the execution of your Lambda function
- Logging streams help you identify if your code is operating as expected
- Event sources can either be poll or push-based
- Event source mapping is the configuration that links your event source to your Lambda function
- Monitoring statistics related to your Lambda function within CloudWatch is configured by default
- A dead-letter queue is used to receive payloads that were not processed due to a failed execution
- The number of invocations determines how many times a function has been invoked

#### Example Question

Q. Which of the following best describes the pricing model for AWS Lambda

A. You are charged only for the number of requests  
B. You are charged only for the duration of the lambda function when invoked  
C. You are charged a flat fee based on the memory allocation selected  
D. **You are charged for both the number of requests and the duration of the lambda function when invoked**
