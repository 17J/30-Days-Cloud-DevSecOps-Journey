Day 23 - Github Actions CI/CD Pipeline

In Present Time software teams need fast, secure, and automated delivery.

Earlier, release flow looked like this:

```text
Developer writes code
        ↓
Manual build
        ↓
Manual test
        ↓
Manual deployment
        ↓
Production issue
```

Today, GitHub Actions can automate this entire process directly from your GitHub repository.


---

## 🔗 Resources

* ** Support the Journey on GitHub:
If you're following along, consider starring and forking the repo:**
    [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)


---


## What is GitHub Actions?

GitHub Actions is GitHub’s automation platform for building, testing, scanning, packaging, and deploying applications.

You define automation using YAML files inside:

```text
.github/workflows/
```

Example:

```text
.github/workflows/ci-cd.yml
```

A workflow can run when:

```text
Code is pushed
Pull request is opened
Tag is created
Manual trigger is clicked
Schedule runs
External webhook/event triggers it
```

GitHub supports workflow triggers for repository activity, schedules, and external events.

---

## Why GitHub Actions?

GitHub Actions is powerful because it is close to the source code.

Benefits:

```text
Code + CI/CD + Security + Packages + Deployments
```

inside one platform.

It helps teams:

* Build automatically
* Test every pull request
* Run security scans
* Build Docker images
* Push artifacts
* Deploy to cloud
* Deploy to Kubernetes
* Use approval gates
* Use OIDC instead of long-lived cloud keys

---

## GitHub Actions Core Concepts

### Workflow

A workflow is the complete automation file.

Example:

```yaml
name: CI/CD Pipeline
```

It contains triggers, jobs, permissions, and steps.

---

### Event

An event starts the workflow.

Example:

```yaml
on:
  push:
    branches: [ main ]
  pull_request:
```

This means the workflow runs on push to main and pull requests.

---

### Job

A job is a group of steps.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

---

### Step

A step is a single command or action.

Example:

```yaml
steps:
  - name: Checkout Code
    uses: actions/checkout@v4
```

---

### Action

An action is a reusable task.

Example:

```yaml
uses: actions/setup-node@v4
```

Actions help you reuse community or official automation components.

---

### Runner

A runner is the machine that executes your job.

There are two main types:

```text
GitHub-hosted runner
Self-hosted runner
```

GitHub-hosted runners are managed by GitHub, while self-hosted runners are machines you manage yourself.

---


![First Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vrywvrqbp4szxj4sv9fv.png)
---

## What are Private / Self-Hosted Runners?

A self-hosted runner is a machine deployed and managed by you to execute GitHub Actions jobs.

It can run on:

* EC2
* Azure VM
* GCP VM
* Kubernetes
* On-prem server
* Private subnet

Use self-hosted runners when your pipeline needs access to:

```text
Private Kubernetes Cluster
Private Database
Internal Nexus
Private SonarQube
Internal APIs
Private VPC Resources
```

---

## Self-Hosted Runner Labels

When added, self-hosted runners automatically receive labels like:

```text
self-hosted
linux
windows
macOS
x64
ARM
ARM64
```

GitHub uses these labels to route jobs.

Example:

```yaml
runs-on: [self-hosted, linux, x64]
```

This job will run on a private Linux x64 runner.

---

## Private Runner Example

```yaml
jobs:
  deploy:
    runs-on: [self-hosted, linux, x64]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Deploy to Private Kubernetes
        run: |
          kubectl get nodes
          kubectl apply -f k8s/
```

This is useful when your Kubernetes API is not public.

---

## Self-Hosted Runner Security Best Practices

Use private runners carefully.

Best practices:

```text
Use ephemeral runners
Restrict repository access
Use runner groups
Avoid running untrusted fork PRs
Use least privilege
Patch runners regularly
Do not store secrets on runner disk
```

For Kubernetes-based autoscaling runners, GitHub identifies Actions Runner Controller as the recommended Kubernetes solution for autoscaling self-hosted runners.

---

## What is Pipeline YAML?

GitHub Actions pipeline is written in YAML.

Basic structure:

```yaml
name: Pipeline Name

on:
  push:

jobs:
  job-name:
    runs-on: ubuntu-latest

    steps:
      - name: Step Name
        run: echo "Hello CI/CD"
```

---

## Important YAML Sections

### name

