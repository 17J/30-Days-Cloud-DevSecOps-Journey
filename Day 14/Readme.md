Day 14 - Kubernetes and AWS EKS Overview

Modern infrastructure is no longer about running applications on a single server.

Today's applications demand **scalability**, **high availability**, **self-healing**, **automation**, **cloud portability**, and **zero-downtime deployments**.

This is exactly where Kubernetes changed everything.

From early-stage startups to global enterprises like Google, Netflix, Spotify, and Airbnb — Kubernetes has become the universal standard for container orchestration. In this deep-dive guide, we'll cover everything you need to understand K8s from the ground up.

---

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

- **AWS Command Sheet:**
  [https://aws-command.vercel.app/](https://aws-command.vercel.app/)

---

## What is Kubernetes?

**Kubernetes** (commonly abbreviated as **K8s**) is an open-source container orchestration platform designed to automate the deployment, scaling, networking, load balancing, self-healing, and rolling updates of containerized applications.

> Think of Kubernetes as the **Operating System for Containers**.

Originally developed internally at Google and open-sourced in 2014, it is now actively maintained by the **Cloud Native Computing Foundation (CNCF)**.

```
If Docker runs containers →
Kubernetes manages containers at massive scale.
```

---

## Why Kubernetes Exists

Before Kubernetes, engineering teams faced a cascading set of infrastructure problems:

- Manual server provisioning and management
- Service downtime during deployments
- Unpredictable and inconsistent scaling
- Infrastructure drift across environments
- Slow, manual recovery from failures
- Heavy vendor lock-in

To illustrate the scale problem:

| Container Count   | Management Difficulty                     |
| ----------------- | ----------------------------------------- |
| 5 containers      | Manageable manually                       |
| 500 containers    | Difficult                                 |
| 50,000 containers | Practically impossible without automation |

Kubernetes was built to solve this by **automating infrastructure operations at scale**.

---

## Why Companies Use Kubernetes

### ⚡ Auto Scaling

Kubernetes can automatically scale applications based on CPU usage, memory pressure, traffic load, and custom application metrics — without human intervention.

### 🩹 Self-Heal

![self healing](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xf14elbrc4iaubs8q5gc.png)

No on-call engineer required for routine container failures.

### 🌍 Multi-Cloud Flexibility

Kubernetes runs identically on:

- **AWS** (EKS)
- **Azure** (AKS)
- **Google Cloud** (GKE)
- **On-premises** bare metal
- **Hybrid cloud** environments

This architecture-level portability eliminates vendor lock-in at the infrastructure layer.

### 🔄 Rolling Updates

![Image zero downtime](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vc5ufpf2rjddd46ili9f.png)

### 📦 Infrastructure Standardization

Teams ship applications consistently across development, staging, and production using identical YAML manifests.

---

## Kubernetes Market Statistics (2026)

The adoption curve has been steep and consistent:

- **Over 96%** of organizations are actively using or evaluating Kubernetes
- **More than 7 million developers** work within Kubernetes ecosystems
- Kubernetes is the **de facto standard** for cloud-native orchestration
- The majority of Fortune 500 companies run Kubernetes in production

Major production users include Google, Amazon, Microsoft, Spotify, Adobe, Shopify, and OpenAI.

---

## Alternatives to Kubernetes

Despite Kubernetes' dominance, alternatives serve specific use cases:

| Platform         | Description                                             |
| ---------------- | ------------------------------------------------------- |
| **Docker Swarm** | Lightweight orchestration built into Docker             |
| **Nomad**        | Simple, flexible orchestrator by HashiCorp              |
| **Apache Mesos** | Large-scale cluster manager with broad workload support |
| **OpenShift**    | Enterprise Kubernetes distribution by Red Hat           |
| **Rancher**      | Multi-cluster Kubernetes management platform            |
| **Amazon ECS**   | AWS-native container orchestration service              |

---

## Kubernetes

At the highest level, a Kubernetes cluster is divided into two distinct planes:

![kubernetes architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6pa0h71kqed0jt8kicbk.png)

The **Control Plane** makes global decisions about the cluster. The **Worker Nodes** are where your actual application containers run.

---

## Control Plane Components

### 1. API Server

The API Server is the **single entry point** for all cluster operations. Every `kubectl` command, every internal controller action, every webhook — everything communicates through the API Server.

```bash
kubectl get pods
# kubectl → API Server → etcd / Scheduler / Controllers
```

### 2. etcd

A distributed key-value store that serves as Kubernetes' **persistent memory**. It stores:

- Complete cluster state
- All configurations
- Secrets
- Networking information

> ⚠️ etcd is the most critical component in your cluster. Its availability determines cluster availability.

### 3. Scheduler

The Scheduler is responsible for **placing pods on nodes**. It evaluates available nodes based on resource requirements, affinity rules, taints and tolerations, and custom policies.

![Schediling flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z0dolrqxscxe98qrvrpv.png)

### 4. Controller Manager

The Controller Manager runs a set of **reconciliation loops** that continuously compare the desired state against the actual state of the cluster — and act to close any gap.

```
Desired replicas = 3
Current running  = 2
         ↓
Controller creates 1 additional pod
```

### 5. Cloud Controller Manager

Integrates Kubernetes with underlying cloud provider APIs. On AWS, this manages:

- Elastic Load Balancers
- EBS storage volumes
- VPC networking
- EC2 node registration

---

## Worker Node Components

### 1. Kubelet

An agent that runs on **every node**. The Kubelet watches the API Server for pod assignments, starts and stops containers via the container runtime, and continuously reports node and pod health back to the control plane.

### 2. Kube Proxy

Maintains **network rules** on nodes. It handles service discovery, traffic routing, and load balancing across pod endpoints using `iptables` or `IPVS`.

### 3. Container Runtime (CRI)

The software responsible for **actually running containers**. Kubernetes abstracts this through the Container Runtime Interface (CRI).

Popular runtimes:

- **containerd** (default in modern clusters)
- **CRI-O** (lightweight alternative)
- **Docker Engine** (legacy; deprecated as a direct runtime)

---

## What is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes. It represents one or more tightly coupled containers that share:

- The same network namespace (IP address + ports)
- The same storage volumes
- The same lifecycle

> In practice, most Pods contain a single application container. Multi-container Pods are used for sidecar patterns (logging agents, service mesh proxies, etc.).

### Pod Manifest Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.25
      ports:
        - containerPort: 80
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "500m"
          memory: "256Mi"
```

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
```

> ⚠️ Running bare Pods in production is discouraged. Pods are ephemeral — if a node fails, the Pod is not rescheduled. Use Deployments instead.

---

## What is a Deployment?

A **Deployment** is the standard way to run stateless applications in Kubernetes. It manages Pods declaratively and provides:

- Automatic **scaling**
- **Rolling updates** with configurable strategies
- Automatic **rollback** on failure
- **Self-healing** via ReplicaSet management

### Deployment Manifest Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.25
          ports:
            - containerPort: 80
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
```

```bash
kubectl apply -f deployment.yaml
kubectl rollout status deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment  # rollback
```

---

## Exposing Applications Using Services

Pods are **ephemeral** — they get new IP addresses when rescheduled. A **Service** provides a stable network endpoint that abstracts over a dynamic set of Pods.

### Service Types

![services](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2cwdhlnkdbkeryyakncc.png)

| Type           | Description                           |
| -------------- | ------------------------------------- |
| `ClusterIP`    | Internal-only; default type           |
| `NodePort`     | Exposes on a static port on each node |
| `LoadBalancer` | Provisions a cloud load balancer      |
| `ExternalName` | DNS alias to an external service      |

### Service Manifest Example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

---

## Full Deployment Flow

![Image deployment flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pt2tp7foixpiqlrt0ujk.png)

---

## Controllers and ReplicaSets

### Built-in Controllers

| Controller                 | Purpose                                            |
| -------------------------- | -------------------------------------------------- |
| **Deployment Controller**  | Manages Deployments and their rolling updates      |
| **ReplicaSet Controller**  | Ensures the desired number of Pod replicas         |
| **Node Controller**        | Monitors node availability and health              |
| **Job Controller**         | Manages one-off batch workloads                    |
| **CronJob Controller**     | Manages time-scheduled Jobs                        |
| **StatefulSet Controller** | Manages stateful applications with ordered scaling |
| **DaemonSet Controller**   | Ensures a Pod runs on every (or selected) node     |

### ReplicaSet Behavior

```
Desired replicas = 5
Current running  = 4
         ↓
ReplicaSet controller detects drift
         ↓
One new Pod scheduled immediately
```

> In practice, you define a Deployment — Kubernetes automatically creates and manages the underlying ReplicaSet.

---

## Kubernetes Networking

Every Pod in a Kubernetes cluster:

- Gets a **unique IP address**
- Can communicate with **any other Pod** without NAT (flat network model)
- Is reachable via **internal DNS** through Services

### Core Networking Concepts

- **Pod-to-Pod**: Direct via the CNI plugin (Calico, Flannel, Cilium, etc.)
- **Pod-to-Service**: Via `kube-proxy` iptables/IPVS rules
- **External-to-Service**: Via `LoadBalancer` or `Ingress` controllers
- **DNS**: CoreDNS resolves service names to ClusterIP addresses automatically

```
my-service.my-namespace.svc.cluster.local
```

---

## Kubernetes Security Basics

| Concept                    | Purpose                                            |
| -------------------------- | -------------------------------------------------- |
| **RBAC**                   | Role-based access control for API operations       |
| **Network Policies**       | Firewall rules between Pods and namespaces         |
| **Secrets**                | Encrypted storage for sensitive configuration      |
| **Service Accounts**       | Identity for Pods to authenticate to the API       |
| **Pod Security Standards** | Policies restricting privileged container behavior |
| **Admission Controllers**  | Intercept and validate/mutate API requests         |

---

## Kubernetes Namespaces

Namespaces provide **logical isolation** within a single cluster. Resources in one namespace are invisible to workloads in another by default (subject to Network Policies).

```bash
kubectl create namespace production
kubectl create namespace staging
kubectl create namespace development
```

```bash
# Deploy to a specific namespace
kubectl apply -f deployment.yaml -n production

# List all resources across namespaces
kubectl get pods --all-namespaces
```

Common patterns:

- **Environment isolation**: `production`, `staging`, `development`
- **Team isolation**: `team-payments`, `team-auth`, `team-platform`
- **System workloads**: `kube-system`, `monitoring`, `logging`

---

## Essential kubectl Commands

### Cluster Information

```bash
kubectl cluster-info
kubectl get nodes
kubectl top nodes
```

### Working with Pods

```bash
kubectl get pods
kubectl get pods -n production
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> -f          # follow logs
kubectl exec -it <pod-name> -- bash # shell into a pod
```

### Working with Deployments

```bash
kubectl get deployments
kubectl scale deployment nginx-deployment --replicas=5
kubectl set image deployment/nginx-deployment nginx=nginx:1.26
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment
```

### Applying and Deleting Resources

```bash
kubectl apply -f manifest.yaml
kubectl delete -f manifest.yaml
kubectl delete pod <pod-name> --force
```

### Debugging

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl describe node <node-name>
kubectl top pods
```

---

## Amazon EKS

**Amazon Elastic Kubernetes Service (EKS)** is AWS's fully managed Kubernetes control plane. AWS handles the undifferentiated heavy lifting of cluster operations, while you focus on your workloads.

### What AWS Manages

- Control Plane availability (multi-AZ by default)
- API Server and etcd
- Kubernetes version upgrades
- Control Plane security patching

![Image dashobard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0omrxrcqodh1zvupz3xd.png)

### What You Manage

- Worker nodes (EC2, Fargate, or Managed Node Groups)
- Application deployments
- Networking configuration
- IAM and security policies

### EKS Architecture Pattern

![EKS Architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vlx9f0uoy9z26217ughp.png)

### Key EKS Integrations

- **IAM Roles for Service Accounts (IRSA)**: Fine-grained AWS API access for Pods
- **AWS Load Balancer Controller**: Native ALB/NLB provisioning from Kubernetes
- **EBS/EFS CSI Drivers**: Persistent storage integration
- **VPC CNI Plugin**: Native VPC networking for Pods
- **Karpenter**: Next-gen node autoscaling for EKS

---

## Real-World Use Cases

### 🎬 Media & Streaming

Netflix and Disney use Kubernetes to handle massive traffic spikes during content releases, dynamically scaling video encoding and streaming workloads.

### 💳 Fintech & Banking

Microservices architectures running on Kubernetes enable independent deployment of transaction APIs, fraud detection services, and real-time analytics pipelines.

### 🤖 AI & Machine Learning

Kubernetes orchestrates GPU-accelerated inference workloads, distributed training jobs, and ML pipeline execution at scale. Projects like Kubeflow are built natively on Kubernetes.

### 🛒 E-Commerce

Platforms like Shopify use Kubernetes to absorb unpredictable traffic during flash sales and seasonal peaks without manual intervention.

---

## Challenges of Kubernetes

Kubernetes is powerful, but it is not simple. Teams should plan for:

- **Steep learning curve**: Significant investment in training required
- **Networking complexity**: CNI selection, service mesh decisions, ingress configuration
- **Security surface**: RBAC, network policies, and secrets management need deliberate design
- **Cost optimization**: Right-sizing nodes and managing cluster sprawl is non-trivial
- **Observability**: Requires a mature monitoring, logging, and tracing stack

> The operational complexity of Kubernetes is why managed services like EKS, GKE, and AKS exist.

---

## Kubernetes Ecosystem Tools

| Tool           | Category              | Purpose                                            |
| -------------- | --------------------- | -------------------------------------------------- |
| **Helm**       | Package Management    | Templating and versioning for Kubernetes manifests |
| **Kustomize**  | Configuration         | Environment-specific overlays without templates    |
| **Prometheus** | Monitoring            | Time-series metrics collection for Kubernetes      |
| **Grafana**    | Visualization         | Dashboards for Prometheus and other data sources   |
| **ArgoCD**     | GitOps / CD           | Declarative continuous delivery to Kubernetes      |
| **Flux**       | GitOps / CD           | Lightweight GitOps toolkit                         |
| **Istio**      | Service Mesh          | Traffic management, mTLS, observability            |
| **Cilium**     | Networking / Security | eBPF-based CNI with advanced network policies      |
| **Velero**     | Backup & Recovery     | Cluster and persistent volume backup               |
| **K9s**        | Developer Tooling     | Terminal-based Kubernetes cluster management       |
| **Karpenter**  | Autoscaling           | Node autoscaling for AWS and beyond                |

---

## Final Thoughts

Kubernetes has become the backbone of modern cloud-native infrastructure. It transformed how engineering teams deploy applications, scale services, manage infrastructure, and build resilient distributed systems.

Whether you're a DevOps engineer, platform engineer, SRE, backend developer, or cloud architect — Kubernetes proficiency is no longer a differentiator.

**It is a baseline expectation.**

The investment in learning Kubernetes pays dividends across every dimension of modern software delivery.

---

## Official Resources

- [Kubernetes Official Documentation](https://kubernetes.io/docs/)
- [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/)
- [Helm Documentation](https://helm.sh/docs/)
- [Prometheus Documentation](https://prometheus.io/docs/)
- [CNCF Landscape](https://landscape.cncf.io/)
- [Karpenter Documentation](https://karpenter.sh/)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)

---
