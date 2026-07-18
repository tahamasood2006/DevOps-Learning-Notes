# What is Internet & How it Works :

When we connect 2 or more computers with each other they form a network, on what size of area these computers are connected this is categorized as LAN, PAN, MAN, WAN.  IF a computer is not connected with any device means making no network this type of device/computer is known as standalone device.

WAN Cables are laid down through sea routes. So till now we know that are country is getting internet access through these submarine cable. Now, how we access internet from our home? These cables get connected to an IXP which then connects to ISP. 

## Image Explanation of How internet comes to our Home

!Screenshot from 2026-07-09 12-21-57.png

## FlowChart type explanation of how internet comes to our Home

!Screenshot from 2026-07-09 12-25-34.png

## ISP vs IXP

An **ISP** (Internet Service Provider) provides individuals and businesses with access to the internet. An **IXP** (Internet Exchange Point) is a physical infrastructure where multiple ISPs, CDNs, and networks meet to exchange traffic directly with one another.

An Internet exchange point (IXP) is a physical location through which Internet infrastructure companies such as Internet Service Providers (ISPs) and CDNs connect with each other

## Bandwidth

Data is transferred in the form of packets, so when we transfer data how much data we can transfer per second is our bandwidth Eg: 20 mbps means we can transfer 20 mega bits of data per second. The larger it is the better internet we get.

## Latency

When we transfer data from our source computer to destination device, the duration data takes to travel across a network from its source to its destination is known as latency. the much lesser it is the better it is. This also plays a vital role in our internet accessing speed.

## Jitter

The amount of variation in our latency is jitter. While *latency* measures the total time delay, jitter measures how inconsistent that delay is.

# Physical Internet:

## Router

Router is used in our homes and offices to route our data to its desired destination address. It takes our data in the form of packets and routes/sends it towards the destination address. 

## Switch

A switch is used in large size enterprises and data centers, when we need to connect our devices/servers/computers with each other inside a data center or large enterprise we use switch. BUT we can use router for same purposes then why switch. So basically switch can connect a large number of devices together with great speed.   

## Hub

Hub is something we used in old days as switch and router. 

# Geographical Locations:

## Edge Location

From taking data from our home to destination, Imagine from Karachi I am sending something to New York the data wouldn’t directly gets send,  it will first go to some place then some place and then finally to our desired address in New York, So All these in between places where it goes is known as edges. All these locations will be edge locations. We cache our data from CDNs and stores it as cache with is stored inside these edge locations (small shops/ small warehouse or super small data center).

## CDN

When we store our data in cloud, our static data and images our stored in places called CDNs.

When a user requests content from a website, a traditional setup requires their request to travel all the way to the website’s primary server (the origin server). A CDN solves this by placing "edge servers" in numerous data centers worldwide, known as Points of Presence (PoPs). When content is requested, the CDN automatically routes the user to the edge server closest to them, dramatically cutting down data travel time

## Availability Zones

An **Availability Zone (AZ)** is **a fully isolated, physical data center location within a cloud provider’s geographic region**. Each AZ features independent power, cooling, and network connectivity. They are separated by meaningful distances to prevent localized disasters from taking out an entire region, yet are close enough to provide single-digit millisecond latency

## Availability Zone vs Edge Location

| **Feature** | **Availability Zone (AZ)** | **Edge Location** |
| --- | --- | --- |
| **Primary Purpose** | Fault tolerance, redundancy, and hosting backend infrastructure. | Content delivery, caching, and accelerating user access. |
| **Physical Setup** | One or more discrete data centers with independent power, cooling, and network links. | Smaller data centers/server hubs placed globally near major population centers. |
| **Count per Region** | Multiple AZs per AWS Region (typically 2-6). | Hundreds of Edge Locations globally. |
| **Typical Services** | Amazon EC2, Amazon RDS, VPCs. | Amazon CloudFront (CDN), AWS WAF, Route 53. |

# Internet Addressing:

## IP Address

An IP Address (Internet Protocol Address) is a unique numerical label assigned to each device connected to a computer network. Every IP address has two parts. The first part indicates which network the address belongs to. The second part specifies the device within that network. However, the length of the "first part" changes depending on the network's class

## Mac Address

Physical Identifier permanently assigned to a device through Network Interface Card(NIC) by its manufacturer 

## IPv4 & Octenet

