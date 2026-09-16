# DevOps Beginner Project Roadmap

A progressive series of personal DevOps projects designed to build practical skills from fundamentals through to a complete DevOps platform.

The projects intentionally use simple applications so that the focus remains on **DevOps rather than application development**.

## Core Technologies

Throughout the projects, I will progressively practise:

- Linux
- Bash
- Git
- Python
- Networking
- GitLab CI/CD
- Docker
- Docker Compose
- AWS concepts
- AWS CLI
- LocalStack
- Terraform
- Infrastructure as Code (IaC)
- IAM and secrets management
- Monitoring and observability
- Prometheus
- Grafana
- Kubernetes
- DevSecOps

> **AWS Cost Strategy:** AWS services will initially be emulated locally using LocalStack or represented through Terraform configuration. Real AWS infrastructure is not required.

---

# Project 1 — Linux Server Lab

**Difficulty:** ⭐

## Goal

Learn how to administer the type of Linux server commonly used to host applications and DevOps tooling.

Use:

- WSL2 Ubuntu
- Ubuntu VM
- or another local Linux environment

## Tasks

Practise:

- Linux filesystem navigation
- Creating files and directories
- Users and groups
- File ownership
- File permissions
- Installing packages
- Environment variables
- Processes
- Services
- SSH
- Logs
- CPU monitoring
- Memory monitoring
- Disk monitoring
- Networking commands

## Bash Scripts

Create:

```text
scripts/
├── server_health.sh
├── backup.sh
└── cleanup_logs.sh
```

Example output:

```text
=== SERVER HEALTH ===

CPU Usage: 18%
Memory Usage: 43%
Disk Usage: 61%

Running services:
nginx
ssh
cron
```

## AWS Connection

Learn how these Linux concepts relate to an **EC2 instance**.

Understand:

- EC2 instances
- AMIs
- SSH keys
- Security Groups
- Instance users
- Services running on EC2
- EC2 logs
- Public/private IP addresses

No real EC2 instance is required.

## Skills

`Linux` `Bash` `SSH` `Processes` `Permissions` `Services` `Networking` `EC2 Concepts`

---

# Project 2 — Git + Automated Python Application

**Difficulty:** ⭐⭐

## Goal

Create a very small application and manage it using proper source-control practices.

The application itself should remain intentionally simple.

Example:

```text
devops-app/
├── app.py
├── requirements.txt
├── tests/
│   └── test_app.py
├── scripts/
│   └── run.sh
└── README.md
```

Example application output:

```text
Hello from my DevOps Lab!
```

## Tasks

Practise:

- Git repositories
- Commits
- Branches
- Merging
- Pull/Merge Requests
- `.gitignore`
- Tags
- Releases
- README documentation

Create:

```bash
./scripts/run.sh
```

which automatically:

1. Creates the environment
2. Installs dependencies
3. Runs the application

## AWS Connection

Install and learn the AWS CLI.

Understand:

```bash
aws configure
aws --version
aws help
```

Learn conceptually about:

- AWS accounts
- Regions
- Availability Zones
- AWS credentials
- Access keys
- AWS CLI profiles
- IAM users
- IAM roles

Do not store credentials in Git.

## Skills

`Git` `Bash` `Python` `AWS CLI` `IAM Concepts`

---

# Project 3 — First CI Pipeline

**Difficulty:** ⭐⭐

## Goal

Automatically validate the application whenever code is pushed.

Use GitLab CI/CD.

## Pipeline

```text
Git Push
    │
    ↓
GitLab Pipeline
    │
    ├── Install Dependencies
    │
    ├── Lint
    │
    ├── Unit Tests
    │
    └── Build
```

## Tasks

Create:

```text
.gitlab-ci.yml
```

Learn:

- Pipelines
- Jobs
- Stages
- Runners
- Artifacts
- Variables
- Exit codes
- Pipeline failures
- Pipeline logs

Deliberately break tests and observe the pipeline failing.

Then fix them.

## AWS Connection

Learn how CI/CD systems authenticate to cloud environments.

Understand:

- CI/CD variables
- Secrets
- AWS credentials
- IAM roles
- Environment variables
- Why credentials should never exist inside repositories

## Skills

`GitLab CI/CD` `YAML` `Testing` `Automation` `Secrets` `IAM`

---

# Project 4 — Dockerise the Application

**Difficulty:** ⭐⭐

## Goal

Package the application into a reproducible container.

## Tasks

Create:

```text
Dockerfile
.dockerignore
```

Practise:

