Day 18 - Infrastructure as Code (IaC) with Terraform

Modern cloud infrastructure is too complex to manage manually.

Imagine creating:

- 10 EC2 instances
- 5 VPCs
- 20 Security Groups
- 15 IAM Roles
- 3 Load Balancers
- Kubernetes Clusters

using only a cloud console.

It quickly becomes:

```text
Slow
Error-Prone
Difficult to Scale
Impossible to Audit
```

This is why Infrastructure as Code (IaC) became one of the most important practices in modern DevOps and Cloud Engineering.

---

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

---

## What is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is the practice of managing infrastructure through code instead of manually creating resources.

Instead of clicking buttons:

```text
AWS Console
      ↓
Create EC2
      ↓
Create Security Group
      ↓
Create VPC
```

You write:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

And infrastructure gets created automatically.

---

## Why Infrastructure as Code Matters

Before IaC, infrastructure management was painful.

Common problems included:

- Manual mistakes
- Configuration drift
- Poor documentation
- Difficult disaster recovery
- Inconsistent environments

Example:

```text
Developer Environment
        ↓
Works Perfectly
        ↓
Production Environment
        ↓
Different Configuration
        ↓
Application Fails
```

IaC solves this problem by making environments reproducible.

---

## Benefits of Infrastructure as Code

### 1. Consistency

Every environment is identical.

```text
Dev
 ↓
QA
 ↓
Staging
 ↓
Production
```

All built from the same code.

---

### 2. Version Control

Infrastructure becomes:

```text
Git Commit
Pull Request
Code Review
Rollback
Audit Trail
```

Infrastructure changes become trackable.

---

### 3. Automation

Entire environments can be created in minutes.

---

### 4. Disaster Recovery

If infrastructure is lost:

```text
Git Repository
        ↓
terraform apply
        ↓
Infrastructure Restored
```

---

### 5. Scalability

Large organizations can manage thousands of resources through code.

---

## Infrastructure as Code Market Growth

Infrastructure automation has become a standard practice.

Today IaC is used by:

- Cloud Engineers
- DevOps Engineers
- Platform Engineers
- SRE Teams
- Security Teams

Organizations running:

- AWS
- Azure
- GCP
- Kubernetes

almost always adopt some form of IaC.

---

## Types of Infrastructure as Code

### Declarative

You describe the desired state.

Example:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

Tool decides how to create it.

Examples:

- Terraform
- CloudFormation
- Bicep

---

### Imperative

You define step-by-step instructions.

Example:

```python
create_vpc()
create_subnet()
create_ec2()
```

Examples:

- Pulumi
- Custom automation scripts

---

## Popular Infrastructure as Code Tools

---

### 1. Terraform

Most popular multi-cloud IaC tool.

Created by:

HashiCorp

Supports:

- AWS
- Azure
- GCP
- Kubernetes
- VMware
- GitHub
- Hundreds of providers

---

### 2. AWS CloudFormation (CFT)

AWS-native IaC service.

Supports:

- VPC
- EC2
- IAM
- S3
- RDS
- Lambda

Example:

```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
```

---

### 3. Azure Bicep

Microsoft's modern IaC language.

Simplifies Azure Resource Manager templates.

Example:

```bicep
resource storage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: 'mystorage'
}
```

---

### 4. Pulumi

Modern Infrastructure as Code.

Uses programming languages:

- Python
- Go
- TypeScript
- C#
- Java

Example:

```python
import pulumi_aws as aws

bucket = aws.s3.Bucket("my-bucket")
```

---

![difference](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wh7x10lqv304gd1a8d8a.png)

---

## Why Terraform Dominates IaC

Terraform became the industry standard because:

```text
One Language
        ↓
Multiple Clouds
        ↓
Single Workflow
```

Engineers can manage:

- AWS
- Azure
- GCP
- Kubernetes

using one tool.

---

## Terraform Architecture

![architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jwxn309w8obxw2pwoek7.png)

---

## Terraform Basics

Understanding Terraform starts with four key concepts:

