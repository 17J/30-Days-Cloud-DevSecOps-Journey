Day 20 - AWS Lambda




Cloud computing has evolved dramatically over the last decade.

The journey looked something like this:

```text
Physical Servers
        ↓
Virtual Machines
        ↓
Containers
        ↓
Serverless Computing
```

One of the biggest innovations in cloud computing is **AWS Lambda**.

Instead of managing:

* Servers
* Operating Systems
* Patching
* Scaling
* Capacity Planning

You simply upload code and AWS runs it.

This is the foundation of **Serverless Computing**.

---

## What is AWS Lambda?

AWS Lambda is a serverless compute service that allows you to run code without provisioning or managing servers.

You upload a function and AWS executes it whenever an event occurs.

Example:

```text
User Uploads Image
        ↓
S3 Event Triggered
        ↓
Lambda Function Runs
        ↓
Image Processed
```

You only pay for execution time.

No execution means:

```text
No Cost
```

---

## Why AWS Introduced Lambda

Before Lambda, deploying applications looked like this:

```text
Provision EC2
       ↓
Install Runtime
       ↓
Deploy Application
       ↓
Monitor Servers
       ↓
Scale Infrastructure
       ↓
Patch OS
```

Even small applications required infrastructure management.

AWS wanted developers to focus on:

```text
Business Logic
```

Instead of:

```text
Infrastructure Management
```

Thus Lambda was introduced in 2014.

---

## What is Serverless?

Serverless does NOT mean servers don't exist.

Servers still exist.

AWS manages them for you.

Instead of:

```text
You Manage Servers
```

Lambda provides:

```text
AWS Manages Servers
You Manage Code
```

---


![Lambda works](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g9ow6mswyqu3wu0jxslh.png)

Example:

```text
API Gateway
       ↓
Lambda
       ↓
DynamoDB
```

---

## Benefits of AWS Lambda

---

## 1. No Server Management

No:

* EC2
* OS updates
* Capacity planning

---

## 2. Automatic Scaling

AWS automatically scales functions.

```text
1 Request
      ↓
1 Lambda Instance

1000 Requests
      ↓
1000 Lambda Instances
```

---

## 3. Pay Per Use

You only pay for:

```text
Requests
+
Execution Duration
```

---

## 4. Event Driven

Lambda reacts to events.

Examples:

* API requests
* S3 uploads
* SNS notifications
* SQS messages
* DynamoDB streams

---

## 5. High Availability

AWS automatically distributes Lambda execution across Availability Zones.

---

## Supported Programming Languages

AWS Lambda supports multiple runtimes.

---

### Python

Popular for:

* Automation
* AI/ML
* Data processing

Example:

```python
import json

def lambda_handler(event, context):
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

---

### Node.js

Popular for:

* APIs
* Web applications

---

### Java

Popular for:

* Enterprise workloads

---

### .NET

Popular for:

* Microsoft environments

---

### Go

Popular for:

* High performance
* Fast startup



---

### Custom Runtime

Using Custom Runtime API, you can run:

* Rust
* PHP
* Other languages

---

## Lambda Function Components

Every Lambda contains:

---

## Function Code

Your business logic.

---

## Runtime

Language execution environment.

Example:

```text
Python 3.12
Node.js 20
Java 21
```

---

## Handler

Entry point of Lambda.

Example:

```python
lambda_handler(event, context)
```

AWS invokes this function.

---

## Event

Input to the Lambda.

Example:

```json
{
  "bucket": "images"
}
```

---

## Context

Runtime information.

Contains:

* Request ID
* Timeout
* Memory

---

## Lambda Execution Lifecycle

```text
Request Arrives
        ↓
Environment Created
        ↓
Function Runs
        ↓
Response Returned
```

---

## Understanding Cold Starts

One of the most important Lambda concepts.

---

### Cold Start

When Lambda has no running execution environment:

```text
Request Arrives
       ↓
Create Environment
       ↓
Load Runtime
       ↓
Execute Function
```

Extra startup time occurs.

---

### Warm Start

If environment already exists:

```text
Request Arrives
       ↓
Execute Function Immediately
```

Faster response.

---

## What is Concurrency?

Concurrency means:

```text
How Many Functions
Can Run Simultaneously
```

Example:

```text
100 Requests
       ↓
100 Concurrent Executions
```

---

## Reserved Concurrency

Reserve capacity for critical workloads.

Example:

```text
Payment Function
Reserved = 100
```

Always guaranteed.

---

## Provisioned Concurrency

Used to eliminate cold starts.

AWS keeps execution environments warm.

Useful for:

* APIs
* User-facing workloads

---

## What is Lambda Scaling?

Lambda automatically scales horizontally.

```text
1 Request
       ↓
