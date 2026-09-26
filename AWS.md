## Old Way before AWS:

Servers were setup on-premise means on-site , inside the office servers were setup to run all the stuff and to manage those servers we had network administrators

## AWS Infrastructure

!image.png

# **Components Of AWS Global Infrastructure**

### **1. Data Center**

- A data center is a physical facility that hosts servers, networking equipment, and storage systems.
- Running applications across multiple data centers improves availability and fault tolerance.
- If one data center fails, workloads can continue running in another location.
- Data centers can also cache content to improve response times for global users.

### **2. Availability Zone (AZ)**

- **An**  Availability Zone has data centers inside it
- **AWS Availability Zones (AZs)** are physically separate data centers within an AWS Region.
- Each AZ includes one or more data centers with independent power, networking, and connectivity.
- AZs are connected through low-latency, high-throughput networks with encrypted traffic.
- A Region contains multiple AZs to ensure high availability and fault tolerance.
- Many AWS services replicate data across AZs to protect against failures and outages.

### **3. Point-of-Presence (PoP)**

AWS Global Infrastructure includes a globally distributed network of Points of Presence (PoPs), which consist of Edge Locations and Regional Edge Caches.

- **Primary Function:** PoPs serve as the "front door" for AWS edge services like Amazon CloudFront (CDN), AWS Global Accelerator, and Amazon Route 53 (DNS).
- **Edge Caching:** They deliver content with ultra-low latency by caching data closer to end users. If a requested file is in the Edge Location, it is served immediately without hitting the origin server.
- **Regional Edge Caches:** These sit between Edge Locations and your origin server. They have larger caches to hold content that isn't popular enough for every Edge Location but still needs to stay close to users to reduce origin load.
- **Security at the Edge:** PoPs also provide a first line of defense, hosting services like AWS Shield (DDoS protection) and AWS WAF, which filter malicious traffic before it ever reaches your VPC.

*Components of Global Infrastructure*

!frame_31

*Components of Global Infrastructure*

### **4. Region**

A region is where you have atleast 3 or more than 3 Availability Zones.

A Region is a physical location in the world where AWS has multiple Availability Zones. When managing resources, you must understand the "Context" of the tool you are using.

**Management Context:** In the Console, CLI, or SDK, you typically specify a Target Region (e.g., us-east-1). For Global services (like IAM), the Region selector will automatically switch to "Global."

**Selection Criteria:**

- **Proximity:** Minimize latency by choosing Regions closest to your user base.
- **Compliance:** Meet data residency laws (e.g., GDPR or GovCloud for sensitive US government data).
- **Service Availability:** Not all services are available in every Region (e.g., new AI services often land in us-east-1 first).
- **Cost Optimization:** Pricing varies by Region. For example, us-east-1 is often cheaper than ap-south-1 (Mumbai).

*Availability Zones*

!availability_zone

*Availability Zones*

### **5. Edge Locations**

- Edge locations are part of the AWS Content Delivery Network and are designed for low-latency, high-throughput content delivery.
- They are globally distributed and use Amazon’s high-speed network to cache content close to end users.
- Services that use edge locations include **Amazon CloudFront** and Lambda@Edge for content caching and edge computing.
- AWS follows a pay-as-you-go model, with free data transfer from AWS origins (such as S3, EC2, and ELB) to edge locations, and charges only for data transferred out to users.
- Cached content is served from the nearest edge location, reducing latency and cost compared to delivering content directly from the origin server.

### **6. Regional Edge Cache**

- A Regional Edge Cache sits between **AWS edge locations** and origin servers in the CloudFront CDN.
- It caches larger or less frequently accessed objects that may not be stored at edge locations.
- When content isn’t in an edge cache, it is retrieved from the regional edge cache, improving delivery efficiency and reducing latency.

## Capital Expense vs Operational Expense(CapEx vs OPex)

- **CAPEX** = upfront spending on assets. 💰🏢*Example: Buy your laptops*
- **OPEX** = ongoing operating expenses. 💸*Example: Cost of energy,internet . cost of running*

## AWS HAS 2 Options to use it :  CLI & CONSOLE

<aside>
💡

When we create an aws account, our account is created as root and root means the admin so that account has all the access

</aside>

## On-Demand vs Spot Instances (IMP FOR INTERVIEW)

## Application Load Balancer vs Network Load Balancer

Application load balancer runs on application layer of OSI MODEL while load balancer runs on 4th layer. 

SOME COMMON ABBREVATIONS:

- ALB - Application Load Balancer
- ELB - Elastic Load Balancer (load balancer service given by aws)
- NLB - Network Load Balancer
- WAF - Web Application Firewall

## AWS EC-2 Auto Scaling Groups

!image.png

When we scale our instances by adding more instances, our users only know the address of 1 instance/machine/IPmaptoDomain.  How will our users go to other instances? The answer is that requests of users will first go to a LOAD BALANCER that load balancer will redirect the requests based on how much load is in our instances.

AutoScaling means if too much traffic comes to this machine I will create more instances like the same as first instance based on traffic/load.

