# Day 7 — Cloud Computing & AWS Fundamentals

Cloud computing is no longer optional.

Whether you're building modern applications, deploying containers, running AI workloads, hosting websites, automating DevOps pipelines, or scaling startups — cloud platforms power almost everything today.

If you're entering:

- DevOps
- Cloud Engineering
- Cybersecurity
- Backend Development
- Platform Engineering
- AI Infrastructure

then understanding cloud fundamentals is one of the best investments you can make.

Let’s start from the foundation.

## 🔗 Resources

- **GitHub Repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

---

## 🌍 What is Cloud Computing?

Cloud computing means using computing resources over the internet instead of managing physical infrastructure yourself.

Instead of buying:

- physical servers
- networking hardware
- storage devices
- cooling systems
- data center space

you rent resources from cloud providers on-demand.

Think of cloud like electricity.

You don’t build a power plant to use electricity.

Similarly, you don’t need to build a data center to run applications anymore.

---

## ⚡ Why Cloud Computing Became So Popular

Traditional infrastructure had many problems:

- High upfront costs
- Scaling issues
- Slow provisioning
- Hardware maintenance
- Downtime risks
- Complex disaster recovery

Cloud solved this by introducing:

✅ Pay-as-you-go pricing
✅ Global scalability
✅ High availability
✅ Fast deployments
✅ Managed services
✅ Built-in security tooling
✅ Infrastructure automation

This completely changed how software is built and deployed.

---

## 🧠 Core Cloud Computing Service Model

Before jumping into AWS, understand these important Service Models.

![Pass SaaS ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/93q0h2hc25pb8m6yh1yn.png)

---

## 🖥 Infrastructure as a Service (IaaS)

You rent infrastructure components like:

- virtual machines
- networking
- storage
- load balancers

Example:

- Amazon EC2
- Azure Virtual Machines
- Google Compute Engine

You manage:

- OS
- applications
- runtime
- security patches

Cloud provider manages:

- hardware
- networking
- physical security

---

## ⚙️ Platform as a Service (PaaS)

The cloud provider manages the infrastructure and runtime.

You focus only on your application code.

Examples:

- AWS Elastic Beanstalk
- Azure App Service
- Google App Engine

---

## 🚀 Software as a Service (SaaS)

Fully managed software delivered over the internet.

Examples:

- Gmail
- Slack
- Zoom
- Microsoft 365

You simply use the software.

No infrastructure management needed.

---

## ☁️ Types of Cloud Computing

---

### 🌐 Public Cloud

Public cloud means infrastructure is owned and managed by cloud providers.

Examples:

- Amazon Web Services (AWS)
- Microsoft Azure
- Google Cloud Platform (GCP)

You share underlying infrastructure with other customers but your workloads remain logically isolated.

### Advantages

✅ Cost effective
✅ Highly scalable
✅ Global infrastructure
✅ Massive service ecosystem
✅ Fast deployment

### Best For

- Startups
- Modern applications
- SaaS platforms
- DevOps environments
- AI workloads

---

### 🏢 Private Cloud

Private cloud is dedicated infrastructure used by a single organization.

It can be hosted:

- on-premises
- in private data centers
- through dedicated cloud setups

### Advantages

✅ More control
✅ Custom security policies
✅ Regulatory compliance
✅ Better for sensitive workloads

### Best For

- Banks
- Government systems
- Healthcare organizations
- Large enterprises

---

### 🔀 Hybrid Cloud

Hybrid cloud combines:

- public cloud
- private cloud
- on-premises infrastructure

Organizations keep sensitive systems private while scaling workloads in public cloud.

This is extremely common in enterprises today.

---

## 📊 Cloud Providers in Market

The cloud industry is dominated by three major players:

![Cloud Provider Types](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ztn6pt6g31kvxs13zezc.png)

Together, these providers control nearly 70% of the global cloud market.

---

## 🥇 Amazon Web Services (AWS)

Amazon Web Services remains the market leader in 2026.

Why?

Because AWS offers:

- 200+ cloud services
- massive global infrastructure
- mature ecosystem
- strongest community support
- enterprise adoption
- startup friendliness
- powerful DevOps integrations

AWS also dominates cloud-related job postings globally. ([CloudPros][1])

---

## 🔵 Microsoft Azure

Microsoft Azure is extremely strong in enterprise environments.

Its biggest strengths are:

- Microsoft ecosystem integration
- Active Directory
- Office 365 integration
- enterprise compliance
- hybrid cloud support

Large corporations heavily prefer Azure.

---

## 🔴 Google Cloud Platform (GCP)

Google Cloud Platform is famous for:

- Kubernetes leadership
- BigQuery
- AI/ML tooling
- data engineering
- global networking

Many AI-first companies choose GCP because of its data and machine learning ecosystem. ([Reddit][2])

---

## 🚀 Why Beginners Usually Start with AWS

Most beginners start with AWS because:

✅ Largest job market
✅ Massive learning resources
✅ Huge community
✅ Strong free tier
✅ Broadest service coverage
✅ Industry-standard cloud concepts

