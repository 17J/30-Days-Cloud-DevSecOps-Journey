Day 24 - Kubernetes Fundamentals & Security

In Current Time applications are no longer deployed as a single server.

Today organizations run:

* Microservices
* Containers
* Kubernetes
* Service Mesh
* GitOps
* Cloud Native Platforms

At the center of this transformation is **Kubernetes**.

Kubernetes has become the de-facto standard for container orchestration.

According to CNCF surveys, Kubernetes adoption continues to grow rapidly across enterprises, startups, cloud providers, and platform engineering teams.

But Kubernetes is much more than deploying containers.

A production Kubernetes engineer must understand:

* Pods
* Services
* ConfigMaps
* Secrets
* Deployments
* RBAC
* Network Policies
* Pod Security
* Runtime Security
* Policy Enforcement

---


## 🔗 Resources

* ** Support the Journey on GitHub:
If you're following along, consider starring and forking the repo:**
    [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)



---

## What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform originally developed by Google and now maintained by the CNCF.

Its purpose is to automate:

```text
Container Deployment
        ↓
Scaling
        ↓
Networking
        ↓
Self-Healing
        ↓
Availability
```

Instead of manually managing containers:

```text
Docker Container
Docker Container
Docker Container
```

Kubernetes manages them automatically.



---

## Why Kubernetes?

Before Kubernetes:

```text
Application
      ↓
Virtual Machine
      ↓
Manual Scaling
      ↓
Manual Recovery
```

Problems:

* Downtime
* Scaling issues
* Operational complexity
* Resource wastage

Kubernetes solves these problems.

Benefits:

* Self-healing
* Auto-scaling
* Service discovery
* Rolling updates
* Multi-cloud portability
* High availability

---




![k8s architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dk3hk912bq9nwm2mr4i6.webp)

Control Plane manages the cluster.

Worker Nodes run workloads.

---

## Understanding Pods

Pods are the smallest deployable unit in Kubernetes.

Think of a Pod as:

```text
Container Wrapper
```

Example:

```text
Pod
 └─ NGINX Container
```

A Pod can also contain multiple containers.

Example:

```text
Pod
 ├─ Application Container
 └─ Logging Sidecar
```

---

## NGINX Pod Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx:latest
```

Create Pod:

```bash
kubectl apply -f pod.yaml
```

---

## Why Pods Matter

Pods provide:

* Shared network
* Shared storage
* Shared lifecycle

Containers inside a Pod communicate using:

```text
localhost
```

---

## What is a Deployment?

Managing Pods directly is not recommended.

Instead use Deployments.

Deployment provides:

* Scaling
* Rollbacks
* Rolling Updates
* Self-healing

---

### Deployment Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

---

### Deployment Benefits

```text
Pod Crashes
      ↓
Deployment Detects Failure
      ↓
New Pod Created
```

This is Kubernetes self-healing.

---

### What is a Service?

Pods are ephemeral.

Their IP addresses change.

Service provides a stable endpoint.

Without Service:

```text
Client
   ↓
Pod IP
```

Pod restart breaks communication.

With Service:

```text
Client
   ↓
Service
   ↓
Pods
```

Applications always connect to Service.

---

## Service Types


![Service Types](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6hu211yb8x5159ogi6mn.webp)



### ClusterIP

Default internal service.

```text
Inside Cluster Only
```

---

### NodePort

Exposes service through node port.

```text
NodeIP:30080
```

---

### LoadBalancer

Creates cloud load balancer.

```text
AWS ELB
Azure LB
GCP LB
```

---

### ExternalName

Maps service to external DNS.

---

### Service Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80

  type: ClusterIP
```

---

## What is ConfigMap?

Applications need configuration.

Examples:

```text
Database Host
API URL
Feature Flags
```

Hardcoding these values is bad practice.

ConfigMaps store non-sensitive configuration.

---

## ConfigMap Example

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_ENV: production
  LOG_LEVEL: info
```

Use inside Pod:

```yaml
envFrom:
- configMapRef:
    name: app-config
```

---

## What are Kubernetes Secrets?

Secrets store sensitive information.

Examples:

```text
Database Password
API Keys
Tokens
Certificates
```

Unlike ConfigMaps, Secrets are intended for sensitive data.

---

### Secret Example

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: db-secret

type: Opaque

stringData:
  username: admin
  password: Password123
```

---


![First Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ay5quqkjl2vfda4op0wq.png)


---

## Important Security Warning

Secrets are Base64 encoded.

They are NOT automatically encrypted.

Bad assumption:

```text
Base64 = Encryption
```

Wrong.

Production clusters should enable:

```text
Encryption at Rest
```

for Secrets.

---

## Kubernetes Security Fundamentals

Many organizations secure:

```text
Cloud
Network
Applications
```

but forget Kubernetes security.

A compromised cluster can expose:

* Customer data
* Internal services
* Cloud credentials
* Secrets

Kubernetes security must be built in from the beginning.

---