IPv4 (Internet Protocol version 4) is the **foundational networking protocol used to identify devices on a network and route most of today's internet traffic**. It utilizes 32-bit addresses, which allows for approximately 4.29 billion unique addresses, typically written in human-readable dotted decimal notation (e.g., `192.168.1.1`). Because the internet has grown exponentially, the available pool of unique IPv4 addresses is largely exhausted.

## IPv6

The next generation Internet Protocol (IP) address standard, known as IPv6, is meant to work in cooperation with **IPv4**. To communicate with other devices, a computer, smartphone, home automation component, Internet of Things sensor, or any other Internet-connected device needs a numerical IP address.

## Sub-net

The way IP addresses are constructed makes it relatively simple for Internet routers to find the right network to route data into. However, in a Class A network (for instance), there could be millions of connected devices, and it could take some time for the data to find the right device. This is why subnetting comes in handy: sub-netting narrows down the IP address to usage within a range of devices.

Because an IP address is limited to indicating the network and the device address, IP addresses cannot be used to indicate which sub-net an IP packet should go to. Routers within a network use something called a sub-net mask to sort data into subnetworks.

## Sub-net Mask

Routers within a network use something called a subnet mask to sort data into subnetworks.

Subnet mask is used to send the data packet to its desired subnet. 

## Class-based Addressing

Networks are categorized into different classes, labeled A through E. Class A networks can connect millions of devices. Class B networks and Class C networks are progressively smaller in size. (Class D and Class E networks are not commonly used.)

Let's break down how these classes affect IP address construction:

**Class A network:** Everything before the first period indicates the network, and everything after it specifies the device within that network. Using 203.0.113.112 as an example, the network is indicated by "203" and the device by "0.113.112."

**Class B network:** Everything before the second period indicates the network. Again using 203.0.113.112 as an example, "203.0" indicates the network and "113.112" indicates the device within that network.

**Class C network:** For Class C networks, everything before the third period indicates the network. Using the same example, "203.0.113" indicates the Class C network, and "112" indicates the device.

## CIDR (imp)

CIDR is used to provide ip addresses in a subnet.

CIDR(Classless Inter Domain Routing) is a method of IP address allocation and routing that allows more efficient use of IP addresses Uses 2^32-n. The first and last IP addresses are not used, they are reserved known as network and broadcast address. 

So  (2^32-n) - 2 are usable number of IP addresses 

https://www.geeksforgeeks.org/computer-networks/classless-inter-domain-routing-cidr/

!image.png

### Network Address

The first IP address CIDR will give is known as network address

### Broadcast Address

The last IP address CIDR will give is known as network address

## WebServer vs Application Server

web-server is used for rendering static pages like we use nginx, it is a webserver while the application server is like our Django or node js server which runs for dynamic data.

## DNS Server & its process (imp for interview)

A DNS (Domain Name System) server **functions like the internet’s phonebook**. It automatically translates human-readable domain names (like `google.com`) into numerical IP addresses (like `192.0.2.1`), allowing your browser to locate and load websites.

```jsx
1. You type:
   www.google.com

           │
           ▼

2. Browser checks:
   - Browser cache
   - Operating system cache
   - Local hosts file

   Found?
   ├── Yes → Use the IP address.
   └── No → Ask the DNS Resolver.

           │
           ▼

3. DNS Resolver (ISP or Cloudflare/Google DNS)

   Checks its own cache.

   Found?
   ├── Yes → Return IP.
   └── No → Ask Root DNS.

           │
           ▼

4. Root DNS Server

   Doesn't know the IP.
   It says:
   "Ask the .com nameserver."

           │
           ▼

5. .com TLD Nameserver

   Says:
   "The authoritative nameserver for google.com is..."

           │
           ▼

6. Authoritative Nameserver

   Returns:
   google.com → 142.250.xxx.xxx

           │
           ▼

7. Resolver

   Caches the answer and sends the IP back to your computer.

           │
           ▼

8. Browser

   Connects to:
   142.250.xxx.xxx

   (TCP/TLS handshake)

           │
           ▼

9. Web Server

   Returns the website.
```

## TLD Server

In the DNS hierarchy, a top-level domain (TLD) represents the first stop after the root zone. In simpler terms, a TLD is everything that follows the final dot of a domain name. For example, in the domain name ‘google.com’, ‘.com’ is the TLD. Some other popular TLDs include ‘.org’, ‘.uk’, and ‘.edu’.

TLDs play an important role in the DNS lookup process. For all uncached requests, when a user enters a domain name like ‘google.com’ into their browser window, the DNS resolvers start the search by communicating with the TLD server. In this case, the TLD is ‘.com’, so the resolver will contact the TLD DNS server, which will then provide the resolver with the IP address of Google’s origin server.