```bash
docker build
docker run
docker ps
docker logs
docker exec
docker inspect
docker stop
docker rm
```

Understand:

- Images
- Containers
- Layers
- Ports
- Volumes
- Environment variables
- Container networking

## CI/CD Integration

Extend the pipeline:

```text
Git Push
    │
    ↓
Test
    │
    ↓
Lint
    │
    ↓
Docker Build
```

## AWS Connection

Learn how Docker relates to:

- Amazon ECR
- Amazon ECS
- AWS Fargate
- Amazon EKS

Understand the conceptual workflow:

```text
Dockerfile
    ↓
Docker Image
    ↓
Container Registry
    ↓
ECR
    ↓
ECS / EKS
```

## Skills

`Docker` `Containers` `Networking` `CI/CD` `ECR Concepts` `ECS Concepts`

---

# Project 5 — Multi-Container Application

**Difficulty:** ⭐⭐⭐

## Goal

Run multiple services together using Docker Compose.

## Architecture

```text
          Docker Network
                │
       ┌────────┴────────┐
       ↓                 ↓
   Python API        PostgreSQL
   Container         Container
```

## Tasks

Create:

```text
docker-compose.yml
```

Configure:

- Application container
- PostgreSQL container
- Environment variables
- Persistent database volume
- Container networking
- Health checks

Understand why `localhost` behaves differently inside containers.

## AWS Connection

Learn how this architecture could map to AWS:

```text
Local                    AWS

Application     →        ECS / EKS
PostgreSQL      →        RDS
Docker Network  →        VPC
Container Port  →        Security Group / Load Balancer
Volume          →        EBS / EFS
```

## Skills

`Docker Compose` `Networking` `PostgreSQL` `Volumes` `AWS Architecture`

---

# Project 6 — Local AWS Application

**Difficulty:** ⭐⭐⭐

## Goal

Start working directly with AWS-style services without paying for AWS infrastructure.

Use **LocalStack**.

## Architecture

```text
Application
     │
     ├────────────→ S3
     │
     ↓
    SQS
     │
     ↓
   Worker
```

## Tasks

Run LocalStack using Docker.

Create:

- S3 bucket
- SQS queue

Interact with them using the AWS CLI.

Examples:

```bash
aws s3 ls
aws s3 cp test.csv s3://my-bucket/

aws sqs list-queues
aws sqs receive-message
```

Build a Python application which:

1. Receives/uploads a file
2. Stores it in S3
3. Sends an SQS message
4. Worker consumes the message
5. Worker processes the file

## Skills

`LocalStack` `AWS CLI` `S3` `SQS` `Docker` `Event-Driven Architecture`

---

# Project 7 — Terraform Local AWS Infrastructure

**Difficulty:** ⭐⭐⭐⭐

## Goal

Stop manually creating cloud resources.

Provision the LocalStack AWS environment using Terraform.

## Structure

```text
terraform/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
└── versions.tf
```

## Infrastructure

Terraform should create:

```text
Terraform
    │
    ↓
LocalStack
    │
    ├── S3 Bucket
    ├── SQS Queue
    ├── DynamoDB Table
    └── Lambda Function
```

## Commands

Practise:

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform state list
terraform show
terraform destroy
```

## Concepts

Understand:

- Providers
- Resources
- Variables
- Outputs
- Terraform state
- Dependency graphs
- Idempotency
- Desired state
- Terraform modules

## Skills

`Terraform` `Infrastructure as Code` `AWS` `LocalStack` `State Management`

---

# Project 8 — Local Serverless AWS Pipeline

**Difficulty:** ⭐⭐⭐⭐

## Goal

Build an event-driven AWS-style architecture locally.

## Architecture

```text
                 EventBridge
                      │
                      ↓
                    Lambda
                      │
             ┌────────┴────────┐
             ↓                 ↓
            S3                SQS
             │                 │
             ↓                 ↓
         Raw Data            Worker
             │
             ↓
       Processed Data
```

## Tasks

Use Terraform to provision:

- S3
- SQS
- Lambda
- EventBridge
- DynamoDB if required

Use Python for Lambda functions.

Use Docker + LocalStack to run the infrastructure locally.

## Learn

- Event-driven architecture
- Serverless computing
- Lambda triggers
- Message queues
- Scheduled events
- Infrastructure automation

## Skills

`AWS Lambda` `S3` `SQS` `EventBridge` `Terraform` `Python` `Serverless`

---

# Project 9 — Full CI/CD Infrastructure Deployment

**Difficulty:** ⭐⭐⭐⭐

## Goal

Connect Git, CI/CD, Docker, Terraform and AWS-style infrastructure.

## Architecture

```text
Developer
    │
    ↓
