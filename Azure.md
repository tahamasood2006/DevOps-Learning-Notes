# *Azure Hierarchy:*

!image.png

## Recommended Setup of Subscriptions

!image.png

## Azure Management Group (IMP):

!image.png

!image.png

!image.png

!ChatGPT Image Sep 24, 2026, 12_03_22 AM.png

## Azure Regions:

!image.png

## Availability Zone

!image.png

## Region Pairing in Azure

!image.png

!image.png

# *Azure Management Infrastructure:*

Management infrastructure helps user manage, monitor and optimize their cloud resources

## Ways to create a resource on Azure

!image.png

## Resource Group

!image.png

# *Azure Compute Services:*

Azure runs 3 major compute services including azure virtual machines, azure functions and azure containers.

I'll teach these in a logical order and point out where the AWS equivalent is useful.

---

# 1. The Big Picture

Think of Azure's compute services like this:

```
                    Azure Compute
                         │
        ┌────────────────┼─────────────────┐
        │                │                 │
       VM              App Service       Functions
        │                │                 │
   Full control      Managed web app    Serverless
        │
        ├── VM Scale Sets
        │
        └── Availability Sets

Containers
    │
    ├── Azure Container Instances
    ├── Azure Container Apps
    └── AKS
         │
         └── Kubernetes

Container Images
    │
    └── Azure Container Registry (ACR)
```

The easiest way to understand Azure is to start from **VM → scaling → availability → containers → serverless**.

---

# 2. Azure Virtual Machines

This is the easiest one because you already know AWS.

### Azure VM ≈ AWS EC2

An Azure VM is simply a virtual server running in Azure.

For example:

```
Azure
 │
 └── VM
      ├── Ubuntu
      ├── 4 vCPU
      ├── 16 GB RAM
      ├── Disk
      └── Network Interface
```

You choose things like:

- OS
- CPU
- RAM
- Disk
- Networking
- Security rules
- SSH access

### AWS → Azure

| AWS | Azure |
| --- | --- |
| EC2 | **Virtual Machine** |
| AMI | VM Image |
| EBS | Managed Disk |
| Security Group | Network Security Group (NSG) |
| Elastic IP | Public IP |
| VPC | Virtual Network (VNet) |
| Subnet | Subnet |
| EC2 instance type | VM size |

So if you understand EC2, **Azure VM should be very easy**.

## Custom Data:

Here we can write commands to pre-install something in vm.

## Data disk and OS disk:

here we have 2 options for disk OS disk is mandatory also we can use data disk and mount it.

---

# 3. VM Scale Set (VMSS)

Now imagine you have an application running on one VM:

```
          Users
            │
            ↓
         VM #1
```

Suddenly traffic increases.

You need:

```
          Users
            │
       Load Balancer
       /      |      \
      ↓       ↓       ↓
   VM #1    VM #2    VM #3
```

A **Virtual Machine Scale Set (VMSS)** manages a group of identical VMs for you.

It can:

- create multiple VM instances
- scale them out/in ⇒ out means increase in resources and in means decrease back to normal size
- maintain the desired number of instances
- integrate with load balancing
- perform automatic scaling based on metrics

### AWS equivalent

Think:

**Azure VMSS ≈ EC2 Auto Scaling Group**

Not a perfect 1:1 feature mapping, but that's the correct mental model.

```
AWS                         Azure

EC2                    →    VM
Auto Scaling Group     →    VM Scale Set
Elastic Load Balancer  →    Azure Load Balancer
```

### Example

You configure:

```
VMSS
├── minimum = 2
├── maximum = 10
└── desired = 2
```

Traffic increases:

```
2 VMs
 ↓
4 VMs
 ↓
7 VMs
```

Traffic decreases:

```
7 VMs
 ↓
3 VMs
 ↓
2 VMs
```

---

# 4. VM Availability Set

This one is important because it introduces **Fault Domains and Update Domains**.

Suppose you have:

```
VM #1
VM #2
VM #3
```

You don't want all three VMs to be affected by the same physical failure or maintenance event.

An **Availability Set** helps distribute VMs across different fault/update groupings within an Azure datacenter.

Think:

> "Azure, please don't put all my important VMs in the same failure/maintenance bucket."
> 

For example:

```
Availability Set

Fault Domain 0       Fault Domain 1
     │                    │
   VM #1                VM #2
   VM #3                VM #4
```

If a particular physical infrastructure component fails, Azure can avoid taking all your VMs down together.

---

# 5. Fault Domains

A **Fault Domain** represents a group of infrastructure that shares a potential physical failure.

Think of it like:

```
Physical infrastructure

Fault Domain 1
 ├── VM #1
 └── VM #3

Fault Domain 2
 ├── VM #2
 └── VM #4
```

If something happens to Fault Domain 1:

```
❌ VM #1
❌ VM #3

✅ VM #2
✅ VM #4
```

The goal is **failure isolation**.

### AWS mental model

The closest AWS concept is distributing instances across different **Availability Zones**.

But don't consider them identical.

An Azure Availability Set is **not the same thing as an Azure Availability Zone**.

---

# 6. Update Domains

Fault Domains handle **hardware/infrastructure failure**.

Update Domains handle **planned maintenance**.

Imagine Microsoft needs to update the underlying infrastructure.

Instead of doing:

```
Update everything simultaneously

VM1 ❌
VM2 ❌
VM3 ❌
VM4 ❌
```

Azure groups VMs into update domains:

```
Update Domain 1
 VM1
 VM2

Update Domain 2
 VM3
 VM4
```

Azure can update one group at a time.

```
UD1 → maintenance
UD2 → still running

then

UD2 → maintenance
UD1 → running
```

So remember:

> **Fault Domain = unplanned physical failure**
> 

> **Update Domain = planned maintenance**
> 

---

# 7. Availability Set vs Availability Zone

This is an important distinction.

### Availability Set

```
One Azure datacenter
       │
       └── Availability Set
             ├── Fault Domain
             └── Update Domain
```

### Availability Zone

```
Azure Region
│
├── Zone 1
│    └── Datacenter
│
├── Zone 2
│    └── Datacenter
│
└── Zone 3
     └── Datacenter
```

Availability Zones provide stronger isolation because the zones are physically separate within a region.

### AWS comparison

```
AWS                         Azure

Availability Zone      ≈   Availability Zone
EC2 across AZs         ≈   VMs across Azure Zones
```

For modern highly available architectures, **Availability Zones are generally the more important concept to understand**.

Availability Sets are still useful to know, especially because you'll encounter them in Azure documentation and existing infrastructure.

---

# 8. Lift and Shift

This isn't an Azure service.

It's a **migration strategy**.

Imagine your company has:

```
On-premises

Web Server
   ↓
Application Server
   ↓
Database Server
```

You want to move to Azure.

With **Lift and Shift**, you basically say:

> "Let's move this workload to Azure with as few application changes as possible."
> 

So:

```
ON-PREMISES                  AZURE

Physical Server       →      Azure VM
Physical Server       →      Azure VM
Physical Server       →      Azure VM
```

You're essentially **lifting** the workload and **shifting** it to the cloud.

### AWS equivalent

Exactly the same concept exists in AWS.

It's not an Azure-specific concept.

---

# 9. Azure Virtual Desktop (AVD)

This one is different from normal VMs.

Azure Virtual Desktop allows organizations to provide users with **virtual Windows desktops/apps hosted in Azure**.

AVD uses Azure VMs, and those VMs can support multiple user sessions, but AVD is an entire managed virtual-desktop solution, not just a multi-user VM.

Think:

```
Employee's Laptop
       │
       │ Internet
       ↓
Azure Virtual Desktop
       │
       ├── Windows Desktop
       ├── Company Apps
       └── Company Data
```

The user can access their work desktop remotely.

### AWS comparison

The closest AWS services/concepts are:

- Amazon WorkSpaces
- Amazon AppStream 2.0

But AVD is Microsoft's virtual desktop solution.

### Why use it?

For example, a company has 500 employees.

Instead of managing powerful PCs individually:

```
Employee
   ↓
AVD
   ↓
Cloud Windows desktop
```

The desktop environment is managed centrally.

!image.png

---

# 10. Azure Containers

This area is important for you because you already know Docker.

!image.png

Azure doesn't have just **one** "container service."

There are several options.

The major ones you'll encounter are:

```
Azure Containers
│
├── Azure Container Instances (ACI)
│
├── Azure Container Apps
│
└── Azure Kubernetes Service (AKS)
```

Let's separate them.

---

# 11. Azure Container Instances (ACI)

ACI is basically:

> "I have a Docker container. I want Azure to run it without me managing VMs."
> 

!image.png

For example:

```
Docker Image
     │
     ↓
Azure Container Instance
     │
     └── Container running
```

You don't have to manage:

```
VM
OS
VM patching
Kubernetes cluster
```

### AWS comparison

Closest mental model:

**ACI ≈ AWS Fargate/ECS task**

Although the architectures and features aren't identical.

ACI is particularly useful for simple container workloads, jobs, or scenarios where you don't need a full orchestration platform.

---

# 12. Azure Container Apps

This is a higher-level managed container platform.

Think:

```
Docker Image
     ↓
Azure Container Apps
     ↓
Managed application
```