# OSI MODEL:

OSI Model is a representation of how data moves Example: if I am sending a msg on insta how it will reach …? The data will move in 7 layers

The **OSI (Open Systems Interconnection) model** is a conceptual framework with **7 layers**. Each layer has a specific job and communicates only with the layers directly above and below it.

```
+----------------------+
| 7. Application       |
+----------------------+
| 6. Presentation      |
+----------------------+
| 5. Session           |
+----------------------+
| 4. Transport         |
+----------------------+
| 3. Network           |
+----------------------+
| 2. Data Link         |
+----------------------+
| 1. Physical          |
+----------------------+
```

---

# Layer 7 — Application

**Purpose:** Provides network services to applications.

This is where users interact with networked applications.

Examples:

- Web browsers
- Email clients
- SSH clients
- FTP clients

Protocols:

- HTTP
- HTTPS
- DNS
- SMTP
- IMAP
- POP3
- FTP
- SSH

---

# Layer 6 — Presentation

**Purpose:** Makes data readable between systems.

Responsibilities:

- Encryption
- Decryption
- Compression
- Decompression
- Character encoding
- Data formatting

Examples:

- TLS/SSL encryption
- JPEG
- PNG
- UTF-8
- JSON
- XML

---

# Layer 5 — Session

**Purpose:** Starts, maintains, and ends communication sessions.

Responsibilities:

- Session creation
- Authentication
- Session recovery
- Session termination

Example:

```
SSH session

Client ---------- Server

Login

Maintain session

Logout
```

Examples:

- SSH
- NetBIOS
- RPC

Modern TCP/IP often combines this layer with the application layer.

---

# Layer 4 — Transport

**Purpose:** End-to-end communication between applications.

Responsibilities:

- Reliability
- Flow control
- Error recovery
- Port numbers
- Segmentation
- Reassembly

Protocols:

- TCP
- UDP

### TCP

Reliable.

Features:

- Three-way handshake
- Retransmission
- Acknowledgements
- Ordered delivery

Example:

```
Packet 1
Packet 2
Packet 3

If Packet 2 is lost:

Receiver:
"I only received Packet 1."

Sender retransmits Packet 2.
```

### UDP

Fast.

No guarantees.

Examples:

- Gaming
- Voice calls
- Video streaming
- DNS queries

Ports:

```
80 HTTP
443 HTTPS
22 SSH
53 DNS
25 SMTP
```

Data Unit:

```
Segment (TCP)
Datagram (UDP)
```

---

# Layer 3 — Network

**Purpose:** Moves packets between different networks.

Main job:

```
Routing
```

Device:

```
Router
```

Protocol:

```
IP
```

Responsibilities:

- Logical addressing
- Routing
- Best path selection
- Packet forwarding

Address:

```
192.168.1.10

8.8.8.8
```

Example:

```
Pakistan

↓

ISP

↓

Internet

↓

Google
```

Each router examines the destination IP and forwards the packet toward its destination.

Data Unit:

```
Packet
```

---

# Layer 2 — Data Link

**Purpose:** Communication within the same local network (LAN).

Device:

```
Switch
```

Address:

```
MAC Address

AA:BB:CC:DD:EE:FF
```

Responsibilities:

- Framing
- Error detection (CRC)
- MAC addressing
- Switching

Protocols:

- Ethernet
- Wi-Fi (802.11)

Example:

```
Laptop

↓

Switch

↓

Printer
```

The switch forwards frames based on the destination MAC address.

Data Unit:

```
Frame
```

---

# Layer 1 — Physical

**Purpose:** Sends raw bits over a physical medium.

Examples:

- Ethernet cables
- Fiber optic cables
- Radio waves (Wi-Fi)
- Electrical signals
- Optical signals

Responsibilities:

- Voltage levels
- Timing
- Connectors
- Cable specifications

Example:

```
101010110101010101
```

Data Unit:

```
Bits
```

Devices:

- Cables
- Hubs
- Repeaters
- Network interface hardware (physical signaling)

---

# Encapsulation (Sending)

As data travels **down** the OSI stack, each layer adds its own header.

```
Application
      │
      ▼
HTTP Request

Presentation
      │
      ▼
TLS Encryption

Session
      │
      ▼
Session Info

Transport
      │
      ▼
TCP Header
(Source Port, Destination Port)

Network
      │
      ▼
IP Header
(Source IP, Destination IP)

Data Link
      │
      ▼
Ethernet Header
(Source MAC, Destination MAC)

Physical
      │
      ▼
101010101010...
```