Git Push
    │
    ↓
GitLab CI/CD
    │
    ├── Lint
    ├── Test
    ├── Terraform Validate
    ├── Docker Build
    ├── Security Checks
    └── Deploy
             │
             ↓
         Terraform
             │
             ↓
         LocalStack
             │
       ┌─────┼─────┐
       ↓     ↓     ↓
      S3    SQS   Lambda
```

## Pipeline Stages

Example:

```text
validate
    ↓
test
    ↓
build
    ↓
security
    ↓
deploy
```

## Skills

`GitLab CI/CD` `Terraform` `Docker` `AWS` `LocalStack` `Automation`

---

# Project 10 — Monitoring and Incident Lab

**Difficulty:** ⭐⭐⭐⭐

## Goal

Learn how DevOps engineers operate and troubleshoot running systems.

Introduce:

- Prometheus
- Grafana

## Architecture

```text
Application
     │
     ↓
Prometheus
     │
     ↓
Grafana
```

## Monitor

Create metrics for:

- CPU
- Memory
- Disk
- HTTP requests
- Response times
- Error rates
- Container health

## Failure Experiments

Deliberately cause:

- Application crash
- Container crash
- Database failure
- CPU spike
- Memory pressure
- Disk usage increase
- Incorrect environment variable
- Broken network connection
- Unavailable port

Diagnose each problem.

## AWS Connection

Learn how these concepts map to:

- CloudWatch Metrics
- CloudWatch Logs
- CloudWatch Alarms
- AWS CloudTrail
- SNS alerts

Example concept:

```text
CPU > 80%
     │
     ↓
CloudWatch Alarm
     │
     ↓
SNS
     │
     ↓
Notification
```

## Skills

`Prometheus` `Grafana` `Logging` `Metrics` `Incident Response` `CloudWatch Concepts`

---

# Project 11 — DevSecOps Lab

**Difficulty:** ⭐⭐⭐⭐

## Goal

Secure the existing platform.

Pretend a security engineer has reviewed the project and requested improvements.

## Find and Fix

Look for:

- Hard-coded passwords
- Credentials committed to Git
- Overly permissive IAM
- Containers running as root
- Vulnerable dependencies
- Vulnerable Docker images
- Exposed ports
- Unnecessary permissions

## Pipeline

Extend CI/CD:

```text
Code
 │
 ↓
Lint
 │
 ↓
Unit Tests
 │
 ↓
Dependency Scan
 │
 ↓
Secret Scan
 │
 ↓
Docker Build
 │
 ↓
Container Scan
 │
 ↓
Deploy
```

## AWS Connection

Learn:

- IAM users
- IAM roles
- IAM policies
- Least privilege
- Secrets Manager
- Parameter Store
- KMS concepts
- Security Groups
- CloudTrail

## Skills

`DevSecOps` `IAM` `Secrets` `Security Scanning` `Least Privilege`

---

# Project 12 — Kubernetes Locally

**Difficulty:** ⭐⭐⭐⭐⭐

## Goal

Learn container orchestration without paying for EKS.

Use:

- Kind
- or Minikube

## Deploy

Create:

```text
kubernetes/
├── deployment.yaml
├── service.yaml
├── configmap.yaml
└── secret.yaml
```

Practise:

```bash
kubectl get pods
kubectl get deployments
kubectl get services

kubectl describe pod

kubectl logs

kubectl exec

kubectl apply

kubectl delete
```

## Scaling

Run:

```text
             Service
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
     Pod 1    Pod 2    Pod 3
```

Delete a Pod and observe Kubernetes recreating it.

## AWS Connection

Understand:

```text
Local Kubernetes          AWS

Kind / Minikube    →      EKS
Docker Image       →      ECR
Node               →      EC2 / Fargate
Service            →      AWS Load Balancer
PersistentVolume   →      EBS
Ingress            →      ALB
Secrets            →      Secrets Manager
```

## Skills

`Kubernetes` `Pods` `Deployments` `Services` `Scaling` `EKS Concepts`

---

# Project 13 — Kubernetes CI/CD

**Difficulty:** ⭐⭐⭐⭐⭐⭐

## Goal

Automatically deploy applications into Kubernetes.

## Architecture

```text
Developer
    │
    ↓
Git Push
    │
    ↓
