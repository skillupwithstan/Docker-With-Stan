What is Express Mode?
Overview
Express Mode is a new ECS feature that enables rapid deployment of containerized applications.
Developers simply provide a container image, and ECS automatically orchestrates the infrastructure.
Key Features
The four key features are:
One-click setup
AWS-provided domain name with HTTPS support
Auto scaling and load balancing
Complete control retention
One-click Setup
Deployment is completed by simply specifying a container image URI and two IAM roles from the ECS console or AWS CLI.
AWS-provided Domain Name with HTTPS Support
A unique URL is automatically generated upon deployment and becomes immediately accessible via HTTPS.
Auto Scaling and Load Balancing
Task counts are automatically adjusted based on traffic patterns, with load balancing performed by Application Load Balancer (ALB).
Up to 25 Express Mode services can be consolidated behind a single ALB.
Complete Control Retention
All automatically configured resources are fully accessible within your account and can be directly modified later as needed.
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

Key Components
Fargate Tasks
AWS Fargate is used as the container execution environment.
Server management is not required, and CPU/memory can be configured individually.
Application Load Balancer
Responsible for traffic load balancing, distinguishing multiple services through host-header based listener rules.
As a key feature of Express Mode, up to 25 services can be consolidated behind a single ALB. Each service is assigned a unique domain name, and the ALB routes traffic using host-headers.
This mechanism enables cost reduction for ALBs while maintaining service isolation.
Route 53
Automatically generates AWS-provided domain names.
Each service is assigned a unique URL, and an A record targeting the ALB is automatically created.
AWS Certificate Manager (ACM)
Responsible for automatic issuance and management of SSL/TLS certificates.
Certificate renewal is also automated, requiring no manual management.
Auto Scaling
By default, CPU utilization-based Auto Scaling policies are configured.
Task counts are automatically adjusted based on traffic patterns, scaling within the range from minimum to maximum task counts.
Security Groups
Security groups are automatically created based on the principle of least privilege, with only necessary ports opened.
Deployment Flow
Deployment with Express Mode is automatically executed following this flow:

All processes are automated, requiring no user intervention.
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
How to Use
This section explains how to use Express Mode using a sample application.
Sample Application Preparation
We'll create a simple Flask application that actually works.
Application Code Creation
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
from flask import Flask, jsonify
import os

app = Flask(__name__)

@app.route('/')
def hello():
    hostname = os.environ.get('HOSTNAME', 'unknown')
    return f'Hello from ECS Express Mode! Container: {hostname}'

@app.route('/health')
def health():
    return jsonify({'status': 'healthy'}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80)

1
2
Flask==3.0.0
gunicorn==21.2.0

Creating Dockerfile
1
2
3
4
5
6
7
8
9
10
11
12
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 80

CMD ["gunicorn", "--bind", "0.0.0.0:80", "app:app"]

Pushing to Amazon ECR
Build the container image and push it to Amazon ECR.
Building docker image
1
2
3
4
5
# Build Docker image
docker build -t express-mode-demo .

# Verify built image
docker images | grep express-mode-demo

Creating ECR repository
1
2
3
aws ecr create-repository \
  --repository-name express-mode-demo \
  --region ap-northeast-1

Pushing image
1
2
3
4
5
6
7
8
9
10
11
# Login to ECR
aws ecr get-login-password --region ap-northeast-1 | \
  docker login --username AWS --password-stdin \
  <account-id>.dkr.ecr.ap-northeast-1.amazonaws.com

# Tag image
docker tag express-mode-demo:latest \
  <account-id>.dkr.ecr.ap-northeast-1.amazonaws.com/express-mode-demo:latest

# Push image
docker push <account-id>.dkr.ecr.ap-northeast-1.amazonaws.com/express-mode-demo:latest

Deploying with Express Mode
Selecting Express Mode
Open the ECS console and select "Express mode" from the navigation bar