It provides features such as:

- autoscaling
- ingress
- revisions
- microservice-oriented deployments
- integrations with other Azure services

You don't manage Kubernetes directly.

### AWS mental model

It's somewhat comparable to:

**Azure Container Apps ↔ AWS App Runner**

with some overlap with serverless container capabilities in ECS/Fargate.

---

# 13. AKS — Azure Kubernetes Service

This is the big one for you.

You already learned Kubernetes, so this should click quickly.

**AKS = managed Kubernetes on Azure.**

```
             AKS
              │
       Kubernetes Control Plane
              │
       ┌──────┴──────┐
       │             │
    Node 1         Node 2
       │             │
    Pods           Pods
```

Azure manages much of the Kubernetes control plane for you.

You manage/work with:

- Kubernetes workloads
- Deployments
- Services
- Ingress
- ConfigMaps
- Secrets
- Nodes/node pools
- Persistent storage
- networking
- RBAC
- etc.

### AWS comparison

```
AWS                    Azure

EKS             →      AKS
ECS             →      Azure Container Apps / other Azure container services
EC2             →      Azure VM
ECR             →      Azure Container Registry
```

And again:

> **EKS is AWS. AKS is Azure.**
> 

---

# 14. Azure Container Registry (ACR)

This one you'll understand immediately.

You asked:

> "like we had a place in AWS to store Docker images"
> 

**Yes.**

ACR is Azure's private container registry.

### AWS

```
Docker
  ↓
ECR
  ↓
myapp:v1
myapp:v2
```

### Azure

```
Docker
  ↓
ACR
  ↓
myapp:v1
myapp:v2
```

So:

**Amazon ECR ≈ Azure Container Registry (ACR)**

Example workflow:

```bash
docker build -t myapp:v1 .
```

Then:

```
Docker image
     ↓
     ACR
     ↓
     AKS
     ↓
 Kubernetes Pod
```

This is a very common Azure DevOps workflow.

---

# 15. Azure App Service

This is one of the most important Azure services.. “It is a PAAS”

The simplest explanation:

> **App Service lets you deploy web applications without managing the underlying VM yourself.**
> 

Suppose you have:

```
Django application
```

Normally you could do:

```
Django
 ↓
Linux VM
 ↓
Configure Python
 ↓
Configure Gunicorn
 ↓
Configure Nginx
 ↓
Configure systemd
 ↓
Manage OS updates
```

With App Service:

```
Django
   ↓
Azure App Service
```

Azure manages much of the underlying infrastructure.

It supports common application stacks such as:

- .NET
- Java
- Node.js
- Python
- PHP
- containers

---

# 16. App Service vs VM

This distinction is important.

### VM

You manage much more:

```
VM
├── OS
├── packages
├── patches
├── runtime
├── application
└── configuration
```

### App Service

Azure manages the underlying infrastructure: Also gives domain like .azurewebsites but we can choose costum domains also from inside

```
Azure-managed infrastructure
        │
        ↓
   App Service
        │
        ↓
   Your application
```

So:

> **VM = more control**
> 

> **App Service = more managed**
> 

!image.png

---

!image.png

## Deployment Slots:

Sure. **Deployment Slots** are an Azure App Service feature that lets you run **different versions of your application side-by-side**.

Think of it like having a **production copy and a testing copy**.

### Without deployment slots

You have:

```
Users
  ↓
App Service
  ↓
Version 1.0
```

You build version 2.0 and deploy it directly:

```
Users
  ↓
App Service
  ↓
Version 2.0
```

If something goes wrong, users immediately experience it.

---

## With Deployment Slots

You can have:

```
App Service
│
├── Production slot
│      └── Version 1.0
│
└── Staging slot
       └── Version 2.0
```

Your users continue using:

```
Users
  ↓
Production
  ↓
Version 1.0
```

While you test:

```
You
 ↓
Staging
 ↓
Version 2.0
```

---

## Then comes the cool part: SWAP 🔄

Once you've tested version 2.0:

```
Before:

Production → v1
Staging    → v2
```

You **swap** the slots:

```
After:

Production → v2
Staging    → v1
```

So you can move the new version into production without doing a traditional "deploy directly to production."

---

## Why is this useful?

Imagine you're deploying a Django application.

### Without slots:

```
Deploy v2
   ↓
Production
   ↓
😰 Something breaks
```

### With slots:

```
Deploy v2
   ↓
Staging
   ↓
Test everything
   ↓
Looks good ✅
   ↓
SWAP
   ↓
Production
```

This is often called **blue-green style deployment**.

---

# AWS comparison

Since you know AWS:

The closest mental model is **Elastic Beanstalk environments / blue-green deployment**, although Azure App Service deployment slots are a specific App Service feature.

Think:

```
Azure App Service

Production
    ↕ SWAP
Staging
```

---

## One important detail

Slots aren't completely separate App Services.

They're **deployment environments within the same App Service app**.

For example:

```
MyApp Service
│
├── Production
├── Staging
└── Testing
```

Each slot can have its own deployed application version.

---

### The easiest thing to remember

> **Deployment slot = a separate environment for your App Service where you can deploy and test a new version before swapping it into production.**
> 

```
        App Service
             │
      ┌──────┴──────┐
      ↓             ↓
 Production       Staging
    v1              v2
      │             │
      └──── SWAP ───┘
             ↓
       Production v2
```

This is particularly useful in **CI/CD**, because your pipeline can deploy to **staging → test → swap to production**.

# 17. Azure Functions

This is Azure's serverless function platform.

You write a small piece of code:

```python
def process_order():
    ...
```

Azure runs it when something triggers it.

For example:

```
HTTP request
     ↓
Azure Function
     ↓
Python code
```

Or:

```
Blob uploaded
     ↓
Azure Function
     ↓
Process file
```

Or:

```
Queue message
     ↓
Azure Function
     ↓
Process message
```

### AWS comparison

This one is almost direct:

**AWS Lambda ≈ Azure Functions**

---

# 18. VM vs App Service vs Container Apps vs AKS vs Functions

This is probably the **most important comparison** from everything you're learning.

| Azure | Think of it as | AWS mental model |
| --- | --- | --- |
| **VM** | Full virtual server | EC2 |
| **VM Scale Set** | Group of scalable VMs | EC2 Auto Scaling |
| **App Service** | Managed web application platform | Elastic Beanstalk-ish / App Runner depending on workload |
| **Container Instances** | Run containers simply | Fargate task-ish |
| **Container Apps** | Managed container application platform | App Runner-ish |
| **AKS** | Managed Kubernetes | EKS |
| **Functions** | Serverless functions | Lambda |
| **ACR** | Container image registry | ECR |
| **AVD** | Cloud Windows desktops | WorkSpaces-ish |

---

# 19. The "How Do I Choose?" Mental Model

Imagine you're deploying a Python application.

### Option 1 — I want a server

```
Python App
    ↓
Azure VM
```

You manage the server.

---

### Option 2 — I want a managed web application

```
Python App
    ↓
Azure App Service
```

Azure handles more infrastructure.

---

### Option 3 — I have a Docker container

```
Docker Image
    ↓
Azure Container Apps
```

Azure handles the infrastructure/orchestration layer for you.

---

### Option 4 — I have simple container workload

```
Docker Image
    ↓
Azure Container Instance
```

No Kubernetes cluster needed.

---

### Option 5 — I need Kubernetes

```
Docker Image
      ↓
     ACR
      ↓
     AKS
      ↓
Kubernetes
      ↓
    Pods
```

This is the Azure equivalent of your:

```
Docker Image
      ↓
     ECR
      ↓
     EKS
      ↓
Kubernetes
      ↓
    Pods
```

---

### Option 6 — I have event-driven code

```
Event
  ↓
Azure Function
  ↓
Code executes
```

Think:

```
AWS Lambda
     ≈
Azure Functions
```

---

# 20. One Architecture You Should Understand

Since you're learning DevOps, this is a very useful Azure architecture to understand:

```
                    Internet
                       │
                       ↓
                  Azure Front Door
                       │
                       ↓
                 Application Gateway
                       │
                       ↓
                      AKS
                 ┌─────┴─────┐
                 │           │
               Node         Node
                 │           │
               Pods         Pods
                 │           │
                 └─────┬─────┘
                       │
                 Azure Database
                       │
                 Azure Storage

Docker Image
     │
     ↓
     ACR
     │
     ↓
    AKS
```

And your DevOps pipeline could look like:

```
Developer
    │
    ↓
GitHub
    │
    ↓
GitHub Actions / Azure DevOps
    │
    ├── Build Docker image
    │
    ├── Push image → ACR
    │
    └── Deploy → AKS
```

That should feel very familiar because the AWS version would be:

```
GitHub
   ↓
CI/CD
   ↓
ECR
   ↓
EKS
```

---

# 21. What You Should Learn Next

Since you've already learned **Azure Resource Management / Resource Groups**, I would learn Azure in this order:

```
1. Azure VM
       ↓
2. VM Scale Sets
       ↓
3. Availability Sets
       ↓
4. Fault Domains / Update Domains
       ↓
5. Availability Zones
       ↓
6. Lift & Shift
       ↓
7. Azure App Service
       ↓
8. Azure Functions
       ↓
9. Azure Container Instances
       ↓
10. Azure Container Apps
       ↓
11. Azure Container Registry
       ↓
12. AKS
       ↓
13. AKS networking
       ↓
14. AKS storage
       ↓
15. AKS + ACR + CI/CD
```

