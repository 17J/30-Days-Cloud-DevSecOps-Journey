Day 26 - Hashicorp Vault & Secrets Management



Modern applications depend on secrets.

Every application requires:

* Database Passwords
* API Keys
* SSH Keys
* TLS Certificates
* Cloud Credentials
* OAuth Tokens
* Service Account Keys

The biggest question is:

> Where should we store them securely?

Unfortunately many organizations still store secrets in:

```text
Git Repository
Docker Image
Application Config Files
Environment Variables
Shared Documents
Excel Sheets
```

This creates a massive security risk.

This is why Secret Management platforms like **HashiCorp Vault** became critical in modern cloud-native environments.

---


## 🔗 Resources

* ** Support the Journey on GitHub:
If you're following along, consider starring and forking the repo:**
    [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)


---

## What is a Secret?

A secret is any sensitive piece of information used to authenticate or authorize access.

Examples:

```text
Database Password
AWS Access Key
JWT Signing Key
API Token
TLS Certificate
Private Key
OAuth Secret
```

If a secret gets exposed:

```text
Attacker
      ↓
Application Access
      ↓
Database Access
      ↓
Infrastructure Compromise
```

---

## What is Secrets Management?

Secrets Management is the process of:

```text
Store
Protect
Rotate
Control
Audit
```

sensitive credentials securely.

A modern secrets management platform provides:

* Centralized storage
* Encryption
* Access control
* Secret rotation
* Audit logs
* Dynamic credentials

---

## Why Secrets Management Matters

Imagine this scenario:

```yaml
database:
  username: admin
  password: Password123
```

committed into GitHub.

Result:

```text
Developer Pushes Code
          ↓
GitHub Repository
          ↓
Credential Leak
          ↓
Database Breach
```

This happens more often than people realize.

---

## The Problem with Traditional Secret Storage

Many teams use:

```text
.env Files
Kubernetes Secrets
Configuration Files
Hardcoded Passwords
```

Problems:

* Difficult rotation
* No audit trail
* Poor access control
* Risk of accidental exposure
* Compliance failures

---

## What is HashiCorp Vault?

HashiCorp Vault is a centralized secrets management platform designed to securely store, access, and manage secrets.

Think of Vault as:

```text
Central Secret Bank
```

for your infrastructure and applications.

Instead of:

```text
Application
     ↓
Database Password
```

stored locally,

you use:

```text
Application
      ↓
Vault
      ↓
Database Credentials
```

---

## Why HashiCorp Created Vault

Modern infrastructure became increasingly complex.

Organizations adopted:

* Kubernetes
* Multi-cloud
* Microservices
* Containers
* CI/CD Pipelines

Suddenly there were thousands of secrets.

Example:

```text
50 Microservices
     ↓
20 Secrets Each
     ↓
1000 Secrets
```

Managing them manually became impossible.

Vault was created to solve this problem.

---

## Core Features of HashiCorp Vault

---

### 1. Centralized Secret Storage

All secrets stored in one location.

```text
Applications
       ↓
HashiCorp Vault
       ↓
Secrets
```

---

### 2. Encryption as a Service

Vault encrypts sensitive data.

```text
Plain Text
     ↓
Encryption
     ↓
Encrypted Secret
```

---

### 3. Dynamic Secrets

One of Vault's most powerful features.

Instead of:

```text
Static Password
```

Vault generates temporary credentials.

Example:

```text
Application
      ↓
Vault
      ↓
Temporary Database User
      ↓
Expires Automatically
```

---

### 4. Secret Rotation

Vault automatically rotates secrets.

Example:

```text
Old Password
      ↓
Vault Rotation
      ↓
New Password
```

No manual work required.

---

### 5. Audit Logging

Every secret access is logged.

Example:

```text
Who accessed?
When?
What secret?
From where?
```

Critical for compliance.

---

### 6. Fine-Grained Access Control

Not everyone should access every secret.

Vault provides:

```text
Policy-Based Access
```

Example:

```text
Developer
     ↓
Read Dev Secrets

Production Secrets
     ✗ Denied
```

---


![Image Full ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/idrg5p4laezgkzzdz8hn.png)

---

## Main Vault Components

---

### Vault Server

Core service responsible for:

* Authentication
* Authorization
* Secret storage
* Encryption

---

### Storage Backend

Stores encrypted secrets.

Examples:

```text
Integrated Storage (Raft)
Consul
AWS DynamoDB
PostgreSQL
```

---

### Authentication Methods

Vault supports:

* Userpass
* LDAP
* GitHub
* Kubernetes
* AWS IAM
* Azure AD
* OIDC

Example:

```text
Developer
     ↓
GitHub Login
     ↓
Vault
```

---

### Policies

Vault policies define access permissions.

Example:

```hcl
path "secret/data/dev/*" {
  capabilities = ["read"]
}
```

Meaning:

```text
Can read dev secrets only
```

---

### What are Secrets Engines?

Secrets Engines are plugins that generate or store secrets.

Vault ships with many.

---

### KV Secrets Engine

Most common.

Stores:

```text
Username
Password
API Keys
Tokens
```

Example:

```bash
vault kv put secret/app \
username=admin \
password=secret123
```

