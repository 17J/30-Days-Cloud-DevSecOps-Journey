Day 25 - Helm Chart


Kubernetes is powerful, but managing Kubernetes YAML files manually becomes difficult very quickly.

A simple application may need:

* Deployment
* Service
* ConfigMap
* Secret
* Ingress
* HPA
* ServiceAccount
* RBAC
* NetworkPolicy

If you write everything manually, your project can easily become messy.

This is where **Helm** comes in.

Helm is often called:

```text
The package manager for Kubernetes
```

Just like:

```text
apt installs packages on Ubuntu
yum installs packages on RHEL
npm installs packages for Node.js
Helm installs applications on Kubernetes
```



---

## 🔗 Resources

* ** Support the Journey on GitHub:
	If you're following along, consider starring and forking the repo:**
    [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)




---

## What is Helm?

Helm is a Kubernetes package manager that helps you define, install, upgrade, and manage Kubernetes applications.

Instead of applying many YAML files one by one:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f configmap.yaml
kubectl apply -f ingress.yaml
```

You can install everything using one Helm command:

```bash
helm install my-app ./my-chart
```

Helm packages Kubernetes manifests into a reusable unit called a **Helm Chart**.

---

## What is a Helm Chart?

A Helm Chart is a collection of files that describe a Kubernetes application.

A chart contains:

* Kubernetes templates
* Default configuration values
* Metadata
* Helper templates
* Dependency definitions

Think of a Helm Chart as:

```text
Reusable Kubernetes application package
```

Example:

```text
nginx-chart
 ├── Deployment
 ├── Service
 ├── ConfigMap
 ├── Ingress
 └── Values
```

---

## Why Do We Need Helm?

Without Helm, Kubernetes YAML becomes repetitive.

Example problem:

You have three environments:

```text
dev
qa
prod
```

Each environment needs the same application but different values.

Dev:

```text
replicas: 1
image: app:dev
```

Prod:

```text
replicas: 5
image: app:v1.0.0
```

Without Helm, you may create separate YAML files for every environment.

With Helm, you create one reusable chart and change only values.

---

## Helm Solves These Problems

### 1. Reusability

One chart can be reused across environments.

```text
Same Chart
   ↓
dev-values.yaml
qa-values.yaml
prod-values.yaml
```

---

### 2. Easy Configuration

You can customize deployments using `values.yaml`.

---

### 3. Version Control

Helm charts can be versioned.

Example:

```text
my-app-chart-1.0.0
my-app-chart-1.1.0
my-app-chart-2.0.0
```

---

### 4. Easy Upgrade and Rollback

Upgrade:

```bash
helm upgrade my-app ./my-chart
```

Rollback:

```bash
helm rollback my-app 1
```

---

### 5. Better Release Management

Helm tracks every release.

```bash
helm list
```

---


![helm flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/43xy87p4m3n69bf21ej3.png)


---

## Helm Architecture


![Helm Architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4vm54f2ocu7tagodt61y.png)


When you run:

```bash
helm install
```

Helm renders templates into Kubernetes YAML and sends them to the Kubernetes API server.

---

## Helm Chart File Structure

When you create a chart:

```bash
helm create my-app
```

Helm creates this structure:

```text
my-app/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── serviceaccount.yaml
│   ├── hpa.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
└── .helmignore
```

Now let’s understand every important file.

---

## Chart.yaml

`Chart.yaml` contains metadata about the chart.

Example:

```yaml
apiVersion: v2
name: my-app
description: A Helm chart for deploying my application
type: application
version: 1.0.0
appVersion: "1.0.0"
```

Explanation:

```text
apiVersion: Helm chart API version
name: Chart name
description: Short chart description
type: application or library
version: Chart version
appVersion: Application version
```

Important difference:

```text
version     = Helm chart version
appVersion  = Application version
```

---

## values.yaml

`values.yaml` stores default values for the chart.

Example:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.25"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 100m
    memory: 128Mi
```

This file makes your chart configurable.

Instead of hardcoding values inside Kubernetes YAML, Helm reads them from `values.yaml`.

---

## templates/ Directory

The `templates/` folder contains Kubernetes YAML templates.

These are not normal YAML files.

They contain template expressions like:

```yaml
{{ .Values.replicaCount }}
```

Helm replaces these values during rendering.

