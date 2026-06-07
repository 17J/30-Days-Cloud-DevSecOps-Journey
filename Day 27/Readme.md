Day 27 - Container Security and Runtime Security

Containers have transformed modern software delivery.

Today almost every cloud-native platform uses:

- Docker
- Kubernetes
- Amazon EKS
- Azure AKS
- Google GKE
- OpenShift

Containers allow organizations to deploy applications faster, scale efficiently, and maintain consistency across environments.

However, containers also introduce new security challenges.

Many teams focus only on:

```text
Container Build
       ↓
Image Scan
       ↓
Deploy
```

and assume they are secure.

Unfortunately, security doesn't stop after deployment.

Modern attackers target:

- Container runtimes
- Kubernetes clusters
- Misconfigured containers
- Exposed secrets
- Privileged workloads

This is where **Container Security** and **Runtime Security** become critical.

---

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

---

## Why Container Security Matters

Modern applications are increasingly deployed as containers.

Example:

```text
Application
      ↓
Container
      ↓
Kubernetes
      ↓
Production
```

If an attacker compromises a container:

```text
Container
      ↓
Host Node
      ↓
Kubernetes Cluster
      ↓
Cloud Environment
```

The impact can be enormous.

---

## What is Container Security?

Container Security is the practice of protecting containers throughout their entire lifecycle.

This includes:

```text
Build Phase
      ↓
Registry
      ↓
Deployment
      ↓
Runtime
```

Container security covers:

- Secure image creation
- Vulnerability scanning
- Image signing
- Access control
- Runtime protection
- Compliance monitoring

---

## Container Lifecycle Security

A secure container journey looks like:

```text
Developer Writes Code
          ↓
Build Container
          ↓
Image Scan
          ↓
Push to Registry
          ↓
Deploy to Kubernetes
          ↓
Runtime Monitoring
          ↓
Threat Detection
```

Security should exist at every stage.

---

## Why Traditional Security Is Not Enough

Traditional security tools focus on:

```text
Servers
Virtual Machines
Networks
```

Containers introduce:

```text
Ephemeral Workloads
Shared Kernel
Microservices
Dynamic Scaling
```

which require specialized security approaches.

---

## Understanding Container Architecture

A container consists of:

![main image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qrjltp5is2rz0qcp6g4o.png)

Unlike virtual machines:

```text
Multiple Containers
      ↓
Shared Kernel
```

This creates unique attack vectors.

---

## Common Container Security Risks

---

### 1. Vulnerable Base Images

Many developers pull images directly from public registries.

Example:

```dockerfile
FROM ubuntu:latest
```

Problem:

```text
Unknown Vulnerabilities
Unknown Dependencies
Unknown Configuration
```

---

### 2. Running Containers as Root

Dangerous example:

```dockerfile
USER root
```

Risk:

```text
Container Escape
Privilege Escalation
Host Compromise
```

---

### 3. Hardcoded Secrets

Bad practice:

```yaml
DATABASE_PASSWORD=password123
```

inside:

```text
Dockerfile
Source Code
Environment Variables
```

---

### 4. Excessive Linux Capabilities

Containers often receive permissions they don't need.

Example:

```text
NET_ADMIN
SYS_ADMIN
```

These capabilities increase attack surface.

---

### 5. Untrusted Container Images

Downloading random images from Docker Hub can introduce:

- Malware
- Crypto miners
- Backdoors

---

## What is Runtime Security?

Container security before deployment is important.

Runtime security focuses on what happens after deployment.

Runtime Security means:

```text
Monitor
Detect
Respond
```

to suspicious container behavior while applications are running.

---

## Why Runtime Security Matters

Even a perfectly scanned image can be compromised.

Example:

```text
Image Clean
      ↓
Application Vulnerability
      ↓
Remote Code Execution
      ↓
Runtime Attack
```

Image scanning cannot detect runtime behavior.

---