```yaml
name: Node.js CI/CD
```

Pipeline display name.

---

### on

```yaml
on:
  push:
    branches: [ main ]
```

Defines when pipeline runs.

---

### permissions

```yaml
permissions:
  contents: read
  id-token: write
```

Defines workflow token permissions.

`id-token: write` is required for OIDC-based cloud authentication.

---

### env

```yaml
env:
  APP_NAME: my-app
```

Defines environment variables.

---

### jobs

```yaml
jobs:
  build:
```

Defines pipeline jobs.

---

### runs-on

```yaml
runs-on: ubuntu-latest
```

Defines runner machine.

---

### steps

```yaml
steps:
  - run: npm install
```

Commands or reusable actions.

---

## Secrets in GitHub Actions

Secrets are encrypted sensitive values.

Examples:

```text
AWS_ACCOUNT_ID
SONAR_TOKEN
DOCKERHUB_TOKEN
SLACK_WEBHOOK
DATABASE_PASSWORD
```

Access secrets like this:

```yaml
env:
  SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

Never hardcode secrets in YAML.

---

## Variables in GitHub Actions

Variables are non-sensitive configuration values.

Examples:

```text
AWS_REGION=ap-south-1
APP_NAME=todo-api
ENVIRONMENT=dev
```

GitHub supports variables and exposes them through the `vars` context.

Example:

```yaml
env:
  AWS_REGION: ${{ vars.AWS_REGION }}
```

---


![Second Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/u6i0v4ktzt3jqjwcua0n.png)

---

## What is GITHUB_TOKEN?

`GITHUB_TOKEN` is an automatically generated token available in workflows.

It can be used for GitHub API operations like:

* Checkout
* Comment on PR
* Create releases
* Push tags
* Update repo content

GitHub provides documentation explaining how `GITHUB_TOKEN` works for secure automation.

Example:

```yaml
permissions:
  contents: read
  packages: write
```

---

## What is OIDC in GitHub Actions?

OIDC means OpenID Connect.

It allows GitHub Actions to authenticate with cloud providers without storing long-lived access keys.

Old approach:

```text
Store AWS_ACCESS_KEY_ID
Store AWS_SECRET_ACCESS_KEY
```

Better approach:

```text
GitHub Actions
      ↓
OIDC Token
      ↓
AWS IAM Role
      ↓
Temporary Credentials
```

Benefits:

* No long-lived cloud keys
* Short-lived credentials
* Better security
* Easier rotation
* Least privilege

---

## AWS OIDC Example

```yaml
permissions:
  id-token: write
  contents: read
```

```yaml
- name: Configure AWS Credentials using OIDC
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy-role
    aws-region: ap-south-1
```

---

## What is a Webhook?

A webhook is an event notification sent from GitHub to another system.

Example:

```text
GitHub Push Event
        ↓
Webhook
        ↓
External System
```

Use cases:

* Trigger Jenkins pipeline
* Notify Slack
* Trigger deployment platform
* Send events to security tools

---

## Branch Rules and Rulesets

Rules protect important branches.

Example:

```text
main branch
```

should not allow direct push.

Common rules:

* Require pull request
* Require approvals
* Require status checks
* Require signed commits
* Restrict force pushes
* Restrict deletions
* Require linear history

---

## Why Rulesets Matter

Rulesets enforce governance.

Example:

```text
Developer opens PR
        ↓
CI pipeline runs
        ↓
Tests pass
        ↓
Security scan passes
        ↓
Approval received
        ↓
Merge allowed
```

Without rulesets, someone may directly push insecure code to production branch.

---

## Environment Protection Rules

GitHub Actions can control deployments using environments, concurrency groups, and protection rules.

Example:

```yaml
environment:
  name: production
```

You can configure:

* Required reviewers
* Wait timer
* Deployment branches
* Environment secrets

Environment secrets and protection rules are available depending on repository type and plan.

---

## Full GitHub Actions CI/CD Pipeline Example

This example does:

```text
Checkout
Install dependencies
Run tests
Run SAST
Build Docker image
Push to Amazon ECR
Deploy to Kubernetes
```

```yaml
name: GitHub Actions CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  contents: read
  id-token: write
  packages: write

env:
  AWS_REGION: ap-south-1
  ECR_REPOSITORY: my-node-app
  IMAGE_TAG: ${{ github.sha }}