┌────────────────────┐
│   GitLab CI/CD     │
├────────────────────┤
│ Lint               │
│ Test               │
│ Security Scan      │
│ Build Image        │
│ Push Image         │
│ Deploy             │
└─────────┬──────────┘
          ↓
   Container Registry
          │
          ↓
      Kubernetes
     ┌────┼────┐
     ↓    ↓    ↓
    Pod  Pod  Pod
```

## Tasks

Automatically:

1. Test application
2. Build Docker image
3. Scan image
4. Push image
5. Deploy new image
6. Verify deployment
7. Roll back failed deployment

## Skills

`Kubernetes` `GitLab CI/CD` `Docker` `Deployment Automation` `Rollbacks`

---

# Project 14 — Mini Company DevOps Platform

**Difficulty:** ⭐⭐⭐⭐⭐⭐⭐

## Goal

Combine everything into one final DevOps project.

Pretend I am the DevOps engineer responsible for a small company.

Developers provide:

```text
frontend/
backend/
database/
```

I am responsible for everything required to build, deploy, secure, monitor and operate the platform.

## Architecture

```text
                       GitLab
                          │
                          ↓
                       CI/CD
                          │
          ┌───────────────┼───────────────┐
          ↓               ↓               ↓
       Testing         Security         Build
                          │
                          ↓
                       Docker
                          │
                          ↓
                 Container Registry
                          │
                          ↓
                      Kubernetes
                          │
              ┌───────────┼───────────┐
              ↓           ↓           ↓
           Frontend     Backend    Database
                          │
                          ↓
                    AWS Services
                    (LocalStack)
                          │
              ┌───────────┼───────────┐
              ↓           ↓           ↓
             S3          SQS        Lambda

Terraform provisions infrastructure.

Prometheus + Grafana provide monitoring.
```

## Environments

Create separate:

```text
DEV
 ↓
STAGING
 ↓
PRODUCTION
```

environments.

Learn how configuration differs between environments.

## Infrastructure

Use Terraform for:

- AWS resources
- Infrastructure configuration
- Variables
- Modules
- Environment-specific configuration

## CI/CD

Pipeline should include:

```text
Validate
   ↓
Lint
   ↓
Test
   ↓
Security Scan
   ↓
Build
   ↓
Push
   ↓
Deploy DEV
   ↓
Integration Test
   ↓
Deploy STAGING
   ↓
Approval
   ↓
Deploy PRODUCTION
```

## Observability

Implement:

- Application logs
- Container logs
- Metrics
- Dashboards
- Alerts
- Health checks

## Security

Implement:

- Secret management
- Least privilege
- Image scanning
- Dependency scanning
- Protected CI/CD variables
- Secure containers

## Failure Testing

Practise:

- Failed deployment
- Broken application
- Failed health check
- Container crash
- Database unavailable
- Queue unavailable
- Incorrect configuration
- Rollback

## Final Skills

`Linux`

`Bash`

`Git`

`Networking`

`Python`

`GitLab CI/CD`

`Docker`

`Docker Compose`

`AWS`

`AWS CLI`

`LocalStack`

`Terraform`

`Infrastructure as Code`

`IAM`

`DevSecOps`

`Monitoring`

`Prometheus`

`Grafana`

`Kubernetes`

`EKS Concepts`

`Troubleshooting`

`Incident Response`

---

# Overall Progression

```text
1. Linux Server Lab
        │
        ↓
2. Git + Python Application
        │
        ↓
3. CI Pipeline
        │
        ↓
4. Docker
        │
        ↓
5. Docker Compose
        │
        ↓
6. Local AWS
        │
        ↓
7. Terraform + AWS
        │
        ↓
8. Serverless AWS
        │
        ↓
9. Full CI/CD
        │
        ↓
10. Monitoring
        │
        ↓
11. DevSecOps
        │
        ↓
12. Kubernetes
        │
        ↓
13. Kubernetes CI/CD
        │
        ↓
14. Mini Company Platform
```

---

# Learning Rule

For every project:

1. Build it.
2. Break it deliberately.
3. Diagnose the failure.
4. Fix it.
5. Document what happened.
6. Rebuild it without blindly copying previous commands.
7. Be able to explain why every major component exists.

Before progressing, I should be able to answer:

- What problem does this technology solve?
- Why did I use it here?
- What happens when it fails?
- How would I diagnose the failure?
- What would the AWS equivalent be?
- How would this architecture differ in production?
- What security risks exist?
- How is the infrastructure recreated?
- What part is automated?
- What part is still manual?

The goal is not simply to complete projects.

The goal is to understand how a DevOps engineer **builds, deploys, automates, secures, monitors and troubleshoots systems**.