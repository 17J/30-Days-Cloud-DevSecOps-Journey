# Day 9 — AWS S3

Modern cloud applications store massive amounts of data.

From:

- website images
- backups
- logs
- videos
- static frontend files
- financial reports

everything needs scalable storage.

That’s where Amazon S3 comes in.

And honestly?

If you’re learning Cloud, DevOps, or Security Engineering, then understanding **S3 buckets and bucket policies** is absolutely mandatory.

Because most real-world cloud breaches happen due to:

- publicly exposed buckets
- weak bucket permissions
- misconfigured policies
- accidentally leaked sensitive files

In this blog, we’ll deeply understand:

- What S3 buckets are
- Bucket policies
- Static website hosting
- Public access risks
- Real-world security mistakes
- Best practices used in enterprises

---

## 🔗 Resources

- **GitHub Repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

- **AWS Command Sheet:**
  [https://aws-command.vercel.app/](https://aws-command.vercel.app/)

- **AWS Policy Generator:**
  [https://awspolicygen.s3.amazonaws.com/policygen.html](https://awspolicygen.s3.amazonaws.com/policygen.html)

---

## 🪣 What is an S3 Bucket?

![s3 dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j3sqcmjbzha184aa5whr.png)

An S3 bucket is basically a container used to store files in AWS.

Think of it like:

```text
Google Drive Folder → But infinitely scalable
```

Inside buckets, you can store:

- Images
- Videos
- Backups
- Application logs
- PDFs
- Website files
- Terraform states
- Kubernetes backups

AWS S3 is designed for:

- high durability
- massive scalability
- global availability

AWS claims:

> S3 provides 99.999999999% durability.

That’s 11 nines.

---

## 🏗️ S3 Architecture

![s3 architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cslp6te0dgbq4lubar9s.png)

---

## 📦 Creating Your First Bucket

When creating a bucket, AWS asks for:

- Bucket name
- Region
- Object ownership
- Public access settings
- Versioning
- Encryption

Example bucket names:

![s3 bucket](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fhqmz81hrgghjjyju0f2.png)

⚠️ Bucket names must be globally unique.

---

## 🌍 S3 Bucket Use Cases

### Common Enterprise Use Cases

| Use Case               | Example             |
| ---------------------- | ------------------- |
| Static website hosting | React frontend      |
| Backup storage         | DB backups          |
| Log storage            | CloudTrail logs     |
| Data lake              | Analytics           |
| Disaster recovery      | Encrypted snapshots |
| Media storage          | Images/videos       |

---

## 🔐 Understanding Bucket Policies

![s3 images](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xpxgwykbm66kwyo0c6ae.png)

```text
User Request → Policy Evaluation → Allow / Deny
```

Bucket policies control:

> Who can access your bucket and what they can do.

These are JSON-based IAM policies attached directly to the bucket.

Example permissions:

- Read objects
- Upload files
- Delete files
- Public access
- Restrict access by IP
- Enforce HTTPS

---

## 🧠 Example Bucket Policy

![s3 bucket policy](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/019z9aquw6l1qbtoitvr.png)

This policy means:

```text
Anyone on the internet can read files from this bucket.
```

Perfect for:

- public websites
- images
- CSS/JS files

Dangerous for:

- financial documents
- backups
- secrets

---

## 🚨 Public Access Risks (Most Important Section)

This is where most beginners make mistakes.

Thousands of companies have leaked sensitive data because:

- S3 buckets became public
- developers disabled block public access
- bucket policies were misconfigured

Real leaked data examples:

- customer records
- bank documents
- API keys
- employee data
- application backups

---

## ⚠️ Common S3 Misconfigurations

### 1️⃣ Public Buckets

```text
Everyone on the internet can access files.
```

---

### 2️⃣ No Encryption

Sensitive files stored without:

- SSE-S3
- SSE-KMS

---

### 3️⃣ Weak IAM Permissions

Example:

```text
s3:*
```

for all users.

Very dangerous.

---

### 4️⃣ No Logging Enabled

Without logging:

- no audit trail
- difficult incident response

---

### 5️⃣ Exposed Terraform State Files

Many DevOps teams accidentally expose:

```text
terraform.tfstate
```

which may contain:

- secrets
- passwords
- infrastructure details

---

## 🌐 Static Website Hosting Using S3

One of the coolest features of S3:

You can host static websites directly from a bucket.

Perfect for:

- portfolio websites
- React apps
- documentation
- landing pages

---

## ⚙️ How Static Website Hosting Works

![s3 website hosting flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5vqpxoyoc9xicrq4b560.png)

---

## 🛠️ Steps to Enable Static Website Hosting

### Step 1 — Create Bucket

Example:

```text
30-days-cloud-devsecops-journey-day9-s3
```

---

### Step 2 — Upload Files

Upload:

- index.html
- error.html

---

### Step 3 — Enable Static Website Hosting

Go to:

```text
Properties → Static Website Hosting
```

Enable it.

Specify:

```text
Index document → index.html
```

---

### Step 4 — Add Bucket Policy

Allow public read access.

---

### Step 5 — Access Website URL

AWS generates:

```text
http://30-days-cloud-devsecops-journey-day9-s3.s3-website-region.amazonaws.com
```

---

## 🎨 Website Hosting Screenshots

### 1. Uploading Files For Website hosting

![Website hosting](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7mebu7h9x0ey7axu32xr.png)

### 2. Url Of Static Website

![Website second hosting](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/phei955o0chs4qcfgi8b.png)

###3. Output of Hosted Website

![s3 static hosting](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xvhljr2725gy4hh55oj0.png)

---

## 🔒 Best Practices for Secure Buckets

### ✅ Enable Block Public Access

Unless explicitly needed.

---

### ✅ Use Least Privilege IAM

Never use:

```text
s3:*
```

for everyone.

---

### ✅ Enable Encryption

Recommended:

- SSE-S3
- SSE-KMS

---

### ✅ Enable Versioning

Helps recover deleted files.

---

### ✅ Enable Logging

Use:

- CloudTrail
- S3 Access Logs

---

### ✅ Use Lifecycle Policies

Automatically:

- archive old files
- delete unnecessary objects

---

## 🏦 Fintech Security Perspective

In fintech environments, S3 often stores:

- transaction logs
- KYC documents
- audit reports
- backups
- customer statements

That means:

> a single public bucket can become a massive compliance violation.

This can break:

- PCI-DSS
- SOC2
- ISO 27001
- RBI compliance

That’s why enterprises:

- heavily audit S3 permissions
- use automated compliance scanning
- continuously monitor bucket exposure

---

## 🔍 Tools Used to Audit S3 Security

### Popular Tools

| Tool         | Purpose                 |
| ------------ | ----------------------- |
| AWS Config   | Compliance monitoring   |
| Security Hub | Security findings       |
| CloudTrail   | Audit logging           |
| ScoutSuite   | Cloud auditing          |
| Prowler      | AWS security assessment |

---

## ☁️ Real Industry Practice

Most mature companies follow:

```text
Private by default
Public only when necessary
```

This is the golden rule of cloud storage security.

---

## 🎯 Final Thoughts

S3 looks simple initially.

But in real enterprise environments:

- permissions
- policies
- encryption
- compliance
- auditing

become extremely important.

A single misconfigured bucket can expose:

- millions of records
- customer identities
- internal infrastructure data

That’s why:

> understanding S3 security is one of the most important cloud skills.