**Target Group** acts as **a logical grouping that routes incoming network requests from an Elastic Load Balancer (ELB) to one or more registered backend resources**

WAF - Web Application Firewall is a service used to eliminate the malicious traffic, we can configure WAF rules to make it functional, IMP “ we attach our waf to our load balancer” .

You can also generate fake traffic using stress on your instance to test the whole stuff.

[]()

<aside>
💡

3 Things to remember:

when the elastic load balancer auto scales our infrastructure , minimum if very less users our on website so at minimum how much instances should we have

1. minimum: so when less users how much min instances should we have atleast
2. desired: when normal how much should we have
3. maximum: when alot of users, how much maximum instances we should allow auto scale to create
</aside>

## AWS S3 Buckets

Buckets store our data in form of objects

We can also give versions to a bucket

Everything stored inside a bucket is automatically stored after encryption and when we download our file/folder from s3 bucket it decrypts for us  

There is a feature in AWS S3 Buckets called object lock in advance settings that works on the principle of Write once read many, prevents deletion of any data inside the bucket. read about it from gpt or google

In order to delete a bucket we need to first delete all of the data stored inside it, we can only delete a bucket when it is empty

Also when using the static website hosting option in the bucket, we can use redirect rules also to redirect something like if a user does /abt we should redirect it to /about

Also can work on Bucket Policies

Which Amazon S3 storage class has the lowest cost? 

### HAS 2 Types of Access:

1. Public Access: Everyone can see the objects stored inside the bucket 
2. Private Access: Only people who has the Access of S3 bucket can see the objects

> IMPORTANT::  By default even when we gives the bucket public access, we still also have to give the read access from bucket policy to read the bucket.
> 

## RDA (Relational Database):

like Mysql and Postgress

<aside>
💡

MYSQL COMMANDS:

- mysql -u root -p (to connect to a locally running mysql db , here local means the machine running this command on)
    - mysql -u username -h remoteDBEndpoint -P 3306 -p ( NEED SOME CONNECTION SETTINGS BEFORE THIS….  to connect to a remote db)
</aside>

## Dynamo Db:

NON Relational like Mongodb  

## ECS:

A serverless architecture service allows us to run docker containers without thinking about scaleability, we can also run our container on normal ec2 but there we will have to think and setup a number of auto-scaling stuff. Here It will run the same ec2 but with auto-scaling whenever needed 

!image.png

We take a normal ec2 instance which will pull code from github and will build docker image inside it and pushes the image to ECR using IAM policies to give access too. Then we tell our ecs  to pull images from ECR and then we also optionally can send metrices to Cloud Watch 

!image.png

We Always Have to Create ECS Clusters to use ECS and can run any number of task definitions inside one ecs cluster, Running a docker Container is a task, and to run that task we have to create Task Definitions . In this task definition we will tell which docker image to use, how much ram, storage, server and will tell about any env variables  we can create multiple task definitions inside a ecs and they can run simultaneously, Here you will see something called services which means we can run/see our multiple containers there.also known as fargate. ALSO have to run these task definitions by our self once.

CAN CHOOSE WHERE TO RUN FARGATE(SERVERLESS) OR SIMPLE EC2

!image.png

## ECR:

ECR is the place where we push our docker images and run them on ecs

## VPC:

!image.png

ARCHITECTURE DIAGRAM IMP FOR INTERVIEW

We create VPC and then creates Subnets inside it , We specify total IP addresses to allocate through CIDR in VPC then we create subnets and assign IP addresses from those allocated IP address to a subnet EG: taking 220 IP address in VPC we can give all these IP’s to 1 subnet or can also create 3 subnets and can give 70 to 1 subnet 30 IP’s to another subnet and the remaining IPs out of 220-70-30 =120 to 3rd Subnet NOW if we create a EC2 instance inside our newly created vpc and then we can select the subnet to which create a ec2 inside . That instance will have one of those IP’s we just assigned to that subnet.  After creation of that ec2 instance inside our subnet ,if we try to connect to that instance we will fail and the reason is that ec2 instance is inside a VPC and a subnet to give that VPC public access we need something known as Internet Gateway, Now lets use  Internet gateway service But we have created a Internet Gateway now it allows the outside requests/traffic to enter inside our VPC but doesn’t know where to send it , where to route it so we will have to create a Route Table . Then after creating the route table we have to tell it where to route the traffic To do so , we have to go inside route table and click on subnet association and assign it the subnet to route to and enable auto-assign ipv4 from there too. Now go and attach the Internet gateway to vpc , can do this from option available there on Internet Gateway. Now our Internet gateway only doesn’t know about our Route table So Next we will go to Route Table  and then edit routes and in destination we will enter 0.0.0.0/0 means anywhere and in target we will give our Internet Gateway

## VPC PEERING :

A way to connect 2 VPCs , also  we have to configure route table for it after vpc peering to connect.

A way to connect 2 VPCs , also  we have to configure route table for it after vpc peering to connect.

## EKS

!image.png