---

## Deployment Template Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: {{ .Release.Name }}-app

spec:
  replicas: {{ .Values.replicaCount }}

  selector:
    matchLabels:
      app: {{ .Chart.Name }}

  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}

    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
```

If `values.yaml` contains:

```yaml
replicaCount: 3

image:
  repository: nginx
  tag: "1.25"
```

Helm renders:

```yaml
replicas: 3
image: nginx:1.25
```

---

## Service Template Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: {{ .Release.Name }}-service

spec:
  type: {{ .Values.service.type }}

  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80

  selector:
    app: {{ .Chart.Name }}
```

This means the same chart can create different service types:

```yaml
service:
  type: ClusterIP
```

or:

```yaml
service:
  type: LoadBalancer
```

---

## What is Helm Templating?

Helm templating allows you to dynamically generate Kubernetes YAML.

Example:

```yaml
replicas: {{ .Values.replicaCount }}
```

This means:

```text
Take replicaCount value from values.yaml
```

Helm templates are powered by Go templating.

---

## Common Helm Template Objects

### .Values

Reads values from `values.yaml`.

```yaml
{{ .Values.image.repository }}
```

---

### .Release

Provides release information.

```yaml
{{ .Release.Name }}
```

Example:

```bash
helm install dev-app ./my-chart
```

Then:

```yaml
{{ .Release.Name }}
```

becomes:

```text
dev-app
```

---

### .Chart

Provides chart metadata.

```yaml
{{ .Chart.Name }}
```

---

### .Namespace

Returns release namespace.

```yaml
{{ .Release.Namespace }}
```

---

### _helpers.tpl

`_helpers.tpl` stores reusable template snippets.

Example:

```yaml
{{- define "my-app.fullname" -}}
{{ .Release.Name }}-{{ .Chart.Name }}
{{- end }}
```

Use it like:

```yaml
name: {{ include "my-app.fullname" . }}
```

This keeps templates clean and avoids duplication.

---

### NOTES.txt

`NOTES.txt` displays information after chart installation.

Example:

```text
Application installed successfully.

Access your app using:

kubectl port-forward svc/{{ .Release.Name }}-service 8080:80
```

After install, Helm prints this message.

---

### .helmignore

`.helmignore` works like `.gitignore`.

It tells Helm which files to ignore while packaging the chart.

Example:

```text
.git/
README.md
*.log
```

---

### Helm Install Command

Install a chart:

```bash
helm install my-app ./my-app
```

Check release:

```bash
helm list
```

Check Kubernetes resources:

```bash
kubectl get all
```

---

## Helm Upgrade Command

If you update image tag or replicas:

```bash
helm upgrade my-app ./my-app
```

Example:

```bash
helm upgrade my-app ./my-app \
  --set image.tag=1.26
```

---

## Helm Rollback Command

Check release history:

```bash
helm history my-app
```

Rollback:

```bash
helm rollback my-app 1
```

This is one of Helm’s most useful features.

---

## Helm Uninstall Command

Remove release:

```bash
helm uninstall my-app
```

This deletes all Kubernetes resources created by that Helm release.

---

## Using Multiple Values Files

You can create different values files:

```text
values-dev.yaml
values-qa.yaml
values-prod.yaml
```

Example dev:

```yaml
replicaCount: 1
image:
  tag: dev
service:
  type: ClusterIP
```

Example prod:

```yaml
replicaCount: 5
image:
  tag: v1.0.0
service:
  type: LoadBalancer
```

Install dev:

```bash
helm install my-app-dev ./my-app -f values-dev.yaml
```

Install prod:

```bash
helm install my-app-prod ./my-app -f values-prod.yaml
```

Same chart, different environments.

---

## Helm Dry Run

Before installing, test the chart:

```bash
helm install my-app ./my-app --dry-run --debug
```

This shows rendered YAML without applying it.

Useful for debugging.

---

## Helm Template Command

Render YAML locally:

```bash
helm template my-app ./my-app
```

This is useful when you want to inspect final Kubernetes manifests.

---

## Helm Lint

Check chart issues:

```bash
helm lint ./my-app
```

It validates chart structure and common problems.

---

## Helm Chart Repository

A Helm chart repository stores packaged charts.





![helm Flow Proper](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5s0korpwe015u3kqp431.png)