jobs:
  ci:
    name: Build, Test and Scan
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install Dependencies
        run: npm ci

      - name: Run Unit Tests
        run: npm test

      - name: Run Semgrep SAST
        uses: semgrep/semgrep-action@v1
        with:
          config: auto

      - name: Build Docker Image
        run: |
          docker build -t $ECR_REPOSITORY:$IMAGE_TAG .

  deploy:
    name: Build Image and Deploy
    needs: ci
    runs-on: [self-hosted, linux, x64]
    if: github.ref == 'refs/heads/main'

    environment:
      name: production

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Configure AWS Credentials using OIDC
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy-role
          aws-region: ${{ env.AWS_REGION }}

      - name: Login to Amazon ECR
        run: |
          aws ecr get-login-password --region $AWS_REGION | \
          docker login --username AWS --password-stdin \
          123456789012.dkr.ecr.$AWS_REGION.amazonaws.com

      - name: Build and Push Docker Image
        run: |
          IMAGE_URI=123456789012.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:$IMAGE_TAG
          docker build -t $IMAGE_URI .
          docker push $IMAGE_URI
          echo "IMAGE_URI=$IMAGE_URI" >> $GITHUB_ENV

      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/my-node-app \
            my-node-app=$IMAGE_URI \
            -n production

          kubectl rollout status deployment/my-node-app -n production
```

---

## Pipeline Flow Explained

```text
Developer Pushes Code
        ↓
GitHub Actions Triggered
        ↓
CI Job Runs on GitHub Runner
        ↓
Tests + SAST
        ↓
Deploy Job Runs on Private Runner
        ↓
OIDC Authenticates to AWS
        ↓
Docker Image Pushed to ECR
        ↓
Kubernetes Deployment Updated
```

---

## Build Automation

Build automation means converting source code into a deployable artifact.

Examples:

```text
Java → JAR/WAR
Node.js → Bundle
Dockerfile → Docker Image
Helm Chart → Versioned Package
```

Example:

```yaml
- name: Build Docker Image
  run: docker build -t my-app:${{ github.sha }} .
```

---

## Deploy Automation

Deploy automation means moving the artifact to the target environment.

Examples:

```text
ECR → EKS
ACR → AKS
Docker Hub → Kubernetes
S3 → CloudFront
Lambda ZIP → AWS Lambda
```

Example:

```yaml
- name: Deploy to Kubernetes
  run: kubectl apply -f k8s/
```

---

## GitOps Deployment Alternative

In modern Kubernetes setups, GitHub Actions should often do only CI.

CD should be handled by ArgoCD or Flux.

Flow:

```text
GitHub Actions
      ↓
Build Image
      ↓
Push to Registry
      ↓
Update GitOps Repo
      ↓
ArgoCD / Flux Deploys
```

This avoids giving CI pipeline direct cluster-admin deployment access.

---

## GitOps Example Step

```yaml
- name: Update GitOps Manifest
  run: |
    git config user.name "github-actions"
    git config user.email "actions@github.com"

    sed -i "s|image: .*|image: $IMAGE_URI|g" k8s/deployment.yaml

    git add k8s/deployment.yaml
    git commit -m "Update image to $IMAGE_TAG"
    git push
```

Then ArgoCD or Flux detects the manifest change and deploys it.

---

## Recommended Pre Production Pipeline


![Third Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uy05512onc9ef6x8rt6j.png)

---

## GitHub Actions Best Practices

Use:

```text
OIDC instead of access keys
Environment approvals for production
Branch rulesets
Private runners for private infra
Least privilege permissions
Pinned action versions
Secrets only for sensitive values
Variables for non-sensitive config
Concurrency control
Artifact retention policies
```

---

## Final Thoughts

GitHub Actions is more than a CI/CD tool.

It is an automation platform tightly integrated with GitHub.

It can handle:

* CI pipelines
* Security scanning
* Docker builds
* Cloud authentication
* Kubernetes deployment
* Release automation
* GitOps workflows

For modern DevOps and DevSecOps teams, GitHub Actions becomes even more powerful when combined with:

```text
Private runners
OIDC
Rulesets
Environment approvals
ArgoCD / Flux
Security scanning
```

A strong production pipeline is not only about deploying fast.

It is about deploying:

```text
Fast
Securely
Repeatably
With control
```
