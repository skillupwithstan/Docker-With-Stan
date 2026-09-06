Amazon ECS Express Mode Setup Guide

## What is ECS Express Mode?

Amazon ECS Express Mode is a feature that enables rapid deployment of containerized applications. Provide a container image, and ECS automatically orchestrates the infrastructure.

* **One-Click Setup:** Deploy by specifying an image URI and two IAM roles.
* **Auto-Generated URLs:** Instantly get an AWS-provided domain name with HTTPS support.
* **Smart Scaling & Load Balancing:** Consolidate up to 25 services behind a single Application Load Balancer (ALB).
* **Zero Lock-In:** Retain complete access to modify auto-created resources later.

## Traditional ECS vs. Express Mode

| Feature | Traditional ECS | Express Mode |
| --- | --- | --- |
| **Initial Setup Time** | Hours to Days | Minutes |
| **Required Config Items** | 10+ Resources | 3 Items |
| **HTTPS Configuration** | Manual | Automatic |
| **ALB Cost (5 Services)** | 5 ALBs | 1 ALB |

## Auto-Configured Architecture

When deployed, Express Mode automatically provisions the following resources using the principle of least privilege:

* **Fargate Task Definition:** Serverless container execution.
* **Application Load Balancer:** Traffic routing (host-header based).
* **Route 53 & ACM:** Auto-provisioned domain names and SSL/TLS certificates.
* **Auto Scaling Policy:** Pre-configured CPU utilization-based scaling.

Automatically Configured Resources
When using Express Mode, the following AWS resources are automatically created:

ECS Cluster: The default cluster is used if not specified

Fargate Task Definition: Serverless container execution environment

ECS Service: Task lifecycle management

Application Load Balancer: Traffic load balancing

Route 53 Record: AWS-provided domain name

ACM Certificate: SSL/TLS certificate for HTTPS support

Security Groups: Configured based on the principle of least privilege

Auto Scaling Policy: CPU utilization-based by default

How It Works
Let's explore in detail how Express Mode automatically orchestrates infrastructure and deploys applications.
Architecture Overview
Express Mode uses AWS Fargate as the compute engine, providing a serverless container execution environment.

<img width="1000" height="672" alt="image" src="https://github.com/user-attachments/assets/733cfa13-5b78-4006-b6b3-a75f0317edf7" />

**Key Components**
___________________

**Fargate Tasks**
AWS Fargate is used as the container execution environment.
Server management is not required, and CPU/memory can be configured individually.

**Application Load Balancer**
Responsible for traffic load balancing, distinguishing multiple services through host-header based listener rules.
As a key feature of Express Mode, up to 25 services can be consolidated behind a single ALB. Each service is assigned a unique domain name, and the ALB routes traffic using host-headers.
This mechanism enables cost reduction for ALBs while maintaining service isolation.

**Route 53**
Automatically generates AWS-provided domain names.
Each service is assigned a unique URL, and an A record targeting the ALB is automatically created.

**AWS Certificate Manager (ACM)**
Responsible for automatic issuance and management of SSL/TLS certificates.
Certificate renewal is also automated, requiring no manual management.

**Auto Scaling**
By default, CPU utilization-based Auto Scaling policies are configured.
Task counts are automatically adjusted based on traffic patterns, scaling within the range from minimum to maximum task counts.

**Security Groups**

Security groups are automatically created based on the principle of least privilege, with only necessary ports opened.

**Deployment Flow**
Deployment with Express Mode is automatically executed following this flow:

<img width="1000" height="396" alt="image" src="https://github.com/user-attachments/assets/e8342881-62f5-4b83-9e50-03d689b5278c" />

## Quick Start Guide

### 1. Create a Sample Application

**`app.py`**

```python
from flask import Flask, jsonify
import os
app = Flask(__name__)

@app.route('/')
def hello():
    hostname = os.environ.get('HOSTNAME', 'unknown')
    return f'Thank you for Choosing Docker with Stan! Container: {hostname}'

@app.route('/health')
def health():
    return jsonify({'status': 'healthy'}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80)

```

**`requirements.txt`**

```text
Flask==3.0.0
gunicorn==21.2.0

```

**`Dockerfile`**

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 80
CMD ["gunicorn", "--bind", "0.0.0.0:80", "app:app"]

```

### 2. Build and Push to Amazon ECR

```bash
# 1. Build and tag the image
docker build -t express-mode-demo .

# 2. Create an ECR repository
aws ecr create-repository --repository-name express-mode-demo --region ap-northeast-1

# 3. Authenticate Docker to ECR
aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-northeast-1.amazonaws.com

# 4. Tag and Push
docker tag express-mode-demo:latest <account-id>.dkr.ecr.ap-northeast-1.amazonaws.com/express-mode-demo:latest
docker push <account-id>.dkr.ecr.ap-northeast-1.amazonaws.com/express-mode-demo:latest

```

### 3. Deploy via Console

1. Navigate to the **ECS Console** and select **Express mode**.
2. Provide your **Image URI**.
3. Select or create your **Task Execution Role** and **Infrastructure Role**.
4. Set container port to `80` and health check path to `/health`.
5. Click **Create** and wait a few minutes for the auto-generated HTTPS URL.

## When to Use Express Mode

* **Recommended For:** Web APIs, stateless HTTP requests, rapid prototyping, and scenarios prioritizing developer autonomy.
* **Use Regular ECS For:** Blue/Green deployments, EC2 launch types (e.g., GPU instances), custom VPC endpoints, or advanced network topologies.

What Changed
Let's examine how Express Mode has transformed container deployment in ECS by looking at traditional challenges and changes brought by the new feature.
Traditional Challenges
There were two main challenges:
Complex infrastructure configuration
Increasing ALB costs
Complex Infrastructure Configuration
Traditional ECS required individually configuring many components including VPC, subnets, security groups, ALB, target groups, ECS clusters, task definitions, service definitions, Route 53 domain configuration, ACM certificate issuance, and Auto Scaling policies.
Advanced AWS knowledge was required, and initial deployment often took hours to days.
Increasing ALB Costs
Each service required its own ALB, with even small microservices incurring full ALB costs.
Operating five API services meant paying for five separate ALBs.
Changes with Express Mode
Express Mode resolves all traditional challenges.
Simplified Configuration
Deployment is completed by simply specifying a container image URI and two IAM roles.
All infrastructure components are automatically created, enabling deployment to production environments within minutes.
Cost Optimization
Up to 25 services can be consolidated behind a single ALB.
While traditional ECS required individual ALBs for each of five API services, Express Mode consolidates them into a single ALB, significantly reducing ALB costs.
Rapid Deployment
Deployment completes within minutes, with immediate access via auto-generated HTTPS URLs.
Complete Control Retention
All automatically configured resources are fully accessible within your account.
Customization is possible as needed, with migration to regular ECS services also available.
Comparison Table
Item	Traditional ECS	Express Mode
Initial Setup Time	Hours to Days	Minutes
Required Configuration Items	10+ Resources	3 Items
HTTPS Configuration	Manual	Automatic
ALB Cost (5 Services)	5 ALBs	1 ALB
