# Day 8 - IAM & AWS CLI

Cloud security starts with one thing:

> **Who can access what?**

That’s exactly where **IAM (Identity and Access Management)** comes in.

Whether you're:

- a Cloud Engineer ☁️
- DevOps Engineer ⚙️
- Security Engineer 🛡️
- Backend Developer 👨‍💻
- or preparing for AWS certifications 📚

Understanding IAM is absolutely mandatory.

And once IAM is understood, the next powerful step is using the **AWS CLI** to interact with AWS directly from your terminal like a real cloud engineer.

In this guide we'll cover:

- What IAM is
- IAM Users
- IAM Roles
- IAM Policies
- MFA (Multi-Factor Authentication)
- AWS CLI setup
- Real-world best practices
- Security mistakes beginners make

---

## 🔗 Resources

- **GitHub Repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

- **AWS Command Sheet:**
  [https://aws-command.vercel.app/](https://aws-command.vercel.app/)

---

## ☁️ What is IAM?

IAM stands for:

> **Identity and Access Management**

It is the AWS service used to control:

- Authentication → _Who are you?_
- Authorization → _What can you do?_

Think of IAM as the **security guard of AWS**.

Without IAM, anyone could access:

- EC2 servers
- S3 buckets
- Databases
- Secrets
- Billing data

And that would become a disaster very quickly.

---

## 🏢 Real-World Example

Imagine a company has:

- Developers
- DevOps Engineers
- Security Team
- Finance Team
- Interns

Should everyone get full AWS admin access?

❌ Absolutely not.

Instead:

| Team       | Access             |
| ---------- | ------------------ |
| Developers | EC2 + Logs         |
| DevOps     | Infrastructure     |
| Finance    | Billing only       |
| Security   | Audit + Monitoring |
| Interns    | Read-only          |

IAM makes this possible.

---

## 🧠 Core IAM Components

AWS IAM mainly consists of:

![Iam DashBoard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jw04997sy5uphgjdt4as.png)

```text
IAM
├── Users
├── Groups
├── Roles
├── Policies
└── MFA
```

---

## 👤 IAM Users

An IAM User represents a person or application that needs access to AWS.

![Iam User](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6wvp5mll8xfentuun4nf.png)

Examples:

- Rahul
- DevOps Engineer
- CI/CD Pipeline
- Jenkins Server
- Terraform Automation

Each IAM user can have:

- Password
- Access Keys
- Permissions
- MFA

---

## 🔑 Types of IAM Access

### 1️⃣ Console Access

Used for:

- AWS Web Dashboard login

Example:

```text
https://aws.amazon.com/console/
```

Uses:

- Username
- Password
- MFA

---

### 2️⃣ Programmatic Access

Used for:

- AWS CLI
- SDKs
- Terraform
- CI/CD Pipelines

Uses:

- Access Key ID
- Secret Access Key

Example:

```bash
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

---

## ⚠️ Important Security Rule

Never use the **Root Account** for daily work.

Root account has unlimited permissions.

If compromised:

💀 Entire AWS account can be destroyed.

Instead:

✅ Create IAM users.

---

## 👥 IAM Groups

Groups help manage permissions more easily.

Instead of assigning permissions individually:

![DevOps Group](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2s12le7olzfc51cgc6r2.png)

```text
Rahul → EC2 Access
Aman → EC2 Access
Riya → EC2 Access
```

You create:

```text
Developers Group → EC2 Access
```

Then add users to the group.

Much cleaner.

---

## 🛡️ IAM Policies

Policies define permissions.

They are written in **JSON**.

Policies answer:

> What actions are allowed or denied?

---

## 📄 Example IAM Policy

This policy gives read-only access to S3 buckets:

![Policy](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eq5yybwmnb29823uf00w.png)

---

## 🧠 Understanding Policy Structure

| Component | Meaning          |
| --------- | ---------------- |
| Effect    | Allow or Deny    |
| Action    | AWS API actions  |
| Resource  | Which resources  |
| Statement | Permission block |

---

## 🚫 Principle of Least Privilege

One of the most important cloud security principles.

Meaning:

> Give only the permissions that are actually required.

Bad Example ❌

```json
"Action": "*",
"Resource": "*"
```

This gives full admin access.

Good Example ✅

```json
"s3:GetObject"
```

Only specific access.

---

## 🎭 IAM Roles

Roles are extremely important in AWS.

A Role is a temporary identity with permissions.

![Iam Roles](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cf07qz2xp3itvxmm76xn.png)

Unlike users:

- Roles do NOT have passwords
- Roles do NOT have permanent access keys

Instead:

✅ AWS provides temporary credentials automatically.

---

## 🧠 Why Roles Matter

Roles are heavily used for:

- EC2 instances
- Lambda functions
- ECS containers
- Cross-account access
- Kubernetes workloads
- CI/CD systems

---

## 🚀 Example: EC2 Accessing S3

Suppose an EC2 server needs access to an S3 bucket.

❌ Wrong Approach:

Store AWS keys inside server files.

Huge security risk.

✅ Correct Approach:

Attach an IAM Role to EC2.

AWS automatically provides temporary credentials securely.

---

## 🔄 User vs Role

| IAM User                 | IAM Role              |
| ------------------------ | --------------------- |
| Permanent identity       | Temporary identity    |
| Has password/access keys | Temporary credentials |
| Used by humans           | Used by services/apps |
| Long-term access         | Short-term access     |

---

## 🔐 MFA (Multi-Factor Authentication)

MFA adds an extra security layer.

Instead of only:

```text
Password
```

You also need:

```text
OTP / Authenticator Code
```

![MFA First](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e188hjkd2w04wupw3wmp.png)

---

## 📱 Common MFA Methods

| MFA Type          | Example              |
| ----------------- | -------------------- |
| Authenticator App | Google Authenticator |
| Hardware Key      | YubiKey              |
| SMS               | OTP Messages         |

---

## ⚠️ Why MFA is Critical

Even if hackers steal passwords:

✅ They still cannot login without MFA.

AWS strongly recommends enabling MFA for:

- Root Account
- Admin Users
- Production Accounts

---

## 🔥 Real Industry Fact

Many cloud breaches happen because:

- Access keys leaked
- No MFA enabled
- Over-permissioned IAM users

Cloud security failures are often **identity failures**.

---

## 💻 What is AWS CLI?

AWS CLI stands for:

> AWS Command Line Interface

It allows you to manage AWS directly from the terminal.

Instead of clicking in the console:

You can automate everything:

```bash
aws s3 ls
```

---

## 🚀 Why AWS CLI is Powerful

With CLI you can:

- Automate infrastructure
- Create scripts
- Manage EC2
- Upload to S3
- Configure IAM
- Integrate CI/CD
- Manage Kubernetes
- Use Terraform pipelines

Professional cloud engineers use CLI daily.

---

### 🛠️ Installing AWS CLI

## 🐧 Linux

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install
```

---

## 🪟 Windows

Download from:

[AWS CLI Official Installer](https://aws.amazon.com/cli/?utm_source=chatgpt.com)

---

## 🍎 macOS

```bash
brew install awscli
```

---

## ✅ Verify Installation

Run:

```bash
aws --version
```

Example:

```bash
aws-cli/2.27.0 Python/3.x
```

---

## ⚙️ Configure AWS CLI

![aws cli](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5a93vmn4y69e4rhigw1u.png)

---

## 📁 AWS CLI Configuration Files

AWS stores credentials here:

```bash
~/.aws/credentials
```

And config here:

```bash
~/.aws/config
```

---

## ⚠️ Never Commit AWS Keys to GitHub

One of the biggest beginner mistakes.

If keys leak publicly:

- Attackers can use your AWS account
- Crypto mining attacks happen
- Huge AWS bills occur

Use:

- IAM Roles
- Secrets Managers
- Environment Variables

Instead.

---

## 🧪 Useful AWS CLI Commands

### List S3 Buckets

```bash
aws s3 ls
```

---

### List EC2 Instances

```bash
aws ec2 describe-instances
```

---

### List IAM Users

```bash
aws iam list-users
```

---

### Get Current Identity

```bash
aws sts get-caller-identity
```

This is extremely useful for debugging permissions.

---

## 🧠 AWS STS (Security Token Service)

STS provides temporary credentials.

Used heavily with:

- IAM Roles
- Federation
- Kubernetes IAM
- Cross-account access

This is one of the most important concepts in enterprise AWS security.

---

## 🏢 Real Enterprise IAM Practices

Large companies usually implement:

✅ SSO (Single Sign-On)
✅ MFA everywhere
✅ Role-based access
✅ Temporary credentials
✅ Permission boundaries
✅ IAM Access Analyzer
✅ Audit logging with CloudTrail

---

## 🔥 Common IAM Mistakes

## ❌ Using Root Account Daily

Very dangerous.

---

## ❌ Giving AdminAccess to Everyone

Creates massive attack surface.

---

## ❌ Hardcoding AWS Keys

Common breach reason.

---

## ❌ No MFA

Huge security risk.

---

## ❌ Overly Permissive Policies

Avoid:

```json
"Action": "*"
```

---

## ☁️ IAM + DevOps + Security

IAM connects with almost everything in AWS:

| Service          | IAM Usage            |
| ---------------- | -------------------- |
| EC2              | Instance Roles       |
| Lambda           | Execution Roles      |
| Kubernetes (EKS) | IAM Service Accounts |
| Terraform        | Automation Access    |
| CI/CD            | Pipeline Permissions |
| CloudTrail       | Audit Logs           |

IAM is the backbone of AWS security.

---

## 🧠 Final Thoughts

If networking is the foundation of cloud…

Then IAM is the foundation of cloud security.

Most real-world AWS problems are not caused by:

- EC2
- Kubernetes
- Lambda

They’re caused by:

❌ Wrong permissions
❌ Exposed credentials
❌ Weak access control

Mastering IAM early will make you a much stronger:

- Cloud Engineer
- DevOps Engineer
- Security Engineer
- Platform Engineer

And AWS CLI will help you automate everything professionally.