---

### Database Secrets Engine

Creates temporary database users.

Example:

```text
Application
      ↓
Vault
      ↓
Temporary PostgreSQL User
```

Automatically expires later.

---

### PKI Secrets Engine

Issues certificates dynamically.

Example:

```text
Vault
      ↓
TLS Certificate
```

instead of manually creating certificates.

---

## AWS Secrets Engine

Generates temporary AWS credentials.

Example:

```text
Application
      ↓
Vault
      ↓
AWS IAM Credentials
```

---

## Dynamic Secrets vs Static Secrets

### Static Secret

```text
password123
```

Exists forever.

---

### Dynamic Secret

```text
Generated
     ↓
Used
     ↓
Automatically Expired
```

Much safer.

---

## Why Dynamic Secrets Are Important

Static credentials are often stolen.

Dynamic credentials reduce risk because:

```text
Credential Expires
       ↓
Attack Window Reduced
```



---


![Second Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7noiosc59hqnl2uh7vu2.png)
---

## Installing Vault in Development Environment

Development mode is useful for learning.

---

### Run Vault Using Docker

```bash
docker run \
--cap-add=IPC_LOCK \
-e VAULT_DEV_ROOT_TOKEN_ID=root \
-p 8200:8200 \
hashicorp/vault
```

Access:

```text
http://localhost:8200
```

Login:

```text
Token: root
```

---

### Verify Vault

```bash
vault status
```

Expected output:

```text
Initialized: true
Sealed: false
```

---

### Store First Secret

```bash
vault kv put secret/app \
username=admin \
password=password123
```

Retrieve:

```bash
vault kv get secret/app
```

---

### Installing Vault in Kubernetes

Most production environments run Vault inside Kubernetes.

---

### Add Helm Repository

```bash
helm repo add hashicorp \
https://helm.releases.hashicorp.com
```

---

### Update Repository

```bash
helm repo update
```

---

### Install Vault

```bash
helm install vault hashicorp/vault
```

Verify:

```bash
kubectl get pods
```

---

### Enable UI

```yaml
server:
  ui:
    enabled: true
```

---

### Production Vault Architecture

Recommended architecture:

```text
Load Balancer
       ↓
Vault Cluster
       ↓
Raft Storage
```

Multiple replicas:

```text
Vault-1
Vault-2
Vault-3
```

for high availability.

---

### Vault Auto-Unseal

Without Auto-Unseal:

```text
Vault Restart
      ↓
Manual Unseal Required
```

Production clusters use:

* AWS KMS
* Azure Key Vault
* GCP KMS

for automatic unsealing.

---

### Vault + Kubernetes Integration

Vault can inject secrets directly into Pods.

Traditional:

```yaml
env:
  DB_PASSWORD: password123
```

Vault:

```text
Pod
     ↓
Vault Agent
     ↓
Secret Injection
```

No hardcoded secrets.

---

### Vault Agent Injector

Automatically injects secrets into Pods.

```text
Application Pod
      ↓
Vault Sidecar
      ↓
Secret Available
```

without storing secrets in Git.

---

### Vault in CI/CD Pipelines

Modern CI/CD:

```text
GitHub Actions
      ↓
Vault Authentication
      ↓
Temporary Secrets
      ↓
Deployment
```

Benefits:

* No hardcoded credentials
* Automatic rotation
* Auditability

---

## Vault Security Best Practices

---

### Enable TLS

Never expose Vault without HTTPS.

---

### Use Auto-Unseal

Avoid manual operations.

---

### Use Least Privilege Policies

Grant minimum access.

---

### Enable Audit Logs

Track every access.

---

### Use Dynamic Secrets

Avoid static passwords.

---

### Integrate with Identity Provider

Examples:

```text
Azure AD
Okta
GitHub
LDAP
```

---

## Common Use Cases

### Kubernetes Secrets Management

```text
Pods
 ↓
Vault
 ↓
Secrets
```

---

### Database Credentials

```text
Application
 ↓
Vault
 ↓
Temporary PostgreSQL User
```

---

### Cloud Credentials

```text
Application
 ↓
Vault
 ↓
AWS IAM Credentials
```

---

### PKI Certificates

```text
Vault
 ↓
Generate TLS Certificates
```

---

### Enterprise Vault Architecture

```text
Developers
       ↓
Applications
       ↓
Vault Cluster
       ↓
Policies
       ↓
Secrets Engines
       ↓
Database / Cloud / Certificates
```



## Final Thoughts

Modern infrastructure depends on secrets.

As organizations adopt:

* Kubernetes
* Multi-cloud
* GitOps
* Platform Engineering
* DevSecOps

traditional secret management approaches are no longer sufficient.

HashiCorp Vault solves this problem by providing:

```text
Centralized Storage
Dynamic Secrets
Secret Rotation
Audit Logging
Encryption
Fine-Grained Access Control
```

For small AWS-only workloads, AWS Secrets Manager may be enough.

For Azure-only environments, Azure Key Vault works well.

But for organizations needing:

```text
Multi-Cloud
Kubernetes
Hybrid Cloud
Advanced Security
```

HashiCorp Vault remains one of the most powerful and widely adopted secrets management platforms available today.