---

# Decapsulation (Receiving)

The receiving device removes the headers in reverse order.

```
Bits
↓

Frame
↓

Packet
↓

Segment
↓

Data
↓

Application
```

---

# Example: Opening `https://google.com`

1. **Application:** Browser creates an HTTPS request.
2. **Presentation:** TLS encrypts the request.
3. **Session:** Manages the secure session.
4. **Transport:** TCP adds ports (e.g., source `49152`, destination `443`) and ensures reliable delivery.
5. **Network:** IP adds source and destination IP addresses and routers forward the packet.
6. **Data Link:** Ethernet/Wi-Fi adds MAC addresses so the frame can reach the next device on the local network.
7. **Physical:** Bits are transmitted over cable, fiber, or Wi-Fi.

The server performs the reverse process and sends the response back.

---

## OSI Summary Table

| Layer | Name | Main Job | Protocols/Examples | Data Unit | Devices |
| --- | --- | --- | --- | --- | --- |
| 7 | Application | User-facing network services | HTTP, HTTPS, DNS, SMTP, SSH | Data | Browser, Email client |
| 6 | Presentation | Encryption, compression, formatting | TLS, JPEG, UTF-8, JSON | Data | — |
| 5 | Session | Start/manage/end sessions | RPC, NetBIOS, SSH session | Data | — |
| 4 | Transport | Reliable delivery, ports | TCP, UDP | Segment/Datagram | Firewall, Load balancer |
| 3 | Network | Routing, IP addressing | IP, ICMP | Packet | Router |
| 2 | Data Link | Local delivery, MAC addressing | Ethernet, Wi-Fi | Frame | Switch |
| 1 | Physical | Send electrical/optical/radio signals | Ethernet cable, Fiber, Wi-Fi radio | Bits | Cable, Hub, Repeater |

### Mnemonics

Top → Bottom:

> **All People Seem To Need Data Processing**
> 

Bottom → Top:

> **Please Do Not Throw Sausage Pizza Away**
> 

These help you remember the order of the seven layers.

## TCP

A protocol through which we use to send data, if use this it is guranteed that our data will reach its destination. Used By transport layer of OSI. Used for accesing static things mostly like a file on server or a pic on server , static things. 

***IMPORTANT FOR INTERVIEW:***

“IMP SSH always uses TCP && https,http & SMTP too”

“The reason data will 100% gets transferred here is because of 3 Way Handshake in TCP “

The **TCP 3-way handshake** is the process used to establish a reliable connection before data is sent.

```
Client                          Server
   |                                |
1. | -------- SYN ----------------> |
   |                                |
2. | <----- SYN + ACK ------------- |
   |                                |
3. | -------- ACK ----------------> |
   |                                |
   |===== Connection Established ===|
```

### Step 1: SYN (Synchronize)

The **client** says:

> "I want to connect."
> 

### Step 2: SYN-ACK (Synchronize + Acknowledge)

The **server** replies:

> "I received your request, and I'm ready."
> 

### Step 3: ACK (Acknowledge)

The **client** responds:

> "Great, let's start communicating."
> 

The TCP connection is now established, and both sides can exchange data.

### Real-life analogy

```
You:     "Can we talk?"          (SYN)
Friend:  "Yes, I hear you."      (SYN-ACK)
You:     "Awesome, let's talk."  (ACK)
```

After these 3 steps, data (such as an HTTP or HTTPS request) can be sent.

## UDP

**User Datagram Protocol (UDP)** A protocol through which we use to send data, if use this it is not guranteed that our data will reach its destination. Used By transport layer of OSI. Used for accesing dynamic data mostly a video stream or voice call / video call. NO 3 way handshake

## TCP IP MODEL

TCP IP Model has 4 layers

The **TCP/IP model** is the practical networking model used on the Internet. Unlike the 7-layer OSI model, it has **4 layers**.

```
+----------------------+
| 4. Application       |
+----------------------+
| 3. Transport         |
+----------------------+
| 2. Internet          |
+----------------------+
| 1. Network Access    |
+----------------------+

1. The Application,Presentatio and session layers are inside the Application her
2. The Transport layer has only Transport 
3. Internet layer has ony Network layer
4. The Data Link and Physical layers are in the Network Access layer

```

## HTTP vs HTTPS