Popular chart repositories:

* ArtifactHub
* JFrog Artifactory
* Sonatype Nexus
* GitHub Pages
* OCI Registry

---

## Packaging a Helm Chart

Package chart:

```bash
helm package my-app
```

Output:

```text
my-app-1.0.0.tgz
```

This package can be uploaded to a chart repository.

---

## Installing Public Helm Charts

Add repo:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Update repo:

```bash
helm repo update
```

Install chart:

```bash
helm install my-nginx bitnami/nginx
```

---

## Helm for Kubernetes Deployments

Kubernetes deployment without Helm:

```text
deployment.yaml
service.yaml
ingress.yaml
configmap.yaml
secret.yaml
```

Kubernetes deployment with Helm:

```text
helm install my-app ./chart
```

Helm makes Kubernetes deployment cleaner, reusable, and easier to manage.

---

## Complete Example: NGINX Helm Chart

Create chart:

```bash
helm create nginx-chart
```

Update `values.yaml`:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.25"

service:
  type: ClusterIP
  port: 80
```

Update `templates/deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: {{ .Release.Name }}-nginx

spec:
  replicas: {{ .Values.replicaCount }}

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
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
```

Update `templates/service.yaml`:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: {{ .Release.Name }}-service

spec:
  type: {{ .Values.service.type }}

  selector:
    app: nginx

  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80
```

Install:

```bash
helm install nginx-demo ./nginx-chart
```

Verify:

```bash
kubectl get pods
kubectl get svc
```

---

## Reusable Deployments with Helm

Helm helps platform teams create reusable templates.

Example:

```text
Frontend Team
Backend Team
Payment Team
Order Team
```

All can use the same base chart.

Only values change:

```text
image.repository
image.tag
replicaCount
resources
env
ingress.host
```

This creates standardization across teams.

---

## Helm in CI/CD Pipeline

Modern CI/CD pipeline:

```text
Developer Commit
      ↓
Build Docker Image
      ↓
Push Image to Registry
      ↓
Update Helm values
      ↓
helm upgrade
      ↓
Kubernetes Deployment
```

Example:

```bash
helm upgrade my-app ./chart \
  --set image.tag=$GITHUB_SHA
```

---

## Helm with GitOps

In modern Kubernetes, Helm is often used with GitOps tools.

Example:

```text
Git Repository
      ↓
Helm Chart
      ↓
ArgoCD / Flux
      ↓
Kubernetes Cluster
```

ArgoCD can deploy Helm charts directly.

---

## Helm Best Practices

### 1. Keep Charts Reusable

Avoid hardcoding environment-specific values.

Bad:

```yaml
replicas: 5
```

Good:

```yaml
replicas: {{ .Values.replicaCount }}
```

---

#3# 2. Use values.yaml Properly

Keep all configurable values in `values.yaml`.

---

### 3. Use Separate Values Files

Use:

```text
values-dev.yaml
values-prod.yaml
```

---

### 4. Run helm lint

Always validate charts.

```bash
helm lint ./chart
```

---

### 5. Use Versioning

Always version charts properly.

```yaml
version: 1.0.0
appVersion: "1.0.0"
```

---

### 6. Avoid Storing Secrets Directly

Do not store plain secrets in values files.

Use:

* External Secrets Operator
* Sealed Secrets
* HashiCorp Vault
* AWS Secrets Manager
* Azure Key Vault

---

## Common Helm Commands

```bash
helm create my-chart
helm install my-app ./my-chart
helm upgrade my-app ./my-chart
helm rollback my-app 1
helm uninstall my-app
helm list
helm history my-app
helm template my-app ./my-chart
helm lint ./my-chart
helm package my-chart
```

---


## Final Thoughts

Helm is one of the most important tools in the Kubernetes ecosystem.

It solves a very real problem:

```text
Too many YAML files
Too much repetition
Too much manual configuration
```

With Helm, you get:

* Reusable deployments
* Environment-based configuration
* Template-based Kubernetes manifests
* Easy upgrades
* Easy rollbacks
* Better release management

If Kubernetes is the platform for running containers, Helm is the tool that makes Kubernetes applications easier to package, reuse, and operate.

For DevOps Engineers, Cloud Engineers, SREs, and Platform Engineers, Helm is not optional anymore.

It is a must-have Kubernetes skill.