Application Setup
Enter the following information:
Image URI: ECR image URI
Task execution role: ecsTaskExecutionRole (create if not exists)
Infrastructure role: ecsInfrastructureRoleForExpressServices (create if not exists)
Optional Configuration
Enter the following information:
Cluster: Target ECS cluster for deployment
Name: express-mode-demo
Container port: 80
Health check path: /health
Creation
After clicking the "Create" button, deployment progress can be monitored in the "Timeline view"

Verification
Once deployment is complete, access the auto-generated URL to verify operation.
1
2
3
4
5
# Access test
curl https://<auto-generated-url>/

# Health check verification
curl https://<auto-generated-url>/health

1
2
3
4
5
# Access test
# Hello from ECS Express Mode! Container: ecs-express-mode-demo-xxxxx

# Health check verification
# {"status": "healthy"}

You can also access the URL via browser.

When to Use Express Mode
While Express Mode is a powerful feature, it's not optimal for all use cases.
This section explains cases where Express Mode is recommended and cases where regular ECS should be used.
Cases Where Express Mode is Suitable
Web Applications and APIs
Optimal for web applications and APIs that handle stateless HTTP requests.
RESTful APIs and web application backends—any request-response type application can be rapidly deployed with Express Mode.
Rapid Prototyping
Effective when you want to quickly run applications without spending time on infrastructure setup, such as validating new features or proof of concept (PoC).
Since an HTTPS-accessible environment is ready within minutes, it significantly shortens the idea validation cycle.
Developer Productivity
Provides an environment where developers can deploy independently without deep AWS knowledge, such as small startup teams or frontend engineers deploying their own APIs.
Developers can deploy autonomously without waiting for infrastructure team support.
Cases Where Regular ECS Should be Used
Advanced Network Requirements
Use regular ECS when you need custom VPC endpoints, complex network topologies, or detailed control over Private Link and VPC peering.
Express Mode assumes standard network configurations and doesn't accommodate advanced customization.
Blue/Green Deployment Requirements
Express Mode doesn't support Blue/Green deployments by default.
When zero-downtime switching or easy rollback is critical, explicit configuration in regular ECS services is necessary.
EC2 Launch Type Requirements
Express Mode only supports Fargate.
Use regular ECS when you need GPU instances, execution on specific EC2 instance types, or cost optimization through spot instances.
Adoption Decision Flow

Migrating from Express Mode to Regular ECS
Since all resources created by Express Mode are accessible within your account, you can seamlessly migrate to regular ECS services as application requirements evolve.
The recommended approach is to start with Express Mode and incrementally add advanced features as needed.
This strategy is effective: prioritize rapid deployment in the early stages, then migrate when fine-grained control becomes necessary as the service matures.
Costs and Considerations
This section explains costs and considerations you should know before using Express Mode.
Pricing
There is no additional charge for the Express Mode feature itself.
However, standard usage charges apply for AWS resources created by Express Mode.
Cost Components
Fargate: Charged based on task vCPU and memory usage
Application Load Balancer: Charged based on ALB operating hours and LCU (Load Balancer Capacity Units)
Data Transfer: Charged based on outbound data transfer volume
CloudWatch Logs (optional): Charged based on log ingestion and storage volumes
Cost Optimization Tips
ALB Sharing: Significantly reduce ALB costs by consolidating services using the same network configuration into a single ALB
Considerations
Feature Limitations
Launch Type: Fargate only (EC2 launch type not supported)
ALB Sharing: Maximum 25 services per ALB
Blue/Green Deployment: Not supported by default (migration to regular ECS service required)
Custom VPC Endpoints: Manual configuration required
Operational Notes
Resources created by Express Mode can be manually modified, but migration to regular ECS is recommended for significant customization
The 26th and subsequent services automatically create a new ALB
Summary
Express Mode, announced this time, is a practical feature that significantly reduces deployment complexity while maintaining the flexibility of traditional ECS.
By simply specifying a container image, an HTTPS-enabled production environment can be built within minutes.
All automatically configured resources are accessible within your account, allowing customization and migration to regular ECS as needed.
Since initial setup that previously took hours to days now completes in minutes, we recommend trying it first in a non-production environment.
This feature, which eliminates infrastructure configuration overhead and enables developers to focus on delivering business value, significantly lowers the barrier to container deployment.
