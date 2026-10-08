# CloudOps-Automated-AWS-Infrastructure-with-Terraform

> A production-style two-tier AWS architecture provisioned and managed using Terraform, with a focus on scalability, security, availability, and infrastructure automation.

## 📌 Overview

**CloudForge** demonstrates the design and automated provisioning of a **two-tier application infrastructure on AWS using Terraform**.

The project uses **Infrastructure as Code (IaC)** to provision and manage AWS resources through reusable Terraform modules rather than manually configuring infrastructure through the AWS Console.

The architecture separates the **application layer** from the **database layer** and incorporates load balancing, auto scaling, managed database services, DNS, CDN delivery, web application security, and SSL/TLS encryption.

## 🏗️ Architecture

```text
                         Internet
                            │
                            ▼
                     ┌──────────────┐
                     │   Route 53   │
                     │     DNS      │
                     └──────┬───────┘
                            │
                            ▼
                     ┌──────────────┐
                     │  CloudFront  │
                     │     CDN      │
                     └──────┬───────┘
                            │
                            ▼
                     ┌──────────────┐
                     │     WAF      │
                     │ Web Security │
                     └──────┬───────┘
                            │
                            ▼
                     ┌──────────────┐
                     │     ALB      │
                     │Load Balancer │
                     └──────┬───────┘
                            │
                    ┌───────┴───────┐
                    ▼               ▼
             ┌────────────┐  ┌────────────┐
             │    EC2     │  │    EC2     │
             │ Application│  │ Application│
             └─────┬──────┘  └─────┬──────┘
                   │               │
                   └───────┬───────┘
                           ▼
                    ┌────────────┐
                    │    RDS     │
                    │  Database  │
                    └────────────┘

              Infrastructure managed by
                       Terraform
```

## 🎯 Project Objectives

- Automate AWS infrastructure provisioning using **Terraform**
- Implement a modular **Infrastructure-as-Code** architecture
- Separate application and database tiers
- Improve availability using **Auto Scaling and Load Balancing**
- Implement AWS-based security controls
- Provision managed database infrastructure using **Amazon RDS**
- Configure DNS using **Amazon Route 53**
- Improve content delivery using **Amazon CloudFront**
- Protect web traffic using **AWS WAF**
- Enable secure communication using **AWS Certificate Manager (ACM)**

## 🛠️ Technology Stack

### Infrastructure & DevOps

- Terraform
- AWS CLI
- Infrastructure as Code
- Terraform Modules

### AWS Services

- Amazon VPC
- Amazon EC2
- Amazon RDS
- Application Load Balancer
- Auto Scaling
- IAM
- Amazon S3
- Amazon Route 53
- Amazon CloudFront
- AWS WAF
- AWS Certificate Manager (ACM)

## 🔐 Networking & Security

### VPC

The infrastructure is deployed inside an isolated **Amazon VPC** with subnet and routing configuration designed to separate different parts of the application infrastructure.

### IAM

IAM roles and policies are used to provide controlled permissions to AWS resources while following the principle of least privilege where applicable.

### Security Groups

Security groups control inbound and outbound traffic between the application infrastructure and other AWS resources.

### AWS WAF

AWS WAF provides an additional web-layer security control for filtering potentially malicious HTTP/HTTPS requests.

### SSL/TLS

AWS Certificate Manager is used to support encrypted HTTPS communication.

## ⚡ Compute & Scalability

### EC2

Amazon EC2 instances provide the compute layer for the application.

### Application Load Balancer

The Application Load Balancer provides a single entry point for application traffic and distributes requests across healthy application instances.

### Auto Scaling

The Auto Scaling configuration allows the application tier to adjust the number of EC2 instances based on demand and infrastructure requirements.

This improves availability and allows the infrastructure to handle changing workloads.

## 🗄️ Database & Storage

### Amazon RDS

Amazon RDS provides the managed relational database layer.

Using RDS removes the need to manually manage the underlying database server infrastructure.

### Amazon S3

Amazon S3 provides object storage for application-related assets and other supported data.

## 🌐 DNS & Content Delivery

### Route 53

Amazon Route 53 provides DNS management for the application domain.

### CloudFront

Amazon CloudFront acts as the CDN layer to improve content delivery performance by serving supported content through geographically distributed edge locations.

## 🏗️ Terraform Structure

The infrastructure is organized using reusable Terraform modules.

```text
.
├── main.tf
├── backend.tf
├── variables.tf
├── variables.tfvars
├── modules/
│   ├── vpc/
│   ├── ec2/
│   ├── rds/
│   ├── alb/
│   ├── iam/
│   └── ...
└── README.md
```

### Important Files

| File | Purpose |
|---|---|
| `main.tf` | Root Terraform configuration and module orchestration |
| `backend.tf` | Terraform backend and provider configuration |
| `variables.tf` | Terraform variable declarations |
| `variables.tfvars` | Environment-specific variable values |
| `modules/` | Reusable infrastructure modules |
| `README.md` | Project documentation |

## 🚀 Deployment

### Prerequisites

Before deploying the infrastructure, make sure you have:

- An AWS account
- Terraform `>= 1.0`
- AWS CLI
- AWS credentials configured
- Required IAM permissions
- A domain name if DNS/HTTPS configuration is being used

Verify the installations:

```bash
terraform --version
aws --version
```