### What is RBAC?

RBAC stands for:

```text
Role-Based Access Control
```

RBAC controls:

```text
Who
Can Do What
Inside Kubernetes
```

---

## RBAC Components

### Role

Defines permissions.

Example:

```text
Read Pods
Read Services
```

---

### RoleBinding

Assigns Role to User.

```text
User
   ↓
RoleBinding
   ↓
Role
```

---

### RBAC Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  name: pod-reader

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list","watch"]
```

---


![RBAC Arctecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hpufesbhqx8ylr2z25j6.png)

---



### Why RBAC Matters

Without RBAC:

```text
Developer
     ↓
Cluster Admin
```

Huge security risk.

Use least privilege.

---

## What are Network Policies?

By default:

```text
Pod A
      ↓
Pod B
```

Communication is often allowed.

This violates Zero Trust principles.

---

## Network Policy Purpose

Control:

```text
Pod-to-Pod Traffic
Namespace Traffic
Ingress
Egress
```

---

## Example Policy

Allow traffic only from frontend Pods.

```yaml
kind: NetworkPolicy
```

Result:

```text
Frontend
    ↓
Backend

Other Pods
    ✗ Blocked
```

---

## Pod Security

Pods themselves must be secured.

Bad Pod:

```yaml
privileged: true
runAsUser: 0
```

This means:

```text
Root Access
```

inside container.

Huge risk.

---

## Secure Pod Example

```yaml
securityContext:

  runAsNonRoot: true

  allowPrivilegeEscalation: false

  readOnlyRootFilesystem: true
```

Benefits:

* Reduced attack surface
* Better compliance
* Least privilege

---

## Secrets Management Best Practices

Never:

```text
Store Passwords in Git
Hardcode API Keys
Commit Secrets to Repository
```

Use:

* Kubernetes Secrets
* External Secrets Operator
* HashiCorp Vault
* AWS Secrets Manager
* Azure Key Vault

---

## Runtime Security with Falco

Kubernetes security doesn't stop at deployment.

You must monitor runtime behavior.

This is where Falco comes in.

---

## What is Falco?

Falco is a CNCF runtime security tool.

It detects suspicious behavior inside Kubernetes.



![Falco Detection](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wjl81pagtay9gmezbonz.png)



Example:

```text
Container Starts Shell
```

Falco Alert:

```text
Unexpected Shell Execution
```

---

## Falco Architecture

```text
Container Activity
        ↓
Falco Rules Engine
        ↓
Alert Generated
```

---

## Falco Detection Examples

Detect:

* Reverse shell
* Privilege escalation
* Crypto miners
* Suspicious processes
* Unexpected file access
* Sensitive mounts

---

## Example Falco Alert

```text
Terminal shell in container
```

This could indicate compromise.

---

## Policy Enforcement with Kyverno

Security should be automated.

Developers make mistakes.

Kyverno prevents insecure workloads from being deployed.

---

## What is Kyverno?

Kyverno is a Kubernetes-native policy engine.

It validates Kubernetes resources before deployment.

---

## Example Use Case

Block privileged containers.

Bad Deployment:

```yaml
privileged: true
```

Kyverno:

```text
Deployment Rejected
```

---

### Kyverno Benefits

Enforce:

* Non-root containers
* Resource limits
* Approved registries
* Label requirements
* Security standards

Automatically.

---

### Example Security Policy

Require:

```text
runAsNonRoot=true
```

Any Pod violating policy:

```text
Rejected
```

before reaching production.

---

## Production Kubernetes Security Checklist

### Identity Security

* RBAC
* Least Privilege
* Service Accounts

---

### Network Security

* Network Policies
* Service Mesh
* mTLS

---

### Secrets Security

* Vault
* Secrets Manager
* Encryption at Rest

---

### Pod Security

* Non-root containers
* Read-only filesystems
* Drop Linux capabilities

---

### Runtime Security

* Falco
* Monitoring
* Audit Logs

---

### Policy Security

* Kyverno
* OPA Gatekeeper

---

## Modern Secure Kubernetes Architecture

```text
Developer
      ↓
Git Repository
      ↓
CI Pipeline
      ↓
Container Scan
      ↓
Kyverno Validation
      ↓
Kubernetes Cluster
      ↓
Network Policies
      ↓
RBAC
      ↓
Falco Runtime Monitoring
      ↓
Alerts
```

---

## Final Thoughts

Learning Kubernetes means more than learning Pods and Deployments.

A production Kubernetes engineer must understand both:

```text
Workload Management
        +
Security
```

Core Fundamentals:

* Pods
* Deployments
* Services
* ConfigMaps
* Secrets

Core Security:

* RBAC
* Network Policies
* Pod Security
* Secrets Management
* Falco
* Kyverno

Organizations that treat Kubernetes security as an afterthought often face misconfigurations, exposed workloads, and compliance failures.

The most successful Kubernetes platforms combine:

```text
Automation
Security
Observability
Governance
```