# Day 10 — AWS EC2

Modern applications don’t survive on a single server anymore.

Whether you're building:

- A fintech platform
- SaaS application
- E-commerce system
- AI backend
- DevSecOps pipeline
- High-traffic API

You need infrastructure that can:

✅ Scale automatically
✅ Handle traffic spikes
✅ Stay highly available
✅ Recover from failures
✅ Distribute traffic intelligently

And this is exactly where AWS compute services shine.

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

- **AWS Command Sheet:**
  [https://aws-command.vercel.app/](https://aws-command.vercel.app/)

---

## ☁️ What is EC2?

Amazon Web Services EC2 (Elastic Compute Cloud) is AWS’s virtual machine service.

Think of EC2 as:

```text
Your own server inside AWS data centers
```

Instead of buying physical hardware:

- CPU
- RAM
- Storage
- Networking

AWS gives them virtually on-demand.

![EC2 Dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4o7xqo3tk4jxqx9jocy9.png)

---

## 🧠 Why EC2 Became So Popular

Before cloud computing:

❌ Companies purchased expensive servers
❌ Scaling took weeks/months
❌ Hardware failures caused downtime
❌ Infrastructure was static

EC2 changed everything.

Now you can:

✅ Launch servers in minutes
✅ Scale globally
✅ Pay only for usage
✅ Automate deployments
✅ Recover automatically

This is why almost every modern startup uses cloud infrastructure.

---

## 🏗️ EC2 Core Concepts

---

## 🖥️ 1️⃣ Instance Types

Different workloads need different hardware.

AWS provides multiple EC2 families:

| Family     | Purpose            |
| ---------- | ------------------ |
| T-Series   | General purpose    |
| C-Series   | Compute optimized  |
| R-Series   | Memory optimized   |
| M-Series   | Balanced workloads |
| P/G-Series | GPU/AI workloads   |

Example:

```text
t3.micro → small testing server
c7g.large → compute-heavy backend
r6g.large → databases
g5.xlarge → AI/ML workloads
```

---

## 💾 2️⃣ AMI (Amazon Machine Image)

AMI is the blueprint of a server.

It contains:

- Operating system
- Installed software
- Configuration
- Packages

Examples:

- Ubuntu AMI
- Amazon Linux
- Windows Server
- Custom enterprise images

---

## 🔐 3️⃣ Security Groups

Security Groups act like firewalls.

Example:

| Port | Purpose |
| ---- | ------- |
| 22   | SSH     |
| 80   | HTTP    |
| 443  | HTTPS   |

Best practice:

```text
Never allow 0.0.0.0/0 on SSH in production.
```

---

## A Quick Glimpse of the EC2 Setup Process

![EC2 First Screenshot](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pyddgjws1jao4tpk73zs.png)

![EC2 Second](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6mkky1f82kg2q6apcw3k.png)

![EC2 Running](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2saiws975l2k23s5d6xl.png)

---

## 🚀 Launch Templates

Now let’s move toward automation.

---

### 🤔 What is a Launch Template?

A Launch Template is a reusable EC2 configuration blueprint.

Instead of manually configuring servers every time, AWS can automatically launch instances using predefined settings.

Launch Templates include:

- AMI
- Instance type
- Security Groups
- IAM role
- User data scripts
- Storage config
- Key pairs

![Launch Template](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/18dpsqpx89eu7ttk47lq.png)

---

## 🧠 Why Launch Templates Matter

Imagine scaling from:

```text
2 servers → 200 servers
```

Manually configuring each one would be impossible.

Launch Templates solve this problem.

---

## ⚙️ Real Example

Suppose you're running a fintech application.

Every server needs:

- Ubuntu
- Docker installed
- Node.js runtime
- Security monitoring agent
- Same firewall rules

Instead of manual setup:

```text
Create Launch Template once
→ Auto Scaling uses it automatically
```

---

## A Quick Glimpse of the Launch Template Setup Process

![Launch Template Second](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3xj5f20l7rp4v2jrspmy.png)

![Launch Template Third](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bw2f5cs73qb034ot9o10.png)

---

## 🛠️ User Data Scripts

One of the most powerful Launch Template features.

When EC2 starts:

AWS can automatically execute scripts.

Example:

```bash
#!/bin/bash
apt update -y
apt install docker.io -y
systemctl start docker
docker run nginx
```

This enables Infrastructure as Code behavior.

---

## 📈 Auto Scaling Groups (ASG)

This is where AWS becomes truly powerful.

---

## 🤔 What is Auto Scaling?

Auto Scaling automatically:

✅ Adds servers during high traffic
✅ Removes servers during low traffic
✅ Replaces unhealthy instances
✅ Maintains application availability

---

## 🧠 Why ASG is Important

Imagine:

Your application suddenly gets viral traffic.

Without Auto Scaling:

❌ Servers crash
❌ Website slows down
❌ Revenue loss
❌ Poor customer experience

With ASG:

✅ AWS automatically launches more EC2 instances

---

## ⚡ How ASG Works

Flow:

![ASG](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q56v5astxailcnsfp7i6.png)

This is how modern internet infrastructure scales.

---

## 🏗️ ASG Components

| Component        | Purpose                     |
| ---------------- | --------------------------- |
| Launch Template  | Defines instance config     |
| Desired Capacity | Number of running instances |
| Minimum Capacity | Lowest server count         |
| Maximum Capacity | Highest scaling limit       |

Example:

```text
Min = 2
Desired = 2
Max = 10
```

---

## 🔥 Scaling Policies

AWS supports multiple scaling strategies.

---

## 📊 Dynamic Scaling

Scale based on metrics:

- CPU utilization
- Memory
- Request count

Example:

```text
CPU > 70%
→ Add 2 instances
```

---

## ⏰ Scheduled Scaling

Useful for predictable traffic.

Example:

```text
Scale up every Friday evening
```

---

## 🎯 Target Tracking

AWS automatically tries to maintain a metric target.

Example:

```text
Keep CPU around 50%
```

This is one of the most commonly used production strategies.

---

## ❤️ Health Checks

ASG continuously checks server health.

If a server becomes unhealthy:

```text
Terminate instance
        ↓
Launch replacement automatically
```

This improves fault tolerance.

---

## 🌐 Load Balancing

Now we need a way to distribute traffic.

This is where Load Balancers enter.

---

## ⚖️ What is a Load Balancer?

A Load Balancer distributes incoming traffic across multiple servers.

Without it:

```text
All traffic → One server
```

With it:

```text
Traffic → Multiple servers evenly
```

Benefits:

✅ High availability
✅ Better performance
✅ Fault tolerance
✅ Scalability

![Load Balancer](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4bltd72pnhmy036d5w2i.png)

---

## 🚦 Types of AWS Load Balancers

AWS mainly provides:

| Type | Layer   | Best For                       |
| ---- | ------- | ------------------------------ |
| ALB  | Layer 7 | HTTP/HTTPS                     |
| NLB  | Layer 4 | TCP/UDP ultra-high performance |

---

## 🌍 Application Load Balancer (ALB)

ALB works at:

```text
OSI Layer 7 (Application Layer)
```

It understands:

- URLs
- HTTP headers
- Hostnames
- Paths

---

## 🧠 ALB Features

---

## 🔀 Path-Based Routing

Example:

```text
/api → Backend API servers
/images → Image servers
/admin → Admin application
```

Single ALB can route traffic intelligently.

---

## 🌐 Host-Based Routing

Example:

```text
api.company.com
blog.company.com
admin.company.com
```

ALB routes traffic based on hostname.

---

## 🔒 HTTPS Termination

ALB can handle SSL/TLS certificates directly.

This reduces backend server complexity.

---

## 🍪 Sticky Sessions

ALB can keep users connected to the same server session.

Useful for legacy applications.

---

## ⚡ Network Load Balancer (NLB)

NLB works at:

```text
OSI Layer 4 (Transport Layer)
```

It is built for:

✅ Ultra-high performance
✅ Millions of requests/sec
✅ Low latency
✅ TCP/UDP traffic

---

## 🧠 When to Use NLB

NLB is perfect for:

- Gaming servers
- Financial transaction systems
- Real-time applications
- VoIP
- TCP services

---

## ⚔️ Load Balancer

![ALB Vs NLB](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2709aomyrw5085n3476m.png)

---

## 🏗️ Real Production Possible Architecture

Typical scalable AWS architecture:

![Possible AWS ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hesk5ynmpjokjzgvewpb.png)

This is the foundation of most modern cloud-native applications.

---

## 🔥 High Availability Architecture

Production systems usually deploy across multiple AZs.

Example:

![multi az architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fxd3pg2jndtqigvmm8av.png)

If one Availability Zone fails:

✅ Traffic automatically shifts

---

## 💰 Cost Optimization

---

## ![cost optimization](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/p3n33qzepwcck3uyi26a.png)

## 🔐 Security Best Practices

---

### ✅ Use IAM Roles Instead of Access Keys

Never hardcode credentials inside servers.

---

### ✅ Restrict Security Groups

Allow only required ports.

---

### ✅ Enable Monitoring

Use:

- CloudWatch
- VPC Flow Logs
- GuardDuty

---

### ✅ Patch Regularly

Outdated instances become security risks.

---

## 🧠 Final Thoughts

EC2 is the core foundation of AWS infrastructure engineering.

If Kubernetes is the future of orchestration…

Then EC2 is still the foundation underneath most cloud environments.

Understanding these concepts deeply will help you in:

- DevOps
- Cloud Engineering
- Site Reliability Engineering
- DevSecOps
- Platform Engineering
- Fintech infrastructure

Because modern infrastructure is not just about launching servers anymore.

It’s about:

```text
Automation
Scalability
Resilience
Availability
Security
```

And AWS provides all of them at massive scale.