And **don't skip Availability Zones just because you learned Availability Sets**. They solve related but different availability problems, and AZs are especially important when you're designing highly available production infrastructure.

If your goal is **Azure DevOps/Cloud/DevOps engineering**, the especially important chain to master is:

```
VM
 ↓
VMSS
 ↓
VNet + NSG
 ↓
Load Balancer
 ↓
App Service
 ↓
Docker
 ↓
ACR
 ↓
AKS
 ↓
Azure Storage
 ↓
Azure Monitor
 ↓
Terraform
 ↓
CI/CD
```

That gives you a very solid Azure equivalent of the AWS infrastructure/DevOps knowledge you're already building.

# Azure Virtual Networks

Absolutely. Since you already understand AWS networking, I'll teach Azure networking as **"AWS VPC translated into Azure"**. The goal is not just memorizing services—you should understand **where traffic goes and why**.

Microsoft describes VNet/subnets as the foundation of Azure networking, with connectivity, load balancing, security, private access, DNS, and monitoring built around them. Microsoft Learn

# 1. The most important mapping: AWS → Azure

Start with this:

| AWS | Azure |
| --- | --- |
| **VPC** | **VNet** |
| Subnet | Subnet |
| Route Table | Route Table / UDR |
| Security Group | **NSG** |
| ENI | **NIC** |
| Internet Gateway | Azure's VNet internet connectivity model* |
| NAT Gateway | NAT Gateway |
| VPC Peering | VNet Peering |
| Transit Gateway | Virtual WAN / hub-and-spoke patterns |
| Site-to-Site VPN | VPN Gateway |
| Direct Connect | ExpressRoute |
| PrivateLink | Private Link |
| Route 53 | Azure DNS |
| ALB | Application Gateway |
| NLB | Azure Load Balancer |
| CloudFront | Azure Front Door |
| WAF | Azure WAF |
| AWS Network Firewall | Azure Firewall |
| Bastion | Azure Bastion |
| EKS | AKS |
- Azure networking doesn't have a standalone "Internet Gateway" resource that maps 1:1 to AWS IGW.

---

# 2. What is an Azure VNet?

If you understand an AWS VPC, you already understand the basic idea.

**VNet = your private network inside Azure.**

Imagine:

```
                    Azure
                      │
                 ┌────▼────┐
                 │   VNet  │
                 │10.0.0.0/16
                 └────┬────┘
                      │
          ┌───────────┼───────────┐
          │           │           │
       Web Subnet  App Subnet  DB Subnet
       10.0.1.0/24 10.0.2.0/24 10.0.3.0/24
```

You decide the VNet's address space.

For example:

```
10.0.0.0/16
```

Then divide it into subnets:

```
VNet: 10.0.0.0/16

web-subnet: 10.0.1.0/24
app-subnet: 10.0.2.0/24
db-subnet:  10.0.3.0/24
```

A VNet is **regional** and can span availability zones within that region. Microsoft Learn

---

# 3. What is a Subnet?

Exactly like AWS.

A subnet is simply a **smaller IP range inside your VNet**.

```
VNet
10.0.0.0/16
│
├── Web subnet
│   10.0.1.0/24
│
├── App subnet
│   10.0.2.0/24
│
└── Database subnet
    10.0.3.0/24
```

You use subnets to:

- organize resources
- separate workloads
- apply NSGs
- apply route tables
- create security boundaries

Azure resources in the same VNet can communicate through Azure's system routes by default, while separate VNets need an explicit connection such as peering or VPN. Microsoft Learn

> In Azure, if both subnets are inside the **same VNet**, they can communicate by default. “IMP”
> 

### Example

```
                 Azure VNet
              10.0.0.0/16
                    │
          ┌─────────┴─────────┐
          │                   │
   Public Subnet         Private Subnet
   10.0.1.0/24           10.0.2.0/24
       │                       │
    Web VM                  App VM
   10.0.1.10               10.0.2.10
       │                       │
       └───────────┬───────────┘
                   │
              VNet routing
```

The Web VM can communicate with the App VM using its **private IP**:

```
10.0.1.10 → 10.0.2.10
```

You don't need NAT Gateway for that.

---

### Then what does NAT Gateway do?

Suppose your private App VM needs to download something from the Internet:

```
Private VM
10.0.2.10
    │
    ↓
NAT Gateway
    │
    ↓
Internet
```

NAT Gateway provides **outbound Internet connectivity** without requiring the VM to have its own public IP.

So:

| Requirement | What you use |
| --- | --- |
| Public subnet → Private subnet | **VNet routing** |
| Private subnet → Public subnet | **VNet routing** |
| Private VM → Internet | **NAT Gateway** |
| Internet → Private VM | Usually **not directly allowed**; use an appropriate public entry point such as Application Gateway/Load Balancer |
| Control traffic between subnets | **NSG** |

### One more important thing

In Azure, **"public subnet" and "private subnet" aren't formal subnet types in the same sense people often mean in AWS.**

A subnet doesn't become public simply because you call it `public-subnet`.

It's more about whether resources have **public IPs** and how their routing is configured.

For example:

```
VNet
│
├── web-subnet
│    └── VM with Public IP
│
└── app-subnet
     └── VM with only Private IP
```

That's a common architecture.

And you can control:

```
Web VM → App VM     ✅
App VM → Internet   ✅ via NAT Gateway
Internet → App VM   ❌ normally
```

**So remember:**

> **Same VNet = private communication is already possible through Azure's system routing. NAT Gateway is for outbound Internet access, not subnet-to-subnet communication.**
> 

---

# 4. IP Addressing

You'll have:

### Private IP

Example:

```
10.0.1.10
```

Used for internal communication.

```
VM1
10.0.1.10
   │
   │ private network
   ↓
VM2
10.0.2.10
```

### Public IP

Example:

```
20.x.x.x
```

Used when something needs public Internet connectivity.

For example:

```
Internet
    │
    ↓
Public IP
    │
    ↓
Azure resource
```

Azure's VNet documentation recommends planning address ranges carefully, especially when connecting VNets to other networks; overlapping CIDRs can cause problems. Microsoft Learn

---

# 5. Important Azure difference: NIC

In AWS you know:

```
EC2
 ↓
ENI
 ↓
Private/Public IP
```

Azure has:

```
VM
 ↓
NIC
 ↓
Private/Public IP
```

**NIC = Network Interface Card**

A VM connects to a VNet through its NIC.

```
VM
 │
 ↓
NIC
 │
 ↓
Subnet
 │
 ↓
VNet
```

Azure also allows NSGs to be associated with either a subnet or an individual NIC. Microsoft Learn

---

# 6. NSG — Network Security Group

This is one of the most important things.

Think:

**AWS Security Group ≈ Azure NSG**

An NSG contains rules like:

```
Allow TCP 443
Allow TCP 22
Deny everything else
```

For example:

```
Internet
   │
   │ HTTPS :443
   ↓
Web Subnet
   │
   └── NSG
        │
        ├── Allow 443
        ├── Allow 80
        └── Deny unwanted traffic
```

NSGs can control **inbound and outbound** traffic. They can be associated with subnets or NICs. Microsoft Learn

---

# 7. NSG vs Azure Firewall

This distinction is important.

### NSG

Think:

> **"Should this network interface/subnet allow this traffic?"**
> 

```
NSG
 ↓
Basic traffic filtering
```

### Azure Firewall

Think:

> **"I want a centralized firewall inspecting and controlling traffic across my network."**
> 

```
              Azure Firewall
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
       Spoke 1              Spoke 2
```

Azure Firewall is a managed, stateful network firewall; its Standard SKU provides L3-L7 filtering and Premium adds capabilities such as IDPS and TLS inspection. Microsoft Learn

So:

```
NSG       → subnet/NIC-level filtering
Firewall  → centralized network firewall
```

---

# 8. Route Tables

Now we get to **routing**.

Suppose:

```
VM1
10.0.1.10
   │
   ↓
VM2
10.0.2.10
```

How does Azure know where to send traffic?

**Routes.**

Azure automatically provides system routes.

You can also create your own:

**User Defined Routes (UDRs).**

For example:

```
10.0.2.0/24
      ↓
Azure Firewall
      ↓
App subnet
```

So instead of traffic going directly:

```
VM → VM
```

you can force:

```
VM → Firewall → VM
```

UDRs are associated with **subnets** and can override Azure's default system routes. Microsoft Learn

### AWS comparison

```
AWS Route Table
       ≈
Azure Route Table / UDR
```

---

# 9. VNet Peering

Suppose you have two VNets:

```
VNet A                 VNet B

10.0.0.0/16             10.1.0.0/16
   │                        │
   └────────────┬───────────┘
                │
             Peering
```

Now resources can communicate privately.

```
VM in VNet A
     │
     │ private IP
     ↓
VM in VNet B
```

Traffic stays on Microsoft's backbone.

This is:

**AWS VPC Peering ≈ Azure VNet Peering**

Azure supports peering within the same region and across supported regions. Peering is **not transitive**. Microsoft Learn