## Runtime Threat Example

```text
Container Running Normally
          ↓
Attacker Exploits Vulnerability
          ↓
Shell Spawned
          ↓
Sensitive Data Accessed
```

Image scans won't catch this.

Runtime security will.

---

## Understanding Runtime Threats

---

## Reverse Shells

One of the most common attacks.

Example:

```text
Container
      ↓
Attacker
      ↓
Reverse Shell
```

Now the attacker has interactive access.

---

## Cryptocurrency Mining

Compromised containers are often used for:

```text
Cryptocurrency Mining
```

Symptoms:

```text
High CPU Usage
Unexpected Processes
Resource Exhaustion
```

---

## Privilege Escalation

Attackers attempt:

```text
Container
      ↓
Root Access
      ↓
Host Access
```

to escape the container boundary.

---

## Suspicious Process Execution

Example:

```text
Nginx Container
      ↓
Unexpected Bash Process
```

This should trigger an alert.

---

## File Tampering

Attackers may modify:

```text
Application Files
System Files
Configurations
```

inside running containers.

---

## Container Escape

One of the most dangerous attacks.

Goal:

```text
Container
      ↓
Host Node
```

If successful:

```text
Entire Kubernetes Node Compromised
```

---

## What is Container Hardening?

Container Hardening reduces the attack surface before deployment.

Think of it as:

```text
Removing Everything Unnecessary
```

from the container.

---

## Why Container Hardening?

Default containers often include:

```text
Extra Packages
Unused Tools
Shells
Package Managers
```

All of these increase risk.

---

## Container Hardening Best Practices

---

### Use Minimal Base Images

Bad:

```dockerfile
FROM ubuntu
```

Better:

```dockerfile
FROM alpine
```

Even better:

```dockerfile
FROM gcr.io/distroless/static
```

Benefits:

```text
Smaller Images
Fewer Vulnerabilities
Reduced Attack Surface
```

---

### Run as Non-Root

Bad:

```dockerfile
USER root
```

Good:

```dockerfile
RUN adduser appuser
USER appuser
```

Benefits:

```text
Reduced Privilege Escalation Risk
```

---

### Remove Unnecessary Packages

Avoid installing:

```text
curl
wget
vim
bash
gcc
```

unless absolutely required.

---

### Use Read-Only File Systems

Example:

```yaml
securityContext:
  readOnlyRootFilesystem: true
```

Benefits:

```text
Prevents File Modification
```

---

### Drop Linux Capabilities

Example:

```yaml
capabilities:
  drop:
    - ALL
```

Grant only required capabilities.

---

### Set Resource Limits

Example:

```yaml
resources:
  limits:
    cpu: "500m"
    memory: "512Mi"
```

Protects against:

```text
DoS
Crypto Mining
Resource Abuse
```

---

## What is Image Scanning?

Image scanning analyzes container images for:

- Vulnerabilities
- Misconfigurations
- Secrets
- Malware

before deployment.

---

## Why Image Scanning Matters

Applications often contain:

```text
Open Source Libraries
Operating System Packages
Framework Dependencies
```

Some may have known vulnerabilities.

---

## Example Vulnerability

```text
Application
      ↓
Old Log4j Version
      ↓
Remote Code Execution
```

This could compromise the entire environment.

---

## Image Scanning Workflow

```text
Developer Builds Image
          ↓
Image Scanner
          ↓
Vulnerability Report
          ↓
Fix Issues
          ↓
Deploy
```

---

## Popular Image Scanning Tools

---

## Trivy

One of the most popular scanners.

Features:

- Image scanning
- Filesystem scanning
- IaC scanning
- Kubernetes scanning

Example:

```bash
trivy image nginx:latest
```

---

## Grype

Container vulnerability scanner.

Benefits:

```text
Fast
Open Source
Accurate
```

---

## Snyk Container

Enterprise-focused platform.

Features:

- Vulnerability detection
- Fix recommendations
- Continuous monitoring

---

## Clair

Open-source container scanner.

Often integrated into registries.

---

### Example Trivy Output

```text
Critical: 2
High: 8
Medium: 14
Low: 20
```

Organizations often block deployments if:

```text
Critical > 0
```

---

## Runtime Security Tools

Image scanning alone is not enough.

You need runtime visibility.

---

### Falco

One of the most popular runtime security tools.

Created by:

```text
Sysdig
```

Now a CNCF project.

---

### How Falco Works

```text
Container Activity
        ↓
Kernel Events
        ↓
Falco Rules
        ↓
Alert
```

---

### Example Falco Detection

Detect:

```text
Shell Spawned in Container
```

Alert:

```text
Unexpected Shell Execution
```

---

### Falco Use Cases

Detect:

- Reverse shells
- Privilege escalation
- Crypto miners
- Suspicious file access
- Container escape attempts

---

### Tetragon

Modern eBPF-based runtime security platform.

Developed by:

Isovalent

Features:

```text
Process Monitoring
Network Monitoring
Security Enforcement
```

---

### Sysdig Secure

Enterprise runtime security platform.

Provides:

- Runtime detection
- Compliance
- Threat intelligence

---

## Runtime Security in Kubernetes

A secure Kubernetes deployment looks like:

```text
Pod
 ↓
Security Context
 ↓
Network Policy
 ↓
Runtime Monitoring
 ↓
Alerting
```

Multiple security layers are required.

---

## Secure Container Pipeline

Modern DevSecOps pipeline:

```text
Developer Commit
        ↓
SAST
        ↓
SCA
        ↓
Container Build
        ↓
Image Scan
        ↓
Registry
        ↓
Kubernetes Deployment
        ↓
Runtime Monitoring
        ↓
Threat Detection
```

---

![demo image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/auu74x7dwjbejco3vtte.png)

---

## Container Security Best Practices

### Use Trusted Images

Only pull images from approved registries.

---

### Scan Every Image

Integrate scanners into CI/CD.

Example:

```text
Trivy
Grype
Snyk
```

---

### Run as Non-Root

Avoid privileged containers.

---

### Use Read-Only Filesystems

Prevent file tampering.

---

### Sign Container Images

Use:

```text
Cosign
Notary
```

to verify image authenticity.

---

### Enforce Kubernetes Policies

Use:

```text
Kyverno
OPA Gatekeeper
```

to prevent insecure deployments.

---

### Monitor Runtime Activity

Use:

```text
Falco
Tetragon
Sysdig
```

for continuous visibility.

---

## Real-World Attack Scenario

```text
Vulnerable Application
        ↓
Remote Code Execution
        ↓
Shell Spawned
        ↓
Credential Theft
        ↓
Cloud Access
        ↓
Infrastructure Compromise
```

Without runtime security:

```text
Attack Goes Undetected
```

With runtime security:

```text
Falco Alert
      ↓
SOC Investigation
      ↓
Threat Contained
```

---

## Enterprise Container Security Architecture

```text
Developer
      ↓
Git Repository
      ↓
CI/CD Pipeline
      ↓
Trivy Scan
      ↓
Container Registry
      ↓
Kubernetes Cluster
      ↓
Falco Runtime Monitoring
      ↓
SIEM
      ↓
Security Team
```

---

## Final Thoughts

Container security is no longer optional.

As organizations adopt:

- Kubernetes
- Microservices
- Cloud Native Platforms
- DevSecOpscode

they must secure containers at every stage.

A mature security strategy includes:

```text
Secure Images
      +
Container Hardening
      +
Image Scanning
      +
Runtime Monitoring
      +
Threat Detection
```

Because the most dangerous attacks often happen **after deployment**, not before.

The strongest container security programs combine preventive controls, runtime visibility, and continuous monitoring to protect modern cloud-native environments.