1 Environment

10000 Requests
       ↓
10000 Environments
```

No manual scaling required.

---

## What is a Lambda Layer?

One of the most important Lambda concepts.

Lambda Layers allow sharing code across multiple functions.

Without Layers:

```text
Function A
 └─ boto3

Function B
 └─ boto3

Function C
 └─ boto3
```

Duplication occurs.

---

## With Layers

```text
Layer
 └─ boto3

Function A
Function B
Function C
```

All functions share the same dependency.

---

## Why Lambda Layers Matter

Benefits:

* Smaller deployment packages
* Reusability
* Easier maintenance
* Faster deployments

---

## Common Layer Use Cases

### Python Libraries

```text
numpy
pandas
requests
```

---

### Monitoring Agents

```text
Datadog
New Relic
OpenTelemetry
```

---

## Lambda Storage Options

---

### Temporary Storage

```text
/tmp
```

Default:

```text
512 MB
```

Can be increased.

---

### Amazon S3

Persistent object storage.

Used for:

* Files
* Images
* Backups

---

### Amazon EFS

Network file system for Lambda.

Useful for:

* Shared storage
* Large datasets

---

## Event Sources for Lambda

---

## API Gateway

```text
User Request
      ↓
API Gateway
      ↓
Lambda
```

Most common pattern.

---

## Amazon S3

```text
File Uploaded
      ↓
Lambda Triggered
```

---

## Lambda and VPC

By default:

```text
Lambda
     ↓
AWS Managed Network
```

For private resources:

```text
Lambda
     ↓
VPC
     ↓
RDS
```

Lambda can connect to:

* RDS
* ElastiCache
* Private APIs

---

## Lambda with RDS

Common architecture:

```text
API Gateway
      ↓
Lambda
      ↓
Aurora MySQL
```

Challenges:

* Connection management
* Database scaling

Solution:

```text
RDS Proxy
```

---


## Lambda Monitoring

---

### CloudWatch Logs

Automatically captures:

* stdout
* stderr
* application logs

---

### CloudWatch Metrics

Monitor:

* Invocations
* Duration
* Errors
* Throttles
* Concurrent executions

---

### AWS X-Ray

Distributed tracing for Lambda applications.

Useful for:

* Performance analysis
* Bottleneck detection

---

## Lambda Security

---

### IAM Roles

Lambda should never use hardcoded credentials.

Use:

```text
IAM Execution Role
```

---

### Secrets Manager

Store:

* Database passwords
* API keys
* Tokens

---

### KMS Encryption

Encrypt:

* Environment variables
* Data

---

## Lambda Limits

Some important limits:

| Feature            | Limit          |
| ------------------ | -------------- |
| Timeout            | 15 Minutes     |
| Memory             | 128 MB – 10 GB |
| Ephemeral Storage  | Up to 10 GB    |
| Deployment Package | 50 MB ZIP      |
| Container Image    | 10 GB          |

---


![ec2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jcx1ao7vttfz2h33vz86.png)

---

![Lambda Container](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hhsjoerrmqf7deyjgpe9.png)

---

## Real-World Lambda Use Cases

---

### Image Processing

```text
S3 Upload
      ↓
Lambda
      ↓
Resize Image
```

---

### Serverless APIs

```text
API Gateway
      ↓
Lambda
      ↓
DynamoDB
```

---

### Log Processing

```text
CloudWatch Logs
       ↓
Lambda
       ↓
Elasticsearch/OpenSearch
```

---

### Scheduled Jobs

```text
EventBridge
      ↓
Lambda
      ↓
Daily Report
```

---

## Production Best Practices

### Keep Functions Small

One function = one responsibility.

---

### Use Layers

Avoid dependency duplication.

---

### Enable Monitoring

Use:

* CloudWatch
* X-Ray

---

### Use Provisioned Concurrency

For latency-sensitive APIs.

---

### Use RDS Proxy

For database-heavy workloads.

---

### Secure Secrets

Use:

* Secrets Manager
* Parameter Store

Never hardcode credentials.

---


---

## Final Thoughts

AWS Lambda transformed cloud computing by allowing developers to focus entirely on code.

Instead of managing:

```text
Servers
Operating Systems
Scaling
Patching
```

you simply write functions and AWS handles the infrastructure.

Lambda is ideal for:

* APIs
* Event-driven applications
* Automation
* Data processing
* Serverless architectures

Understanding concepts like:

* Layers
* Cold Starts
* Concurrency
* Provisioned Concurrency
* Event Sources
* RDS Proxy

is essential for designing production-grade serverless applications.

For modern cloud engineers, AWS Lambda is no longer optional—it is one of the most important services in the AWS ecosystem.