---

# 10. Why "not transitive" matters

Suppose:

```
VNet A ←→ VNet B ←→ VNet C
```

You might think:

```
A → B → C
```

automatically works.

But normal VNet peering doesn't work that way.

You don't automatically get:

```
A → C
```

just because both are connected to B.

This is one reason Azure architectures often use **hub-and-spoke** networking.

---

# 11. Hub-and-Spoke

This is a very important Azure architecture.

```
                  Hub VNet
                     │
        ┌────────────┼────────────┐
        │            │            │
        ↓            ↓            ↓
     Spoke 1      Spoke 2      Spoke 3
     VNet         VNet         VNet
```

The hub can contain shared networking services:

```
Hub VNet
│
├── Azure Firewall
├── VPN Gateway
├── ExpressRoute
├── Bastion
└── Shared services
```

And application VNets become spokes.

Microsoft documents hub-and-spoke as a common pattern where shared services such as Firewall and VPN/ExpressRoute gateways live in the hub and application workloads live in spoke VNets. Microsoft Learn

---

# 12. VPN Gateway

Now imagine your company has an on-premises network:

```
Company Datacenter
10.10.0.0/16
       │
       │ VPN
       ↓
	Azure VPN Gateway
	       │
	       ↓
	Azure VNet
	10.0.0.0/16
```

This creates connectivity between your on-prem network and Azure.

### AWS comparison

```
AWS Site-to-Site VPN
        ≈
Azure VPN Gateway
```

VPN uses an encrypted connection over the public Internet.

---

# 13. ExpressRoute

Now imagine a company says:

> "I don't want my connection to Azure to travel over the public Internet."
> 

You can use:

**Azure ExpressRoute.**

Conceptually:

```
Company
  │
  │ private connection
  ↓
Provider
  │
  ↓
Microsoft
  │
  ↓
Azure
```

### AWS comparison

```
AWS Direct Connect
        ≈
Azure ExpressRoute
```

ExpressRoute provides private connectivity through a connectivity provider and supports BGP-based routing. Microsoft Learn

---

# 14. Private Endpoint / Private Link

This is **very important for DevOps/cloud architecture**.

Imagine you have:

```
Azure VM
   │
   ↓
Azure Storage
```

Normally you might think about accessing the service through a public endpoint.

With **Private Link + Private Endpoint**, Azure can give the service a **private IP inside your VNet**.

```
VNet
│
├── VM
│
└── Private Endpoint
       │
       ↓
   Azure Storage
```

The private endpoint is essentially a **private IP/NIC inside your VNet representing the service**.

Traffic stays on Microsoft's network rather than requiring public Internet access. Microsoft Learn

### AWS comparison

```
AWS PrivateLink
       ≈
Azure Private Link
```

---

# 15. NAT Gateway

Suppose your private VMs need to access the Internet:

```
Private VM
10.0.1.10
    │
    ↓
NAT Gateway
    │
    ↓
Internet
```

The VM can make **outbound** Internet connections without needing its own public IP.

### AWS comparison

```
AWS NAT Gateway
       ≈
Azure NAT Gateway
```

Azure NAT Gateway is associated with a subnet and provides outbound connectivity for resources in that subnet. Microsoft Learn

---

# 16. Azure Bastion

You have a private VM:

```
VM
10.0.1.10
```

It doesn't have a public IP.

How do you SSH/RDP into it?

Instead of giving the VM a public IP, you can use:

**Azure Bastion**

```
Your Browser
     │
     ↓
Azure Bastion
     │
     ↓
Private VM
```

So:

> **Bastion = secure way to access VMs without exposing their public IPs.**
> 

Microsoft specifically describes Azure Bastion as a way to connect securely to VMs without exposing public IP addresses. Microsoft Learn

AWS comparison:

**AWS Systems Manager Session Manager** is conceptually relevant, though it isn't a direct 1:1 equivalent.

---

# 17. Azure Load Balancer

This operates primarily at **Layer 4** (TCP/UDP).

Imagine:

```
              Internet
                  │
                  ↓
           Azure Load Balancer
             /           \
            ↓             ↓
          VM 1           VM 2
```

It distributes network traffic across backend resources.

### AWS comparison

Think:

**Azure Load Balancer ≈ AWS Network Load Balancer**

It's especially useful for VM/VMSS-based workloads.

---

# 18. Application Gateway

Now you need HTTP/HTTPS-aware routing.

For example:

```
https://example.com/api
          ↓
    Application Gateway
          │
          ├── /api → API servers
          │
          └── /web → Web servers
```

Application Gateway operates at the application layer and supports things such as:

- HTTP/HTTPS routing
- TLS termination
- URL-based routing
- WAF

### AWS comparison

Closest mental model:

**Azure Application Gateway ≈ AWS Application Load Balancer**

---

# 19. WAF

WAF = **Web Application Firewall**

It protects web applications from attacks such as:

- SQL injection
- XSS
- other common web-layer attacks

```
Internet
   ↓
WAF
   ↓
Application
```

Azure WAF can be used with services such as Application Gateway and Front Door. Microsoft Learn

AWS equivalent:

**AWS WAF**

---

# 20. Azure Front Door

Now we're going **global**.

Imagine you have:

```
Users worldwide
       │
       ↓
Azure Front Door
       │
   ┌───┴────┐
   ↓        ↓
US Azure   Europe Azure
Region     Region
```

Front Door is designed for global application delivery, routing, acceleration, and protection.

### AWS comparison

Think:

**Azure Front Door ≈ CloudFront + some global application delivery/routing capabilities**

Don't treat it as a perfect 1:1 mapping.

---

# 21. Traffic Manager

Traffic Manager is another global traffic-routing service, but it works differently from Front Door.

The simple distinction:

**Traffic Manager = DNS-based traffic routing**

```
User
 ↓
DNS
 ↓
Traffic Manager
 ↓
Choose endpoint
```

It can direct users toward different endpoints based on routing methods such as performance, priority, geographic location, etc.

So:

```
Traffic Manager → DNS-level routing

Front Door → Global application delivery/proxy
```

---

# 22. Azure DNS

Pretty straightforward.

You have:

```
www.example.com
```

Azure DNS can host/manage DNS zones and records.

For example:

```
example.com

A record
www → 20.x.x.x
```

### AWS comparison

**Route 53 ≈ Azure DNS**

---

# 23. Private DNS

This becomes especially important with private endpoints.

Suppose:

```
mydatabase.database.windows.net
```

Your application might need that hostname to resolve to a **private IP** rather than a public address.

Private DNS zones help provide that internal name resolution.

Conceptually:

```
Application
    │
    ↓
Private DNS
    │
    ↓
10.0.2.15
    │
    ↓
Private Endpoint
```

---

# 24. Azure Firewall vs NSG vs WAF

This is something you should **definitely memorize**.

| Service | Main purpose |
| --- | --- |
| **NSG** | Control network traffic to subnet/NIC |
| **Azure Firewall** | Centralized network firewall |
| **WAF** | Protect web applications |
| **DDoS Protection** | Protect against DDoS attacks |

Think:

```
                 Internet
                    │
                    ↓
                  WAF
                    │
                    ↓
              Application
                    │
                    ↓
             Azure Firewall
                    │
                    ↓
                  NSG
                    │
                    ↓
               VM / workload
```

That's simplified—actual architecture depends on the workload.

---

# 25. How all of this fits together

Here's a realistic architecture:

```
                         INTERNET
                            │
                            ↓
                    Azure Front Door
                       + WAF
                            │
                            ↓
                  Application Gateway
                       + WAF
                            │
                            ↓
                    ┌─── Azure VNet ───┐
                    │                  │
                    │  Web Subnet      │
                    │      │           │
                    │      ↓           │
                    │   AKS / VMSS     │
                    │                  │
                    │  App Subnet      │
                    │      │           │
                    │      ↓           │
                    │  Application     │
                    │                  │
                    │  Data Subnet     │
                    │      │           │
                    │      ↓           │
                    │ Private Endpoint │
                    │      │           │
                    └──────┼───────────┘
                           ↓
                     Azure Storage
```

And for outbound Internet:

```
Private VM / AKS
      │
      ↓
  NAT Gateway
      │
      ↓
   Internet
```

For administration:

```
Your laptop
    │
    ↓
 Azure Bastion
    │
    ↓
 Private VM
```

For on-premises:

```
On-Prem
   │
   ├── VPN Gateway
   │
   └── ExpressRoute
          │
          ↓
       Hub VNet
          │
       Peering
      /      \
     ↓        ↓
 Spoke A    Spoke B
```

---

# 26. The Azure networking "family tree"

If you're studying for DevOps, this is the hierarchy I'd memorize:

```
AZURE NETWORKING
│
├── FOUNDATION
│   ├── VNet
│   ├── Subnet
│   ├── NIC
│   ├── Private IP
│   └── Public IP
│
├── SECURITY
│   ├── NSG
│   ├── Azure Firewall
│   ├── WAF
│   └── DDoS Protection
│
├── ROUTING
│   ├── System Routes
│   ├── Route Tables / UDR
│   └── Route Server
│
├── CONNECTIVITY
│   ├── VNet Peering
│   ├── VPN Gateway
│   ├── ExpressRoute
│   └── Virtual WAN
│
├── PRIVATE ACCESS
│   ├── Private Link
│   └── Private Endpoint
│
├── INTERNET / OUTBOUND
│   └── NAT Gateway
│
├── LOAD BALANCING
│   ├── Azure Load Balancer
│   ├── Application Gateway
│   └── Front Door
│
	├── DNS
│   ├── Azure DNS
│   ├── Private DNS
│   └── DNS Private Resolver
│
└── MANAGEMENT
    ├── Bastion
    ├── Network Watcher
    └── Azure Monitor
```

Microsoft's current networking documentation organizes Azure networking broadly into foundation, load balancing/content delivery, hybrid connectivity, security, monitoring/management, and container networking. Microsoft Learn

---

# 27. Your AWS → Azure cheat sheet

The most useful part for you:

```
AWS                         AZURE

VPC                    →    VNet

Subnet                 →    Subnet

EC2 ENI                →    NIC

Security Group         →    NSG

Route Table            →    Route Table / UDR

VPC Peering            →    VNet Peering

Transit Gateway        →    Virtual WAN /
                             Hub-Spoke architecture

NAT Gateway            →    NAT Gateway

Site-to-Site VPN       →    VPN Gateway

Direct Connect         →    ExpressRoute

PrivateLink            →    Private Link

PrivateLink endpoint   →    Private Endpoint

Route 53               →    Azure DNS

NLB                    →    Azure Load Balancer

ALB                    →    Application Gateway

CloudFront             →    Azure Front Door

AWS WAF                →    Azure WAF

Network Firewall       →    Azure Firewall

EKS                    →    AKS
```

## The most important mental model

Don't try to memorize 30 Azure networking services individually.

Think about **what problem you're solving**:

```
"I need a private network"
        ↓
      VNet

"I need to divide it"
        ↓
     Subnets

"I need to control traffic"
        ↓
       NSG

"I need custom routing"
        ↓
     UDR/Route Table

"I need VNet → VNet"
        ↓
     Peering

"I need Azure → on-prem"
        ↓
 VPN Gateway / ExpressRoute

"I need private access to PaaS"
        ↓
 Private Endpoint

"I need outbound Internet for private resources"
        ↓
 NAT Gateway

"I need to distribute traffic"
        ↓
 Load Balancer / Application Gateway

"I need global application delivery"
        ↓
 Front Door

"I need web attack protection"
        ↓
 WAF

"I need centralized network firewall"
        ↓
 Azure Firewall

"I need private VM administration"
        ↓
 Bastion
```

That is the **core Azure networking picture**. Once this makes sense, AKS networking becomes much easier because AKS essentially sits **inside/alongside this networking foundation** rather than being a completely separate networking world.

# *Azure Storage*

Absolutely. Since you already know AWS, I’ll teach **Azure Storage by mapping it to AWS**, just like we did with Azure Compute.

# ☁️ Azure Storage — Complete Guide

Think of Azure Storage as **Azure's family of managed storage services**.

The main services you need to know are:

| Azure | AWS equivalent | Used for |
| --- | --- | --- |
| **Blob Storage** | S3 | Files, objects, backups, images, videos |
| **Managed Disks** | EBS | VM disks |
| **Azure Files** | EFS / FSx-ish | Shared file system |
| **Queue Storage** | SQS-ish | Message queues |
| **Table Storage** | DynamoDB-ish | NoSQL key-value data |
| **Data Lake Storage Gen2** | S3 + data lake capabilities | Big data / analytics |

The **four core Azure Storage services** you'll encounter most are:

> **Blob Storage + Managed Disks + Azure Files + Queue Storage**
> 

---

# 1. Azure Storage Account

Before using most Azure Storage services, you normally create a:

**Storage Account**

Think of it as a **container/account that provides access to Azure Storage services**.

AWS comparison:

> Azure Storage Account ≈ an AWS storage account/resource boundary, while Blob containers inside it are closer to S3 buckets.
> 

Example:

```
Azure
│
└── Storage Account
      │
      ├── Blob Storage
      │     ├── Container
      │     │     ├── image1.jpg
      │     │     └── video.mp4
      │
      ├── File Shares
      │     └── shared-files
      │
      ├── Queues
      │     └── order-queue
      │
      └── Tables
            └── users
```

---

# 2. Blob Storage 🪣

!image.png

!image.png

This is probably the **most important Azure Storage service** for you.

**Blob = Binary Large Object**

Used for:

- Images
- Videos
- PDFs
- Backups
- Logs
- Static website files
- Terraform state
- Application files
- Data lake storage

AWS equivalent:

> **Azure Blob Storage ≈ Amazon S3**
> 

---

## Blob hierarchy

```
Storage Account
      │
      └── Container
             │
             ├── image.jpg
             ├── backup.zip
             └── video.mp4
```

AWS:

```
S3
 │
 └── Bucket
       ├── image.jpg
       ├── backup.zip
       └── video.mp4
```

So remember:

> **S3 Bucket ≈ Azure Blob Container**
> 

---

# 3. Blob Types

Azure Blob Storage has different blob types.

### Block Blob

Most commonly used.

Used for:

```
images
videos
documents
backups
application files
```

Think:

> General-purpose objects/files.
> 

---

### Append Blob

Designed for data that is continuously appended.

Example:

```
Application logs
        ↓
Append Blob
        ↓
log1
log2
log3
log4
```

Useful for logging scenarios.

---

### Page Blob

Optimized for random read/write operations.

One important use:

> **Azure VM disks**
> 

You don't normally need to manually work with Page Blobs when using normal Azure VM disks because Azure manages the underlying implementation.

---

# 4. Blob Access Tiers

This is very important for cost optimization.

Azure provides different access tiers.

### Hot

For data accessed frequently.

Example:

```
Application images
Frequently accessed files
Website assets
```

Higher storage cost, lower access cost.

---

### Cool

For data accessed less frequently.

Example:

```
Backups
Old application data
```

Lower storage cost but higher access costs.

---

### Cold

For data accessed very rarely.

---

### Archive

!image.png

For long-term data that you almost never access.

Example:

```
Old backups
Compliance data
Historical records
```

Very cheap storage but retrieving data takes much longer.

Think:

```
Frequently used
      ↓
    HOT
      ↓
    COOL
      ↓
    COLD
      ↓
   ARCHIVE
      ↓
Rarely accessed
```

AWS comparison:

```
Azure Hot      ≈ S3 Standard
Azure Cool     ≈ S3 Infrequent Access
Azure Archive  ≈ S3 Glacier
```

---

# 5. Azure Managed Disks 💽

Now let's move from objects to **VM disks**.

AWS:

> EBS
> 

Azure:

> **Managed Disks**
> 

When you create an Azure VM, it needs a disk.

```
Azure VM
   │
   ├── OS Disk
   │
   └── Data Disk
```

AWS:

```
EC2
 │
 ├── Root EBS
 └── Data EBS
```

So:

> **Azure Managed Disk ≈ AWS EBS**
> 

---

# 6. Types of Managed Disks

You will commonly encounter:

### Standard HDD

Cheap.

Good for:

- Development
- Low-performance workloads
- Infrequently accessed data

---

### Standard SSD

Better performance than HDD.

Good general-purpose option.

---

### Premium SSD

Higher performance.

Used for:

```
Production applications
Databases
High-performance workloads
```

---

### Ultra Disk

Very high-performance workloads.

Used when you need extremely high IOPS and low latency.

---

# 7. Azure Files 📁

!image.png

Azure Files provides a **managed shared file system**.

Think:

> Multiple machines can access the same files.
> 

Example:

```
        Azure Files
             │
       ┌─────┼─────┐
       ↓     ↓     ↓
      VM1   VM2   VM3
```

All can access:

```
/shared/data
```

AWS comparison:

> **Azure Files ≈ Amazon EFS** for the common shared-file-system use case.
> 

Example:

You have three application servers:

```
VM1 ─┐
VM2 ─┼──→ Azure Files
VM3 ─┘
```

All three can access the same files.

---

# 8. Azure Files vs Blob Storage

This is a very important distinction.

### Blob

Object storage:

```
Application
     ↓
Blob Storage
     ↓
file.jpg
```

### Azure Files

File system:

```
VM1 ─┐
VM2 ─┼──→ Shared filesystem
VM3 ─┘
```

Simple rule:

> **Need objects/files accessed through APIs? → Blob**
> 

> **Need a shared filesystem mounted by machines? → Azure Files**
> 

---

# 9. Azure Queue Storage

Azure Queue Storage is used for **asynchronous communication**.

Example:

```
Web App
   │
   │ "Process this order"
   ↓
Queue
   │
   ↓
Worker
```

Instead of making the web application wait for the worker:

```
User
 ↓
Web App
 ↓
Queue
 ↓
Worker processes later
```

AWS comparison:

> **Azure Queue Storage ≈ Amazon SQS**
> 

---

# 10. Azure Table Storage

Azure Table Storage is a **NoSQL key-value/document-style storage service** for structured non-relational data.

Example:

```
PartitionKey | RowKey | Name | Age
-------------|--------|------|----
users        | 001    | Ali  | 25
users        | 002    | John | 30
```

AWS comparison:

> **Azure Table Storage ≈ DynamoDB conceptually**
> 

It's useful when you need simple NoSQL storage without relational database features.

---

# 11. Azure Data Lake Storage Gen2

This is important if you're going into:

- Big Data
- Data Engineering
- Analytics
- AI/ML
- Massive datasets

Azure Data Lake Storage Gen2 is built on **Blob Storage** and adds capabilities for analytics workloads, including hierarchical namespace.

Think:

```
Huge amounts of data
        ↓
Azure Data Lake Storage Gen2
        ↓
Databricks / Synapse / analytics
```

AWS comparison:

> **ADLS Gen2 ≈ S3 configured/used as a data lake**
> 

---

# 12. Storage Redundancy ⭐

This is a **very important Azure Storage concept**.

Azure lets you choose how many copies of your data should exist and where.

The basic idea:

```
Your data
   │
   ├── Copy 1
   ├── Copy 2
   └── Copy 3
```

Why?

Because disks/hardware/datacenters can fail.

---

## LRS — Locally Redundant Storage

Data is replicated within a single physical location/datacenter.

Conceptually:

```
Region
 │
 └── Datacenter
       ├── Copy
       ├── Copy
       └── Copy
```

Good when you mainly need protection against hardware failures within that location.

---

## ZRS — Zone Redundant Storage

Copies are distributed across availability zones within the region.

```
Azure Region
│
├── Zone 1 → Copy
├── Zone 2 → Copy
└── Zone 3 → Copy
```

Protects against a zone-level failure.

---

## GRS — Geo-Redundant Storage

Data is replicated to another Azure region.

```
Primary Region
      │
      │ replication
      ↓
Secondary Region
```

Useful for disaster recovery.

---

## GZRS — Geo-Zone-Redundant Storage

Combines zone redundancy and geo-replication.

Conceptually:

```
Primary Region
├── Zone 1
├── Zone 2
└── Zone 3
      │
      ↓
Secondary Region
```

---

# 13. Storage redundancy mental model

Remember:

```
LRS
↓
Multiple copies locally

ZRS
↓
Multiple zones

GRS
↓
Multiple regions

GZRS
↓
Multiple zones + another region
```

This is similar to thinking about AWS:

```
Single location
     ↓
Multi-AZ
     ↓
Cross-region
```

---

# 14. Azure Storage Security 🔐

Azure Storage supports several ways to secure data.

Important ones:

### Access Keys

Storage accounts have access keys.

They provide powerful access to the storage account.

Conceptually:

```
Application
    ↓
Storage Account Key
    ↓
Storage
```

But don't hard-code these in applications.

---

# 15. SAS — Shared Access Signature

SAS allows you to give **limited access** to storage.

For example:

```
User
 ↓
SAS URL
 ↓
Blob
```

You can control:

- What resource they access
- What permissions they have
- How long access is valid

Example concept:

```
User can:
READ blob

for:
30 minutes
```

Very useful.

---

# 16. Microsoft Entra ID

For modern Azure applications, you can use:

**Microsoft Entra ID**

for identity-based access.

Instead of:

```
Application
   ↓
Hard-coded storage key ❌
```

you can use:

```
Application
   ↓
Managed Identity
   ↓
Entra ID
   ↓
Azure Storage
```

This is a very important DevOps/cloud security pattern.

---

# 17. Managed Identity ⭐

This is something you should definitely learn.

Suppose an Azure VM needs to access Blob Storage.

Bad approach:

```
VM
 ↓
storage account key
```

Better:

```
VM
 ↓
Managed Identity
 ↓
RBAC
 ↓
Blob Storage
```

No password/key needs to be stored inside the VM/application.

AWS comparison:

> **Azure Managed Identity ≈ AWS IAM Role**
> 

This is one of the most useful mappings to remember.

---

# 18. Azure RBAC

You can give identities permissions to storage.

Example:

```
VM Managed Identity
        ↓
Storage Blob Data Reader
        ↓
Blob Storage
```

Or:

```
Storage Blob Data Contributor
```

depending on what the application needs.

So:

> **Azure RBAC + Managed Identity = secure access without storing credentials**
> 

---

# 19. Private Endpoint

You learned Private Link while studying Azure networking.

It becomes very important with Storage.

Normally:

```
VM
 ↓
Internet/public endpoint
 ↓
Azure Storage
```

With Private Endpoint:

```
VNet
 │
 └── Private Endpoint
          │
          ↓
     Azure Storage
```

The storage service can be accessed through a **private IP inside your VNet**.

AWS comparison:

> **Azure Private Endpoint ≈ AWS PrivateLink interface endpoint**
> 

---

# 20. Storage Account Firewall

You can restrict which networks can access your storage account.

For example:

```
Storage Account
      │
      ├── Allow VNet A
      ├── Allow specific IPs
      └── Deny everything else
```

This gives another layer of network security.

---

# 21. Encryption

Azure Storage encrypts data at rest.

Conceptually:

```
Application
     ↓
Azure Storage
     ↓
Encrypted data
```

You can also use customer-managed keys in scenarios requiring greater control over encryption keys.

---

# 22. Blob Lifecycle Management

This is **very useful for DevOps**.

Suppose you have:

```
Logs
```

Initially:

```
Hot
```

After 30 days:

```
Cool
```

After 90 days:

```
Archive
```

After 7 years:

```
Delete
```

Azure can automate this using lifecycle management rules.

Example:

```
Day 0
 ↓
HOT

Day 30
 ↓
COOL

Day 90
 ↓
ARCHIVE

Day 3650
 ↓
DELETE
```

This can significantly reduce storage costs.

---

# 23. Blob Versioning

Suppose:

```
config.json
```

You upload version 1.

Then version 2.

Then version 3.

Azure can retain previous versions.

```
config.json
 ├── v1
 ├── v2
 └── v3 ← current
```

Useful if something gets accidentally overwritten.

---

# 24. Soft Delete

Suppose you accidentally delete:

```
important-backup.zip
```

With soft delete enabled, the object can remain recoverable for a configured retention period.

Concept:

```
Delete
 ↓
Soft deleted
 ↓
Recover
```

instead of:

```
Delete
 ↓
Gone immediately
```

---

# 25. Azure Storage + VM

Let's put everything together.

Imagine:

```
                 Azure VNet
                     │
          ┌──────────┴──────────┐
          │                     │
       Web VM                App VM
          │                     │
          │                     │
          └─────────┬───────────┘
                    │
             Managed Identity
                    │
                    ↓
              Blob Storage
                    │
              application files
```

The VM's OS disk:

```
VM
 │
 └── Managed Disk
```

Shared files:

```
VM1 ─┐
VM2 ─┼──→ Azure Files
VM3 ─┘
```

Async messages:

```
Application → Queue Storage → Worker
```

Huge analytics data:

```
Applications
     ↓
ADLS Gen2
     ↓
Analytics
```

---

# 26. Azure Storage vs AWS — Cheat Sheet

This is the table I'd memorize:

| Azure | AWS | Purpose |
| --- | --- | --- |
| Storage Account | — | Storage resource boundary |
| Blob Storage | S3 | Object storage |
| Blob Container | S3 Bucket | Object container |
| Managed Disk | EBS | VM block storage |
| Azure Files | EFS | Shared file storage |
| Queue Storage | SQS | Message queue |
| Table Storage | DynamoDB | NoSQL |
| ADLS Gen2 | S3 Data Lake | Data lake |
| Managed Identity | IAM Role | Workload identity |
| SAS | Pre-signed URL | Temporary storage access |
| Private Endpoint | PrivateLink endpoint | Private access |
| Storage lifecycle | S3 Lifecycle | Automatic tiering/deletion |

---

# 🧠 The Azure Storage Mental Model

If you remember only this, you're in good shape:

```
                    Azure Storage
                         │
        ┌────────────────┼────────────────┐
        │                │                │
       Blob             Disk            Files
        │                │                │
       S3               EBS              EFS
        │
   Objects/files
        │
 ┌──────┼─────────┐
 │      │         │
Hot   Cool      Archive
```

And:

```
Queue  → messaging
Table  → NoSQL
ADLS   → big data
```

### 🔐 Security

```
Managed Identity
       ↓
      RBAC
       ↓
    Storage
```

### 🌍 Availability

```
LRS → local redundancy
ZRS → availability zones
GRS → another region
GZRS → zones + another region
```

### 💰 Cost optimization

```
Hot
 ↓
Cool
 ↓
Cold
 ↓
Archive
 ↓
Delete
```

**For your DevOps learning path, the next Azure Storage concepts I'd focus on are:** **Blob Storage hands-on → Managed Disks → Azure Files → Storage Account networking/private endpoints → RBAC + Managed Identity → lifecycle management → redundancy.**

# Entra ID

Absolutely. Since you already know AWS, I’ll teach **Microsoft Entra ID** by constantly mapping it to AWS concepts.

# 🔐 Microsoft Entra ID — Complete Guide

First, the most important thing:

> **Microsoft Entra ID is Microsoft's cloud identity and access management (IAM) service.**
> 

AWS equivalent:

> **Microsoft Entra ID ≈ AWS IAM + some AWS Organizations/identity-center functionality**
> 

But they're not exactly identical.

---

# 1. What problem does Entra ID solve?

