# Day 11 - AWS VPC

When you start learning AWS, services like EC2, S3, Lambda, and RDS feel exciting.

But behind almost every real AWS architecture, there is one silent foundation:

**Amazon VPC — Virtual Private Cloud**

If EC2 is your server, RDS is your database, and ALB is your traffic manager, then **VPC is the private network where all these resources live**.

In this blog, we will understand:

- What is VPC
- CIDR and subnetting basics
- Public and private subnets
- Route tables
- Internet Gateway
- Security Groups
- VPC Peering
- Step-by-step VPC Peering example

---

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

- **AWS Command Sheet:**
  [https://aws-command.vercel.app/](https://aws-command.vercel.app/)

- **CIDR Calculater:**
  [https://cidr.xyz/](https://cidr.xyz/)

---

## What is AWS VPC?

**Amazon VPC stands for Virtual Private Cloud.**

A VPC is your own isolated network inside AWS. AWS describes it as a virtual network dedicated to your AWS account where you can define IP ranges, create subnets, configure route tables, attach gateways, and apply security controls.

Think of it like this:

```text
AWS Cloud
 └── Your VPC
      ├── Public Subnet
      ├── Private Subnet
      ├── Route Tables
      ├── Internet Gateway
      ├── Security Groups
      └── EC2 / RDS / Load Balancer
```

In a traditional data center, you create networks, switches, routers, firewalls, and subnets manually.

In AWS, VPC gives you similar networking control, but in a cloud-native way.

![VPC Dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kxq1arf27apewpifbhsc.png)

![Image vpc](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rurk2a5e0g0ryy0408md.png)

---

## Why VPC is Important

Without a VPC, your cloud architecture has no proper network boundary.

A VPC helps you:

- Isolate your AWS resources
- Control inbound and outbound traffic
- Separate public and private workloads
- Connect multiple AWS services securely
- Build production-ready cloud architecture
- Connect different VPCs using VPC Peering
- Connect AWS with on-premise networks using VPN or Direct Connect

Example:

```text
Public Subnet  → Load Balancer / Bastion Host
Private Subnet → Application Server / Database
```

This separation is one of the most important cloud security practices.

---

## CIDR and Subnetting Basics

Before understanding subnets, you need a little idea about **CIDR**.

CIDR stands for **Classless Inter-Domain Routing**.

In simple words, CIDR defines the IP address range of your network.

Example:

```text
10.0.0.0/16
```

This means your VPC has a large private IP range starting from `10.0.0.0`.

The `/16` tells how big the network is.

Common examples:

```text
10.0.0.0/16    → Large VPC range
10.0.1.0/24    → Smaller subnet range
10.0.2.0/24    → Another subnet range
```

Simple way to understand:

```text
VPC CIDR:       10.0.0.0/16

Public Subnet:  10.0.1.0/24
Private Subnet: 10.0.2.0/24
DB Subnet:      10.0.3.0/24
```

So the VPC is the big network, and subnets are smaller parts inside that network.

---

## What is a Subnet?

A **subnet** is a smaller IP range inside your VPC.

AWS says a subnet is a range of IP addresses inside your VPC where you can launch resources like EC2 instances. ([AWS Documentation][1])

Example:

```text
VPC: 10.0.0.0/16

Subnet A: 10.0.1.0/24
Subnet B: 10.0.2.0/24
Subnet C: 10.0.3.0/24
```

Subnets are created inside **Availability Zones**.

Example:

```text
VPC: 10.0.0.0/16

AZ-1:
 └── Public Subnet: 10.0.1.0/24

AZ-2:
 └── Private Subnet: 10.0.2.0/24
```

## ![Subnetting description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uoz65q90n90dzr5adaqg.png)

## Public Subnet vs Private Subnet

A subnet becomes **public or private based on its route table**.

### Public Subnet

A public subnet has a route to the internet through an Internet Gateway.

Example route:

```text
Destination: 0.0.0.0/0
Target: Internet Gateway
```

Public subnet is useful for:

- Load balancer
- Bastion host
- Public EC2 instance
- NAT Gateway

---

### Private Subnet

A private subnet does not have a direct route to the Internet Gateway.

Private subnet is useful for:

- Application servers
- Databases
- Internal services
- Backend workloads

Example:

```text
Private Subnet
 └── EC2 App Server
 └── RDS Database
```

In production, databases should usually stay in private subnets.

---

## Route Tables

A **route table** controls where network traffic goes.

AWS defines a route table as a set of rules, called routes, that decide where traffic from your subnet or gateway is directed. ([AWS Documentation][2])

Example route table:

```text
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         igw-xxxxxxxx
```

Meaning:

```text
10.0.0.0/16 → Traffic inside VPC
0.0.0.0/0   → Traffic to internet through Internet Gateway
```

Every subnet must be associated with a route table. A subnet can be associated with only one route table at a time, but one route table can be associated with multiple subnets.

![Route table description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7wutxywwliw8bl3u496b.png)

---

## Internet Gateway

An **Internet Gateway** allows communication between your VPC and the internet.

For a subnet to become public, two things are required:

```text
1. Internet Gateway attached to VPC
2. Route table route:
   0.0.0.0/0 → Internet Gateway
```

![Internet gateway](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hlm796iiptxcuax3k6y5.png)

Architecture:

```text
Internet
   ↓
Internet Gateway
   ↓
Public Route Table
   ↓
Public Subnet
   ↓
EC2 Instance
```

Without an Internet Gateway route, your subnet will not be publicly reachable.

![Internet gateway](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0d4yx4d7xqx8klyhbqyk.png)

---

## Security Groups

A **Security Group** acts like a virtual firewall for AWS resources.

AWS explains that a security group controls traffic allowed to reach an instance, and only traffic allowed by security group rules can reach that resource.

Security Groups are attached to resources like:

- EC2
- RDS
- Load Balancer
- Lambda inside VPC

Example Security Group rule:

```text
Inbound Rules:

Type        Port     Source
SSH         22       Your IP
HTTP        80       0.0.0.0/0
HTTPS       443      0.0.0.0/0
```

Important point:

Security Groups are **stateful**.

That means if inbound traffic is allowed, response traffic is automatically allowed.

---

## VPC Peering

**VPC Peering** allows two VPCs to communicate privately using private IP addresses.

Example:

```text
VPC-A: 10.0.0.0/16
VPC-B: 192.168.0.0/16
```

After VPC Peering:

```text
EC2 in VPC-A can communicate with EC2 in VPC-B privately.
```

Use cases:

- Connect two application VPCs
- Connect shared services VPC with app VPC
- Connect dev VPC with monitoring VPC
- Connect VPCs across accounts
- Connect VPCs across regions

Important: VPC Peering does **not support overlapping CIDR blocks**. AWS states that you cannot create a VPC peering connection if the VPCs have matching or overlapping IPv4 or IPv6 CIDR blocks.

![vpc peering](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2lzqawqs973dg3xlscj0.png)

---

## Step-by-Step Example: Create VPC Peering

Let’s say we have two VPCs:

```text
VPC-A: 10.0.0.0/16
VPC-B: 192.168.0.0/16
```

Goal:

```text
EC2 instance in VPC-A should communicate with EC2 instance in VPC-B.
```

---

## Step 1: Create VPC-A

Go to:

```text
AWS Console → VPC → Create VPC
```

Create:

```text
Name: VPC-A
CIDR: 10.0.0.0/16
```

Create subnet:

```text
Name: VPC-A-Private-Subnet
CIDR: 10.0.1.0/24
```

---

## Step 2: Create VPC-B

Create second VPC:

```text
Name: VPC-B
CIDR: 192.168.0.0/16
```

Create subnet:

```text
Name: VPC-B-Private-Subnet
CIDR: 192.168.1.0/24
```

---

## Step 3: Launch EC2 Instances

Launch one EC2 instance in each VPC.

```text
EC2-A → VPC-A → 10.0.1.0/24 subnet
EC2-B → VPC-B → 192.168.1.0/24 subnet
```

Make sure both instances have private IPs.

Example:

```text
EC2-A Private IP: 10.0.1.10
EC2-B Private IP: 192.168.1.10
```

---

## Step 4: Create VPC Peering Connection

Go to:

```text
VPC Console → Peering Connections → Create Peering Connection
```

Fill details:

```text
Name: VPC-A-to-VPC-B
Requester VPC: VPC-A
Accepter VPC: VPC-B
```

Click:

```text
Create Peering Connection
```

---

## Step 5: Accept Peering Request

Go to:

```text
VPC → Peering Connections
```

Select the request.

Click:

```text
Actions → Accept Request
```

Now the peering connection status should become:

```text
Active
```

---

## Step 6: Update Route Table of VPC-A

Go to VPC-A route table.

Add route:

```text
Destination: 192.168.0.0/16
Target: VPC Peering Connection
```

AWS requires route tables on both sides to be updated so private IPv4 traffic can flow between peered VPCs. The destination should be the peer VPC CIDR and the target should be the VPC peering connection. ([AWS Documentation][6])

---

## Step 7: Update Route Table of VPC-B

Go to VPC-B route table.

Add route:

```text
Destination: 10.0.0.0/16
Target: VPC Peering Connection
```

Now both VPCs know how to reach each other.

---

## Step 8: Update Security Groups

For EC2-A security group, allow traffic from VPC-B:

```text
Type: ICMP / SSH / Custom TCP
Source: 192.168.0.0/16
```

For EC2-B security group, allow traffic from VPC-A:

```text
Type: ICMP / SSH / Custom TCP
Source: 10.0.0.0/16
```

For testing ping:

```text
Allow ICMP
```

For testing SSH:

```text
Allow TCP 22
```

---

## Step 9: Test Connectivity

Login to EC2-A and ping EC2-B private IP:

```bash
ping 192.168.1.10
```

Or test SSH:

```bash
ssh ec2-user@192.168.1.10
```

If route tables and security groups are correct, communication should work privately.

---

## Final Thoughts

AWS VPC is one of the most important concepts in cloud networking.

If you understand VPC properly, then services like EC2, Load Balancer, RDS, EKS, Lambda networking, VPN, Direct Connect, and Transit Gateway become much easier.

At a high level, remember this:

```text
VPC = Your private network in AWS
Subnet = Smaller network inside VPC
Route Table = Traffic direction rules
Internet Gateway = Internet access
Security Group = Firewall for resources
VPC Peering = Private connection between two VPCs
```