Verify your AWS credentials:

```bash
aws sts get-caller-identity
```

### 1. Clone the Repository

```bash
git clone <YOUR-REPOSITORY-URL>
cd <YOUR-REPOSITORY-NAME>
```

### 2. Configure AWS Credentials

Configure the AWS CLI:

```bash
aws configure
```

Provide:

```text
AWS Access Key ID
AWS Secret Access Key
Default region
Output format
```

**Do not commit AWS credentials to the repository.**

### 3. Configure Variables

Review:

```text
variables.tf
variables.tfvars
```

Update environment-specific values such as:

- AWS region
- VPC CIDR
- subnet configuration
- instance configuration
- RDS configuration
- domain name
- database credentials

**Never commit real passwords, access keys, secret keys, or other sensitive credentials.**

### 4. Initialize Terraform

```bash
terraform init
```

This initializes the Terraform working directory and downloads the required provider dependencies.

### 5. Validate the Configuration

```bash
terraform validate
```

This checks whether the Terraform configuration is syntactically valid.

### 6. Review the Execution Plan

```bash
terraform plan -var-file=variables.tfvars
```

Review the resources Terraform intends to create, modify, or destroy.

### 7. Provision the Infrastructure

```bash
terraform apply -var-file=variables.tfvars
```

Review the plan and confirm the deployment when prompted.

For automated environments, the following can be used when appropriate:

```bash
terraform apply -var-file=variables.tfvars --auto-approve
```

### 8. Verify the Infrastructure

After deployment, verify the resources through the AWS Console or AWS CLI.

Useful checks include:

```bash
aws ec2 describe-instances
aws rds describe-db-instances
aws elbv2 describe-load-balancers
```

### 9. Destroy the Infrastructure

When the environment is no longer required:

```bash
terraform destroy -var-file=variables.tfvars
```

For automated cleanup:

```bash
terraform destroy -var-file=variables.tfvars --auto-approve
```

> **Warning:** `terraform destroy` permanently removes Terraform-managed resources. Never execute it against infrastructure you need to preserve.

## 🔄 Terraform Workflow

The project follows the standard Terraform lifecycle:

```text
Terraform Configuration
          │
          ▼
   terraform init
          │
          ▼
   terraform validate
          │
          ▼
    terraform plan
          │
          ▼
   terraform apply
          │
          ▼
     AWS Resources
          │
          ▼
   terraform destroy
```

## 🔒 Security Considerations

The project follows several infrastructure security practices:

- IAM-based access control
- Security groups for network-level access control
- Web-layer protection through AWS WAF
- HTTPS/SSL using ACM
- Managed database infrastructure through Amazon RDS
- Separation of application and database infrastructure
- No hardcoded credentials in source code
- Sensitive configuration kept outside version-controlled source files

## 📈 Scalability & Availability

The infrastructure is designed to support scalable application workloads through:

- Application Load Balancer
- EC2 Auto Scaling
- Distributed AWS infrastructure
- Managed RDS database
- CloudFront content delivery
- Route 53 DNS management

These components help create infrastructure that can handle changing workloads while improving availability and maintainability.

## 🧠 Key DevOps Concepts Demonstrated

This project provides hands-on exposure to:

- Infrastructure as Code
- Terraform modules
- AWS networking
- VPC architecture
- EC2 infrastructure
- Load balancing
- Auto Scaling
- IAM
- Security groups
- WAF
- RDS
- S3
- DNS
- CDN
- SSL/TLS
- Infrastructure provisioning
- Infrastructure lifecycle management
- Cloud security
- AWS CLI

## 📚 What I Learned

Through this project, I worked with:

1. Designing a two-tier AWS architecture
2. Provisioning infrastructure using Terraform
3. Structuring reusable Terraform modules
4. Configuring AWS networking and security
5. Deploying scalable compute infrastructure
6. Connecting application infrastructure with managed RDS
7. Configuring DNS, CDN and HTTPS
8. Managing the infrastructure lifecycle through Terraform
9. Applying cloud security principles to AWS resources

## ⚠️ Disclaimer

This project was developed for **learning and hands-on practice with AWS and Terraform**.

The implementation was based on publicly available DevOps learning material and was adapted and configured for my own AWS environment.

All infrastructure resources should be reviewed and secured appropriately before being used in a production environment.

## 👨‍💻 Author

**Shubham Raj**

Computer Science Engineering  
Vellore Institute of Technology, Bhopal

[GitHub](https://github.com/er-shubham-raj) · [LinkedIn](https://www.linkedin.com/in/shubham-raj-a0979a289/)
```

### One thing I strongly recommend

Because you're using the original project as your starting point, **don't leave the README saying you independently designed everything** if you haven't modified it. The README above deliberately says *"based on publicly available DevOps learning material"* in the disclaimer.

Once you actually deploy it, we should replace generic statements with **your real implementation details**, for example:

> "Provisioned a VPC with 2 AZs, 4 subnets, an ALB, Auto Scaling Group and RDS using 6 reusable Terraform modules."

That is far more convincing in a Plivo interview because I can then question you about **your exact architecture** rather than a generic tutorial.

Also, before putting the project on your resume, **remove `variables.tfvars` secrets from Git history** if the cloned repository contains credentials/passwords. Use variables/environment variables or AWS Secrets Manager instead.
