Day 28 - Monitoring & Observability Part One

In Modern Time applications are no longer simple monolithic systems.

Today organizations run:

- Microservices
- Kubernetes
- Containers
- Serverless Functions
- Multi-Cloud Platforms
- Distributed Systems

As infrastructure becomes more distributed, troubleshooting becomes significantly harder.

A single user request may travel through:

```text
Frontend
    ↓
API Gateway
    ↓
Microservice A
    ↓
Microservice B
    ↓
Database
```

When something breaks, the biggest challenge becomes:

> "What exactly happened?"

This is where **Observability** becomes critical.

---

## 🔗 Resources

- ** Support the Journey on GitHub:
  If you're following along, consider starring and forking the repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

---

## What is Observability?

Observability is the ability to understand the internal state of a system by analyzing the data it produces.

In simple words:

```text
Can we understand
what is happening
inside our systems?
```

Observability helps engineers answer:

- Why is the application slow?
- Which service is failing?
- Which request caused the issue?
- What changed recently?
- Where is latency occurring?

Without observability:

```text
Problem Exists
      ↓
Guessing Begins
```

With observability:

```text
Problem Exists
      ↓
Evidence Available
      ↓
Faster Resolution
```

---

## Why Observability Matters

Modern cloud-native systems generate enormous amounts of data.

Example:

```text
100 Microservices
      ↓
Millions of Requests
      ↓
Thousands of Containers
```

Traditional monitoring alone is no longer sufficient.

Organizations need:

```text
Visibility
Insights
Correlation
Root Cause Analysis
```

Observability provides all of them.

---

## Monitoring vs Observability

Many people confuse monitoring and observability.

Monitoring asks:

```text
What is wrong?
```

Observability asks:

```text
Why is it wrong?
```

Example:

Monitoring:

```text
CPU Usage = 95%
```

Observability:

```text
Which service?
Which request?
Which dependency?
Which deployment caused it?
```

Observability provides context.

---

## The Three Pillars of Observability

Modern observability is built on three primary pillars.

```text
Metrics
Logs
Traces
```

Or:

```text
Monitoring
Logging
Tracing
```

Together they provide a complete picture of system behavior.

---

![First Image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2lr34lsfhl1m4mh9ehnk.png)

---

### Pillar 1: Monitoring (Metrics)

Monitoring focuses on numerical measurements.

Examples:

```text
CPU Usage
Memory Usage
Request Rate
Error Rate
Latency
Disk Usage
```

Metrics answer:

```text
How much?
How often?
How fast?
```

---

### Pillar 2: Logging

Logs provide detailed event information.

Example:

```text
User Login Success
Database Connection Failed
API Request Received
```

Logs answer:

```text
What happened?
```

---

### Pillar 3: Tracing

Tracing follows a request across multiple services.

Example:

```text
User Request
      ↓
Frontend
      ↓
API
      ↓
Payment Service
      ↓
Database
```

Tracing answers:

```text
Where did the request spend time?
```

---

## Why Metrics Matter First

Among all observability signals:

```text
Metrics
```

are usually the first thing engineers implement.

Reasons:

- Lightweight
- Efficient
- Fast alerting
- Low storage cost
- Easy visualization

This is why Prometheus became the industry standard.

---

## What is Prometheus?

Prometheus is an open-source monitoring and alerting system originally developed at SoundCloud and now maintained by CNCF.

Prometheus collects:

```text
Metrics
```

from applications and infrastructure.

Example:

```text
CPU
Memory
Network
Latency
Errors
```

---

## Why Prometheus Became Popular

Before Prometheus:

```text
Monitoring Tools
      ↓
Complex
Expensive
Difficult Scaling
```

Prometheus introduced:

```text
Pull-Based Collection
Powerful Query Language
Kubernetes Integration
Open Source
```

---

![Prometheus](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5nw8gf66jies0ltsdw7g.png)

---

## Understanding Prometheus Components

---

### Prometheus Server

Core component.

Responsible for:

- Metric collection
- Storage
- Query processing
- Alerting

---

## Exporters

Prometheus collects metrics through exporters.

Examples:

```text
Node Exporter
MySQL Exporter
MongoDB Exporter
Redis Exporter
Blackbox Exporter
```

---

## Alertmanager

Handles alerts.

Example:

```text
CPU > 90%
      ↓
Alertmanager
      ↓
Email
Slack
Teams
PagerDuty
```

---

## Time-Series Database

Prometheus stores metrics as:

```text
Timestamp + Value
```

Example:

```text
10:00 CPU=45%
10:01 CPU=48%
10:02 CPU=51%
```

---

## What is Grafana?

Grafana is a visualization platform used to create dashboards from Prometheus metrics.

Prometheus stores data.

Grafana visualizes data.

Relationship:

```text
Prometheus
      ↓
Metrics
      ↓
Grafana
      ↓
Dashboards
```

---

## Why Grafana is Popular

Grafana provides:

- Beautiful dashboards
- Alerting
- Multiple data sources
- Real-time visualization

Supported sources:

```text
Prometheus
Elasticsearch
Loki
InfluxDB
CloudWatch
Azure Monitor
```

---

## Prometheus + Grafana Architecture