HTTP and HTTPS are **application-layer protocols** used to transfer web data between a browser and a web server.HTTP defines **how these requests and responses are formatted** so every browser and web server can understand each other.

| HTTP | HTTPS |
| --- | --- |
| **HyperText Transfer Protocol** | **HyperText Transfer Protocol Secure** |
| Data is **not encrypted** | Data is **encrypted** using TLS/SSL |
| Less secure | More secure |
| Default port **80** | Default port **443** |
| URL starts with `http://` | URL starts with `https://` |

## TLS/SSL Certificate

A **TLS certificate** (often still called an **SSL certificate**) is a **digital identity card** for a website.

It proves:

- ✅ The website is authentic.
- 🔒 The website can establish an encrypted connection using TLS.

---

## Why do we use it?

Without a certificate:

```
You → "Is this really google.com?"

Server → "Trust me."

You → ❌ No proof.
```

With a certificate:

```
You → "Is this really google.com?"

Server → "Here's my certificate."

Certificate Authority (CA)
        ↓
"Yes, I verified this website."

You → ✅ Trust the server.
```

This prevents attackers from pretending to be legitimate websites.

---

# What's inside a certificate?

A TLS certificate contains information like:

```
Domain Name:
google.com

Owner:
Google LLC

Public Key:
ABCD12345...

Issued By:
Certificate Authority (CA)

Expiration Date:
2027-05-01
```

| Term | Purpose |
| --- | --- |
| **TLS** | Encrypts communication and protects integrity. |
| **SSL** | Older protocol; modern websites use TLS. |
| **TLS/SSL Certificate** | Proves the server's identity and contains its public key. |
| **Certificate Authority (CA)** | Trusted organization that issues and signs certificates. |
| **Public Key** | Shared with clients to help establish a secure connection. |
| **Private Key** | Secret key on the server used to prove ownership and complete the handshake. |
| **Session Key** | Temporary symmetric key used to encrypt all data after the handshake. |

## Web Server & Nginix

A **web server** is software that receives HTTP/HTTPS requests from clients (browsers or apps) and sends back responses (HTML, images, JSON, files, etc.).

**Nginx** (pronounced **"Engine-X"**) is one of the most popular web server software.

It can:

- Serve websites
- Reverse proxy requests
- Load balance traffic
- Handle HTTPS (TLS termination)
- Serve static files (HTML, CSS, JS, images)

---

## Example without Nginx

```
Browser
    │
    ▼
Node.js App
```

The browser talks directly to your application.

---

## Example with Nginx

```
Browser
    │
HTTPS
    ▼
Nginx
    │
HTTP
    ▼
Node.js App

```

If you **don't use Nginx**, the client connects **directly to your application**.

### Without Nginx

```
Browser
    │
HTTPS/HTTP
    ▼
Node.js App (Port 3000)
```

Your application must:

- Handle HTTP/HTTPS
- Manage TLS certificates
- Serve static files
- Process application logic

## PORT & some common numbers of Port Services(IMP)

Ports are pathways thrugh which we can send data from one IP/computer to another. 

there are 1-65535 number of ports in total, every ports runs a different service on it. That port must be open through which we are sending data, communicating. A **port** is a logical communication endpoint on a computer. It tells the operating system **which application or service should receive incoming network traffic**.

## Why do we need ports?

A single computer can run many network services at once.

Example:

```
Your PC
│
├── Web Server      → Port 80
├── HTTPS Server    → Port 443
├── SSH Server      → Port 22
├── Database        → Port 5432
└── Minecraft       → Port 25565
```

The port tells the OS which program should receive the data.

---

## Most Common Ports (IMP)

| Port | Protocol | Used For |
| --- | --- | --- |
| **20, 21** | FTP | File Transfer |
| **22** | SSH | Secure remote login |
| **23** | Telnet | Remote login (insecure) |
| **25** | SMTP | Sending email |
| **53** | DNS | Domain name lookup |
| **67, 68** | DHCP | Automatic IP assignment |
| **80** | HTTP | Websites |
| **110** | POP3 | Receiving email |
| **143** | IMAP | Receiving email |
| **123** | NTP | Time synchronization |
| **161** | SNMP | Network device monitoring |
| **389** | LDAP | Directory services |
| **443** | HTTPS | Secure websites |
| **3306** | MySQL | MySQL database |
| **5432** | PostgreSQL | PostgreSQL database |
| **6379** | Redis | Redis cache |
| **27017** | MongoDB | MongoDB database |
| **8080** | HTTP (alternate) | Development web servers |
| **3000** | Common dev port | Node.js/React apps |
| **5000** | Common dev port | Flask apps |
| **8000** | Common dev port | Django/Python apps |