Imagine your company has:

```
100 employees
50 applications
20 Azure resources
10 servers
```

You need to answer:

- Who is this person?
- Can they log in?
- What resources can they access?
- What applications can they use?
- Can they access Azure?
- What permissions do they have?
- Should they use MFA?
- What happens when they leave the company?

Entra ID handles the **identity side** of these problems.

```
                 Entra ID
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      Users      Groups      Applications
        │           │           │
        └───────────┼───────────┘
                    ↓
              Authentication
                    ↓
               Authorization
```

---

# 2. Entra ID vs Azure

This is a very important distinction.

**Azure** is the cloud platform.

**Entra ID** manages identities.

For example:

```
Azure
│
├── Virtual Machines
├── Storage
├── AKS
├── VNets
└── Databases
```

Entra ID:

```
Entra ID
│
├── Users
├── Groups
├── Applications
├── Service Principals
├── Managed Identities
└── Permissions
```

So:

> Azure provides resources.
> 
> 
> Entra ID controls **who can access those resources**.
> 

---

# 3. Tenant

This is probably the **most important Entra ID concept**.

When an organization uses Entra ID, it has an **Entra tenant**.

Think of a tenant as:

> **Your organization's identity boundary.**
> 

Example:

```
Company ABC
     │
     ↓
Entra Tenant
     │
 ┌───┼────┬────┐
 ↓   ↓    ↓    ↓
Users Groups Apps Devices
```

An Entra tenant has its own identity directory.

---

# 4. Tenant vs Azure Subscription

Don't confuse these.

```
Entra Tenant
      │
      ├── Users
      ├── Groups
      ├── Applications
      └── Identities
             │
             ↓
       Azure Subscription
             │
      ┌──────┼──────┐
      ↓      ↓      ↓
     VM     VNet   Storage
```

A subscription is mainly a **billing/resource management boundary**.

A tenant is an **identity boundary**.

One Entra tenant can be associated with multiple Azure subscriptions.

Example:

```
Entra Tenant
     │
     ├── Production Subscription
     ├── Development Subscription
     └── Testing Subscription
```

---

# 5. User

A user represents a person.

Example:

```
Taha
   ↓
Entra User
   ↓
taha@company.com
```

Users can authenticate and access resources according to their permissions.

AWS comparison:

> Entra User ≈ IAM user, although Entra users are generally designed around organizational identity and cloud applications.
> 

---

# 6. Groups

Instead of giving permissions to every user individually, create groups.

Example:

```
DevOps Team
│
├── Ali
├── Taha
├── Ahmed
└── John
```

Then assign permissions to the group:

```
DevOps Team
      ↓
Contributor
      ↓
Production Resource Group
```

Everyone in the group gets the appropriate access.

This is much easier to manage.

---

# 7. Roles

A role defines **what someone is allowed to do**.

Examples:

### Reader

Can view resources.

```
VM
 ↓
Can see it
❌ Can't modify it
```

### Contributor

Can manage resources but generally cannot manage access permissions.

```
VM
 ↓
Can create
Can modify
Can delete
```

### Owner

Broad control, including managing access.

```
Owner
 ↓
Manage resources
+
Manage access
```

---

# 8. Azure RBAC ⭐

This is extremely important for DevOps.

**Azure Role-Based Access Control (RBAC)** determines what identities can do to Azure resources.

The basic model is:

```
WHO
 ↓
WHAT ROLE
 ↓
ON WHICH RESOURCE
```

Example:

```
Taha
 ↓
Contributor
 ↓
Resource Group: production
```

Meaning:

> Taha can perform the actions allowed by the Contributor role on that resource group.
> 

---

# 9. RBAC Scope

This is another important concept.

Permissions can be assigned at different levels.

```
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

Example:

```
Subscription
│
├── Resource Group A
│      ├── VM
│      └── Storage
│
└── Resource Group B
       ├── VM
       └── Database
```

You could give someone:

```
Reader
  ↓
Entire Subscription
```

or:

```
Contributor
  ↓
Resource Group A
```

or:

```
Reader
  ↓
Only one VM
```

This is called **scope**.

---

# 10. Authentication vs Authorization

You absolutely need to understand this.

### Authentication

> **Who are you?**
> 

Example:

```
Username
+
Password
+
MFA
      ↓
Authentication
      ↓
"You are Taha"
```

### Authorization

> **What are you allowed to do?**
> 

```
Taha
 ↓
Contributor
 ↓
Production Resource Group
```

So:

```
Authentication
      ↓
WHO ARE YOU?

Authorization
      ↓
WHAT CAN YOU DO?
```

---

# 11. Microsoft Entra Authentication

When you log into Azure:

```
You
 ↓
Entra ID
 ↓
Authenticate
 ↓
Token
 ↓
Azure service
```

Entra ID issues security tokens that applications/services use to establish your identity and permissions.

---

# 12. MFA

**Multi-Factor Authentication**

Instead of:

```
Password
```

you can require:

```
Password
+
Authenticator app
```

or another supported authentication factor.

So even if someone steals your password:

```
Attacker
 ↓
Password ❌
 ↓
Needs second factor
```

---

# 13. Conditional Access ⭐⭐⭐

This is a very important enterprise Entra feature.

Conditional Access lets you create policies such as:

```
IF
user is outside trusted location

THEN
require MFA
```

Or:

```
IF
device isn't compliant

THEN
deny access
```

Or:

```
IF
high-risk sign-in

THEN
require stronger authentication
```

Mental model:

> **Conditional Access = IF/THEN security rules for identity access.**
> 

Example:

```
User
 ↓
Login
 ↓
Conditional Access
 ↓
┌─────────────────────┐
│ Is MFA required?    │
│ Is device trusted?  │
│ Is location allowed?│
└─────────────────────┘
 ↓
Allow / Require action / Block
```

---

# 14. Identity Protection

Entra can detect potentially risky identities and sign-ins.

For example:

```
Normal login:
Pakistan → normal device

Suspicious:
Pakistan
   ↓
5 minutes later
   ↓
Another distant location
```

Identity Protection can identify risky sign-in/user signals and integrate with Conditional Access.

Think:

> **Identity Protection = risk detection around identities/sign-ins.**
> 

---

# 15. Service Principal ⭐⭐⭐

Now we're getting into **DevOps**, and this is very important.

Suppose Terraform needs to create Azure resources.

Terraform is not a human.

So you don't want:

```
Terraform
 ↓
Taha's personal account
```

Instead, you can create an application identity/service principal.

```
Terraform
   ↓
Service Principal
   ↓
Azure
```

The service principal gets permissions through Azure RBAC.

AWS comparison:

> **Service Principal ≈ IAM role/user used by an application, depending on the scenario.**
> 

More precisely, in Azure, a service principal is an identity representing an application/service in a tenant.

---

# 16. App Registration

This is related to service principals but **not the same thing**.

When you register an application with Entra ID, you create an:

> **App Registration**
> 

It tells Entra:

> "This application exists and needs to use Entra identity."
> 

Example:

```
My Web Application
        ↓
App Registration
        ↓
Entra ID
```

An app registration defines things such as:

- Application identity
- Authentication configuration
- Redirect URIs
- API permissions
- Credentials/certificates

---

# 17. App Registration vs Service Principal

This confuses almost everyone initially.

Think:

```
App Registration
       ↓
Application definition
       ↓
Service Principal
       ↓
Identity of that application
       ↓
Used in a tenant
```

A simplified mental model:

> **App Registration = blueprint/definition of the application**
> 

> **Service Principal = application's identity in a tenant**
> 

---

# 18. Managed Identity ⭐⭐⭐

This is one of the most important Azure concepts for you as a DevOps learner.

Suppose:

```
Azure VM
   ↓
Blob Storage
```

How does the VM authenticate?

Bad:

```
VM
 ↓
Storage password/key stored in code ❌
```

Better:

```
VM
 ↓
Managed Identity
 ↓
Entra ID
 ↓
RBAC
 ↓
Blob Storage
```

No secret needs to be manually stored in the application.

AWS comparison:

> **Managed Identity ≈ IAM Role attached to an AWS resource**
> 

---

# 19. Two types of Managed Identity

There are two main types.

### System-assigned

Identity is tied to the Azure resource.

```
VM
 ↓
System-assigned identity
```

If the VM is deleted, its identity is also deleted.

---

### User-assigned

Identity exists independently.

```
Managed Identity
       │
 ┌─────┼─────┐
 ↓     ↓     ↓
VM1   VM2   VM3
```

You can attach the same identity to multiple resources.

This is useful when several resources need the same permissions.

---

# 20. Managed Identity + RBAC Example

Suppose your application needs to read blobs.

You could do:

```
Application VM
      │
      ↓
Managed Identity
      │
      ↓
Storage Blob Data Reader
      │
      ↓
Blob Storage
```

The important part is:

```
Identity
   +
Role
   +
Scope
```

This is the Azure authorization model you should remember.

---

# 21. External / Guest Users

Entra can also work with users outside your organization.

For example:

```
Company A
   ↓
Entra Tenant
   ↓
Guest User
   ↑