Learning AWS fundamentals also makes learning Azure and GCP easier later.

---

## 🧾 AWS Prerequisites Before Learning

Before starting AWS seriously, you should have:

---

## 💻 Basic Linux Knowledge

Understand:

- file systems
- permissions
- package management
- shell commands

Important commands:

```bash
ls
cd
mkdir
rm
chmod
chown
grep
cat
```

---

## 🌐 Basic Networking Concepts

You should know:

- IP addresses
- DNS
- HTTP/HTTPS
- ports
- firewalls
- routing

These concepts become critical in cloud networking.

---

## 🔐 Basic Security Understanding

Learn:

- IAM basics
- authentication
- authorization
- SSH keys
- least privilege principle

Cloud security is one of the most important skills today.

---

## 🐳 Optional but Helpful

These are not mandatory but highly useful:

- Git & GitHub
- Docker
- CI/CD basics
- Kubernetes basics

---

### 🪪 Step 1: Create an AWS Account

To start learning AWS:

![AWS Sign Up Page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/u3qqq2uzxlv3yavchlrc.png)

1. Go to AWS official website
2. Create a free-tier account
3. Add billing information
4. Enable MFA (Multi-Factor Authentication)
5. Create an IAM user instead of using root account daily

Using the root account regularly is considered bad practice.

---

### 🌍 AWS Regions & Availability Zones

This is one of the MOST important AWS concepts.

---

### 📍 AWS Region

A Region is a geographical location where AWS has data centers.

Examples:

- us-east-1
- ap-south-1
- eu-west-1

Each region is isolated from others.

---

### 🏢 Availability Zone (AZ)

An Availability Zone is one or more physically separate data centers inside a region.

Example:

```text
Region: ap-south-1 (Mumbai)

AZs:
- ap-south-1a
- ap-south-1b
- ap-south-1c
```

Applications are deployed across multiple AZs for:

✅ High availability
✅ Fault tolerance
✅ Disaster recovery

---

### 🧠 Easy Analogy

Think of it like this:

```text
Country → State → Buildings

Region → Availability Zones → Data Centers
```

---

## ⚙️ Core AWS Services Every Beginner Should Learn

---

### 🖥 Amazon EC2 (Elastic Compute Cloud)

Virtual machines in AWS.

Used for:

- hosting applications
- web servers
- backend APIs
- databases

EC2 is foundational AWS knowledge.

---

### 🪣 Amazon S3 (Simple Storage Service)

Object storage service.

Used for:

- backups
- static websites
- media storage
- logs
- data lakes

S3 is one of the most widely used AWS services.

---

### 🌐 Amazon VPC (Virtual Private Cloud)

Allows you to create isolated cloud networks.

You control:

- subnets
- routing tables
- firewalls
- internet access

This is where networking becomes important.

---

### 🔐 IAM (Identity and Access Management)

Controls permissions in AWS.

You manage:

- users
- roles
- groups
- policies

IAM is the heart of AWS security.

---

### ⚖️ Elastic Load Balancer (ELB)

Distributes traffic across multiple servers.

Benefits:

✅ High availability
✅ Scalability
✅ Better fault tolerance

---

### 📈 Auto Scaling

Automatically increases or decreases infrastructure based on traffic.

This is one of cloud computing’s biggest advantages.

---

### 🗄 Amazon RDS

Managed relational database service.

Supports:

- MySQL
- PostgreSQL
- MariaDB
- SQL Server

AWS handles backups, patching, and maintenance.

---

### 🧱 CloudFormation

Infrastructure as Code (IaC) service.

You define infrastructure using templates.

Modern cloud engineering heavily depends on automation.

- Cloud Formation Templates
- Terraform

---

## 🔐 AWS Shared Responsibilty Model:

![Shared Responsbility Model](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z7e301xumvzxcuw9kjr5.png)

This concept is VERY important.

Many beginners misunderstand cloud security.

---

### 🤝 What AWS Handles

AWS is responsible for:

✅ Physical servers
✅ Data centers
✅ Networking hardware
✅ Hypervisors
✅ Physical security

---

### 👨‍💻 What YOU Handle

You are responsible for:

✅ IAM permissions
✅ Application security
✅ OS patching (EC2)
✅ Data encryption
✅ Security groups
✅ Network configuration

---

### 🧠 Simple Rule

```text
Security OF the cloud → AWS
Security IN the cloud → You
```

This is the core idea behind the shared responsibility model.

---

### 🎯 Final Thoughts

Cloud computing is now the backbone of modern technology.

Every major industry today relies on cloud platforms for:

- scalability
- automation
- security
- AI workloads
- application hosting
- global infrastructure

If you're serious about DevOps, backend engineering, cybersecurity, platform engineering, or modern software development — cloud fundamentals are non-negotiable.

Start small.

Learn the basics deeply.

Understand networking.

Understand security.

Then build projects consistently.

Because in 2026, cloud knowledge is no longer a bonus skill.

It’s a core engineering skill.