---

## Example

You visit:

```
https://google.com
```

Your browser connects to:

```
Destination IP: 142.250.xxx.xxx
Destination Port: 443
```

The operating system on Google's server delivers the request to the web server listening on **port 443**.

---

## Port ranges

| Range | Description |
| --- | --- |
| **0–1023** | Well-known ports (HTTP, HTTPS, SSH, DNS, etc.) |
| **1024–49151** | Registered ports (applications/services) |
| **49152–65535** | Dynamic/Ephemeral ports (temporary client ports) |

## Internet Gateway (IGW)

An **Internet Gateway** connects your **VPC (Virtual Private Cloud)** to the **Internet**.

Without an IGW, resources in your VPC **cannot communicate with the Internet**.

## NAT Gateway

Use a NAT Gateway when private servers need outbound Internet access (updates, APIs, Docker images, etc.) without exposing them to inbound connections from the Internet.A **NAT (Network Address Translation) Gateway** works by **translating the private IP address of your instance into its own public IP** when traffic goes to the Internet.A **NAT (Network Address Translation) Gateway** lets **private instances access the Internet** while **preventing the Internet from initiating connections back to them**.

# Internet Security:

## Firewall

## Ingress & Egress

Ingress means incoming data and egress means outgoing data 

# System Design:

## Micro-services vs Monolith vs Monorepo

Monolith means A **single application** where everything is in one codebase and deployed together.

Micro-services means The application is split into **multiple independent services**.

Mono-repo means A **repository structure**, not an architecture. It means **multiple projects live in one Git repository**.

## JWT Token Based Authentication vs OAuth :

A **JWT** is a token that proves a user is authenticated.

Flow:

```
Login
   │
   ▼
Server verifies credentials
   │
   ▼
Server creates JWT
   │
   ▼
Client stores JWT
   │
   ▼
Client sends JWT with every request
```

Example:

```
Authorization: Bearer <JWT>
```

**Purpose:** Keep the user logged in without sending their password every time.

---

**OAuth** is an authorization protocol that lets users grant an app limited access to another service **without sharing their password**.

Example:

```
Login with Google
```

Flow:

```
You
 │
 ▼
App
 │
 ▼
Google
 │
User logs in
 │
 ▼
Google returns Access Token
 │
 ▼
App can access allowed Google data
```

---

## Difference

| JWT | OAuth |
| --- | --- |
| A **token format** | An **authorization framework** |
| Stores user identity/claims | Grants permission to access resources |
| Used for authentication | Used for delegated authorization |

### Easy way to remember

- **JWT** = **"Who are you?"** (identity/authentication)
- **OAuth** = **"What are you allowed to access?"** (authorization)

> **Note:** OAuth often uses **JWTs** as access or ID tokens, but OAuth itself is not JWT.
> 

## gRPC

**In Web2, RPC lets one application call functions on another server over the network, while REST exposes resources through URLs.** Both achieve communication between clients and servers, just with different API styles.

## Elastic Load Balancer

## Horizontal Scaling & Vertical Scaling

### Horizontal Scaling

**Add more servers.**

```
Before:
App

After:
App1
App2
App3
```

A load balancer distributes traffic across them.

**Example:** 1 server → 5 servers.

---

### Vertical Scaling (Scale Up)

**Make the existing server more powerful.**

```
Before:
2 CPU
4 GB RAM

↓

After:
8 CPU
32 GB RAM
```

No extra servers—just upgrade the current one.

### Easy way to remember

- **Horizontal** = ➡️ **More servers**
- **Vertical** = ⬆️ **More power** (CPU/RAM) on the same server

## Horizontal Pod Scaling (HPA) – Kubernetes

**Horizontal Pod Autoscaler (HPA)** automatically **adds or removes Pods** based on load (CPU, memory, or custom metrics).

## Vertical Pod Scaling (VPA) – Kubernetes

**Vertical Pod Autoscaler (VPA)** automatically **increases or decreases the CPU and memory allocated to a Pod**.

# DevOps:

## What is DevOps?

Culture which improves the delivery of Applications. The job is to decrease the time to market of the application. DevOps is the process of improving application delivery.

## Introduce Yourself (Imp for interview)

## SDLC

A standard process used by software industry for design, develop and test applications/softwares.

```jsx
Plan --> Define --> Design --> Building --> Testing --> Deployment
```