```text
Applications
      ↓
Exporters
      ↓
Prometheus
      ↓
Grafana
      ↓
Engineers
```

---

## Common Metrics Monitored

Infrastructure:

```text
CPU
Memory
Disk
Network
```

Application:

```text
Request Rate
Response Time
Error Rate
```

Kubernetes:

```text
Pod Count
Node Status
Container CPU
Container Memory
```

---

## Installing Prometheus in Development Environment

For local development, Docker is easiest.

---

## Run Prometheus Container

```bash
docker run -d \
--name prometheus \
-p 9090:9090 \
prom/prometheus
```

Verify:

```text
http://localhost:9090
```

---

## Check Targets

Navigate:

```text
Status
   ↓
Targets
```

---

## Installing Node Exporter

```bash
docker run -d \
--name node-exporter \
-p 9100:9100 \
prom/node-exporter
```

This exposes:

```text
CPU Metrics
Memory Metrics
Disk Metrics
```

---

## Configure Prometheus

Example:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    static_configs:
      - targets:
          - localhost:9100
```

Restart Prometheus.

---

## Installing Grafana in Development Environment

Run Grafana:

```bash
docker run -d \
--name grafana \
-p 3000:3000 \
grafana/grafana
```

Access:

```text
http://localhost:3000
```

Default:

```text
admin/admin
```

---

## Connect Grafana to Prometheus

Add Data Source:

```text
Grafana
    ↓
Connections
    ↓
Data Sources
    ↓
Prometheus
```

URL:

```text
http://prometheus:9090
```

Save and Test.

---

## Creating First Dashboard

Example panel:

```promql
rate(node_cpu_seconds_total[5m])
```

Shows CPU usage.

---

## Installing Prometheus in Pre-Production Kubernetes

Production-like environments typically use Helm.

---

## Add Prometheus Community Repo

```bash
helm repo add prometheus-community \
https://prometheus-community.github.io/helm-charts
```

Update:

```bash
helm repo update
```

---

## Install kube-prometheus-stack

```bash
helm install monitoring \
prometheus-community/kube-prometheus-stack \
-n monitoring \
--create-namespace
```

This installs:

```text
Prometheus
Grafana
Alertmanager
Node Exporter
Kube State Metrics
```

in one deployment.

---

## Verify Installation

```bash
kubectl get pods -n monitoring
```

Expected:

```text
prometheus
grafana
alertmanager
node-exporter
```

---

## Access Grafana

```bash
kubectl port-forward svc/monitoring-grafana \
3000:80 \
-n monitoring
```

Open:

```text
http://localhost:3000
```

---

## Access Prometheus

```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus \
9090:9090 \
-n monitoring
```

Open:

```text
http://localhost:9090
```

---

## Production Monitoring Stack

A typical enterprise monitoring stack looks like:

```text
Kubernetes Cluster
       ↓
Node Exporter
       ↓
Prometheus
       ↓
Alertmanager
       ↓
Grafana
       ↓
Operations Team
```

---

## Example Alert Rule

CPU Alert:

```yaml
groups:
  - name: cpu-alerts

    rules:
      - alert: HighCPUUsage

        expr: node_cpu_seconds_total > 90

        for: 5m
```

---

## Grafana Dashboard Examples

Infrastructure Dashboard:

```text
CPU Usage
Memory Usage
Disk Usage
Network Traffic
```

Kubernetes Dashboard:

```text
Nodes
Pods
Deployments
Namespaces
```

Application Dashboard:

```text
Request Rate
Error Rate
Latency
Availability
```

---

## Monitoring Best Practices

---

### Use Labels Properly

Good:

```text
environment=prod
team=platform
service=payment
```

---

### Retain Metrics Wisely

Avoid storing metrics forever.

---

### Create Actionable Alerts

Bad:

```text
CPU > 80%
```

Good:

```text
CPU > 90% for 10 minutes
```

---

### Separate Environments

```text
Dev
QA
PreProd
Prod
```

should have independent monitoring.

---

## Observability Tools Landscape

Monitoring:

```text
Prometheus
Grafana
Datadog
New Relic
CloudWatch
Azure Monitor
```

Logging:

```text
ELK Stack
EFK Stack
Loki
Splunk
```

Tracing:

```text
Jaeger
Zipkin
Tempo
OpenTelemetry
```

---

## What We'll Cover in Part Two

This article focused on:

```text
Observability Fundamentals
Monitoring
Prometheus
Grafana
```

In Part Two we'll cover:

```text
Logging
Centralized Log Management
ELK Stack
EFK Stack
Loki
Tracing
Jaeger
OpenTelemetry
Distributed Tracing
End-to-End Observability
```

---

## Final Thoughts

Observability is one of the most important capabilities in modern cloud-native platforms.

Without observability:

```text
Failures Become Guesswork
```

With observability:

```text
Metrics
Logs
Traces
      ↓
Faster Troubleshooting
Better Reliability
Improved User Experience
```

For most organizations, the journey starts with:

```text
Prometheus
+
Grafana
```

because they provide a powerful, scalable, and Kubernetes-native monitoring platform.

Once monitoring is established, the next step is adding:

```text
Logging
+
Tracing
```

to achieve full-stack observability.