- Providers
- Resources
- Variables
- State

---

### Terraform Providers

Providers allow Terraform to communicate with platforms.

Examples:

```text
AWS Provider
Azure Provider
Google Provider
Kubernetes Provider
GitHub Provider
```

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Terraform now knows where to create resources.

---

### Terraform Resources

Resources are actual infrastructure components.

Examples:

```text
EC2 Instance
S3 Bucket
VPC
Security Group
IAM Role
```

Example:

```hcl
resource "aws_s3_bucket" "demo" {
  bucket = "my-demo-bucket"
}
```

Terraform will create:

```text
AWS S3 Bucket
```

---

### Terraform Variables

Variables make code reusable.

Without variables:

```hcl
instance_type = "t2.micro"
```

With variables:

```hcl
variable "instance_type" {}

instance_type = var.instance_type
```

Now different environments can use:

```text
Dev → t2.micro
QA → t3.small
Prod → t3.large
```

---

### Terraform State

Terraform keeps track of infrastructure using:

```text
terraform.tfstate
```

This file stores:

- Resource IDs
- Current state
- Dependency mapping

Terraform compares:

```text
Current State
        vs
Desired State
```

and calculates required changes.

---

## Terraform Workflow

### Step 1

Write Code

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

### Step 2

Initialize

```bash
terraform init
```

Downloads providers.

---

### Step 3

Validate

```bash
terraform validate
```

Checks syntax.

---

### Step 4

Preview

```bash
terraform plan
```

Shows changes before execution.

---

### Step 5

Apply

```bash
terraform apply
```

Creates infrastructure.

---

## Deep Terraform Example

Let's create a simple AWS infrastructure.

---

### Provider

```hcl
provider "aws" {
  region = "us-east-1"
}
```

---

### Variable

```hcl
variable "instance_type" {
  default = "t2.micro"
}
```

---

### Security Group

```hcl
resource "aws_security_group" "web_sg" {

  name = "web-sg"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

### EC2 Instance

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456"
  instance_type = var.instance_type

  vpc_security_group_ids = [
    aws_security_group.web_sg.id
  ]

  tags = {
    Name = "Terraform-Web"
  }
}
```

---

## What Happens Behind the Scenes?

![Behined the Scene](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xav04xp20stqk8vou4r2.png)

---

## Terraform File Structure

Typical project:

```text
terraform-project/

├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
└── providers.tf
```

---

## Best Practices

### Use Remote State

Store state in:

```text
S3
Azure Storage
GCS
Terraform Cloud
```

Never store production state locally.

---

### Use Modules

Avoid repeating code.

```hcl
module "vpc" {
  source = "./modules/vpc"
}
```

---

### Use Version Control

Infrastructure should always live in Git.

---

### Enable Code Reviews

Treat infrastructure like application code.

---

### Separate Environments

```text
Dev
QA
Staging
Production
```

should have separate state files.

---

## Infrastructure as Code in DevOps Pipeline

```text
Developer Pushes Terraform
          ↓
Pull Request
          ↓
Code Review
          ↓
terraform validate
          ↓
terraform plan
          ↓
Security Scan
          ↓
terraform apply
          ↓
Infrastructure Created
```

---

## Security Considerations

Never store:

```text
AWS Keys
Passwords
Tokens
Secrets
```

inside Terraform code.

Use:

- AWS Secrets Manager
- Azure Key Vault
- HashiCorp Vault

instead.

---

## Final Thoughts

Infrastructure as Code transformed how cloud infrastructure is managed.

Instead of:

```text
Manual Infrastructure
```

we now have:

```text
Version Controlled Infrastructure
```

Among all IaC tools:

- Terraform dominates multi-cloud environments
- CloudFormation is ideal for AWS-centric teams
- Bicep is excellent for Azure
- Pulumi is attractive for developers who prefer real programming languages

For anyone pursuing:

- DevOps
- Cloud Engineering
- Platform Engineering
- Site Reliability Engineering

Infrastructure as Code is no longer optional—it is a fundamental skill of modern cloud operations.