Company B user
```

This is commonly associated with **Microsoft Entra B2B collaboration**.

Useful when partners, contractors, or external users need controlled access.

---

# 22. Entra ID vs Active Directory

This is another VERY common interview question.

### Active Directory Domain Services

Traditional on-premises identity:

```
Company Data Center
       │
       ↓
Active Directory
       │
 ┌─────┼─────┐
 ↓     ↓     ↓
PCs   Servers Users
```

### Microsoft Entra ID

Cloud identity:

```
Cloud
  ↓
Entra ID
  ↓
Azure
SaaS Apps
Cloud Apps
```

Entra ID is **not simply "Active Directory in the cloud."**

They are different identity platforms designed for different environments and protocols/use cases.

---

# 23. Microsoft Entra Domain Services

There is also:

**Microsoft Entra Domain Services**

This provides managed domain services for workloads that require traditional domain capabilities such as:

- Domain join
- LDAP
- Kerberos
- NTLM

Mental model:

```
Entra ID
   │
   └── Cloud identity

Entra Domain Services
   │
   └── Managed traditional domain capabilities
```

---

# 24. Single Sign-On — SSO

Suppose your company uses:

```
Microsoft 365
Azure
GitHub
Salesforce
Jira
```

Without SSO:

```
Login
Login
Login
Login
Login
```

With SSO:

```
             Entra ID
                 │
       ┌─────────┼─────────┐
       ↓         ↓         ↓
    Azure     GitHub     Salesforce
```

User authenticates through the identity provider and can access configured applications without repeatedly entering credentials.

---

# 25. OAuth 2.0

You will encounter OAuth when working with applications/APIs.

Simplified:

```
Application
    ↓
Entra ID
    ↓
Access Token
    ↓
API
```

The token represents delegated/application access according to the configured permissions.

You don't need to memorize the protocol details yet, but understand:

> **OAuth is primarily about delegated authorization/access to resources.**
> 

---

# 26. OpenID Connect

OIDC is built on OAuth 2.0 and is commonly used for **authentication**.

Simple distinction:

```
OAuth
 ↓
Authorization

OpenID Connect
 ↓
Authentication / identity
```

You'll encounter OIDC heavily when integrating applications with Entra ID.

---

# 27. SAML

SAML is another enterprise authentication/federation technology.

You may see:

```
Enterprise Application
       ↓
SAML
       ↓
Entra ID
```

It's commonly used for SSO with enterprise applications.

You don't need to master SAML before understanding basic Entra ID.

---

# 28. Enterprise Applications

An organization might use:

```
Slack
Salesforce
GitHub
ServiceNow
Custom internal application
```

These can be integrated with Entra ID.

Then administrators can control:

- Who can access the application
- SSO
- Provisioning
- Conditional Access
- Authentication

---

# 29. Identity Governance

As organizations become larger, they need to control the **lifecycle of access**.

For example:

```
Employee joins
      ↓
Gets appropriate access
      ↓
Changes department
      ↓
Access changes
      ↓
Leaves company
      ↓
Access removed
```

Entra has identity-governance capabilities for things such as:

- Access reviews
- Entitlement management
- Lifecycle workflows
- Privileged access management

---

# 30. Privileged Identity Management — PIM ⭐

Imagine an administrator has permanent:

```
Owner
```

permissions.

That's risky.

Instead:

```
Normal User
     ↓
Request elevated access
     ↓
Approval / policy
     ↓
Temporary privileged role
     ↓
Access expires
```

That's the basic idea behind **Privileged Identity Management (PIM)**.

Think:

> **Don't give admins powerful permissions permanently when temporary elevation is enough.**
> 

---

# 31. Microsoft Entra ID Protection vs PIM vs Conditional Access

Easy distinction:

| Feature | Main purpose |
| --- | --- |
| **Conditional Access** | Decide whether/how access is allowed |
| **Identity Protection** | Detect identity/sign-in risks |
| **PIM** | Control temporary privileged access |

Example:

```
User logs in
     ↓
Identity Protection
     ↓
Is this risky?
     ↓
Conditional Access
     ↓
Should MFA be required?
     ↓
PIM
     ↓
Does user need temporary admin privileges?
```

---

# 32. Entra ID for DevOps ⭐⭐⭐

This is where it becomes very useful for you.

Imagine you have:

```
GitHub Actions
       ↓
Azure
       ↓
AKS
       ↓
Azure Container Registry
       ↓
Azure Storage
```

You need authentication.

You don't want:

```
GitHub Actions
     ↓
Permanent Azure password ❌
```

Instead, modern setups can use **workload identity federation / OIDC**.

Conceptually:

```
GitHub Actions
      ↓
OIDC token
      ↓
Entra ID
      ↓
Federated identity
      ↓
Azure permissions
      ↓
Deploy
```

This avoids storing long-lived Azure client secrets in GitHub.

This is a **very important DevOps pattern**.

---

# 33. Azure CLI + Entra ID

When you run:

```
az login
```

you're authenticating with Microsoft Entra ID.

Then:

```
az account show
```

shows information about your current Azure account/subscription context.

You can then execute Azure operations according to your permissions.

---

# 34. Terraform + Entra ID

Terraform also needs an identity.

Conceptually:

```
Terraform
    ↓
Azure Provider
    ↓
Entra authentication
    ↓
Identity
    ↓
Azure RBAC
    ↓
Azure resources
```

The identity could be represented through supported service-principal/workload-identity approaches.

The key DevOps principle is:

> **Terraform should use a dedicated workload identity with only the permissions it needs.**
> 

Not your personal account.

---

# 35. Entra ID + AKS

You will eventually see this architecture:

```
Developer
    ↓
Entra ID
    ↓
Authentication
    ↓
AKS
    ↓
Kubernetes RBAC
```

There are actually **two authorization layers** you need to understand:

```
Entra ID
   ↓
Azure authorization / identity

Kubernetes RBAC
   ↓
Permissions inside Kubernetes
```

For example:

```
User
 ↓
Entra ID
 ↓
AKS authentication
 ↓
Kubernetes RBAC
 ↓
Can access pods?
Can access deployments?
Can view namespaces?
```

This becomes very important when you work with production AKS.

---

# 36. Entra ID vs Azure RBAC

Don't confuse them.

### Entra ID

Answers:

> **Who are you?**
> 

```
User
Service Principal
Managed Identity
Application
```

### Azure RBAC

Answers:

> **What can that identity do to Azure resources?**
> 

```
Reader
Contributor
Owner
Storage Blob Data Reader
...
```

Together:

```
Entra Identity
      ↓
Azure RBAC Role
      ↓
Azure Resource
```

---

# 37. The Complete Mental Model

This is the diagram I want you to remember:

```
                    ENTRA ID
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      Users          Groups       Applications
        │                             │
        │                       Service Principal
        │
        └──────────────┬──────────────┘
                       ↓
                Authentication
                       │
                  Access Token
                       ↓
              ┌────────────────┐
              │ Conditional     │
              │ Access / MFA    │
              └───────┬────────┘
                      ↓
                 Azure RBAC
                      │
            ┌─────────┼─────────┐
            ↓         ↓         ↓
           VM       Storage     AKS
```

And for Azure workloads:

```
Azure VM / Function / App Service
              ↓
       Managed Identity
              ↓
          Entra ID
              ↓
          Azure RBAC
              ↓
      Azure Storage / Key Vault
```

---

# 🧠 Entra ID Cheat Sheet

Memorize these:

| Concept | Simple meaning |
| --- | --- |
| **Tenant** | Organization's identity boundary |
| **User** | Human identity |
| **Group** | Collection of users |
| **Authentication** | Who are you? |
| **Authorization** | What can you do? |
| **MFA** | Extra authentication factor |
| **Conditional Access** | IF/THEN access policies |
| **RBAC** | Resource permissions |
| **Role** | Set of permissions |
| **Scope** | Where permissions apply |
| **App Registration** | Application definition |
| **Service Principal** | Application identity in a tenant |
| **Managed Identity** | Azure-managed workload identity |
| **PIM** | Temporary/controlled privileged access |
| **Identity Protection** | Detect identity/sign-in risk |
| **SSO** | One identity for multiple apps |
| **OAuth** | Authorization |
| **OIDC** | Authentication/identity on OAuth |
| **SAML** | Enterprise SSO/federation |
| **B2B** | External/guest collaboration |
| **Entra Domain Services** | Managed traditional domain capabilities |

## ⭐ The 5 things I'd prioritize for your DevOps path

Since you're learning Azure for **DevOps/cloud**, focus especially on:

```
1. Entra Tenant
       ↓
2. Users / Groups
       ↓
3. Azure RBAC
       ↓
4. Managed Identity
       ↓
5. Service Principal + OIDC
```

If you understand those five deeply, you'll understand a huge portion of how **Terraform, GitHub Actions, Azure Storage, Key Vault, AKS, VMs, and Azure APIs authenticate and authorize**.

## Azure RBAC

!image.png

!image.png

!image.png

## Azure Zero Trust Model:

!image.png

## Defense In depth

!image.png

## Microsoft Defender

!image.png

!image.png

## Azure Cost Optimization

!image.png

!image.png

!image.png

# Azure Governance and Compliance

> Also use service trust portal
> 

!image.png

# Azure Policy & Resource Locks

!image.png

!image.png

## Azure Advisor

!image.png

!image.png

!image.png
