# AWS Production-Grade 3-Tier Java Application

This project demonstrates how to design, build, and deploy a **production-ready 3-tier Java application on AWS** using real-world cloud and DevOps best practices.

The goal of this project is not just to run an application, but to build a **secure, scalable, highly available system** similar to what is used in real production environments.

---

## Architecture Overview

User >> Route 53 (DNS) >> CloudFront (CDN + HTTPS) >> Application Load Balancer (Public Subnets) >> EC2 Auto Scaling Group (Private App Subnets) >> Redis Cache RDS Proxy (Cache Layer) >> RDS MySQL (Primary + Read Replica)

---

## Technology Stack

- **Backend**: Java, Spring Boot
- **Compute**: EC2 Auto Scaling Group
- **Networking**: VPC, Subnets, Internet Gateway, NAT Gateway
- **Load Balancer**: Application Load Balancer (ALB)
- **Database**: Amazon RDS MySQL (Multi-AZ) + Read Replica
- **DB Connections**: Amazon RDS Proxy
- **Secrets**: AWS Secrets Manager (with automatic rotation)
- **Cache**: Amazon ElastiCache (Redis)
- **Edge**: Amazon CloudFront
- **DNS**: Amazon Route 53
- **Access**: Bastion Host
- **Security**: IAM Roles, Security Groups

---

## Application Profiles

The application uses **Spring Profiles** to support multiple environments using the **same codebase**.

| Profile | Purpose |
|---------|---------|
| `local` | Run without DB, Redis, or ALB (bastion/local testing) |
| default (prod) | Full production stack (RDS, Redis, ALB, CloudFront) |

### Run in local/bastion mode
```bash
java -jar app.jar --spring.profiles.active=local

### Run in production
```bash 
java -jar app.jar

### Step-by-Step Build Guide
**1. Networking (VPC)**
    - Create a VPC (10.0.0.0/16)
    - Create subnets:
        Public subnets (ALB, NAT Gateway, Bastion)
        Private app subnets (EC2 Auto Scaling)
        Private DB subnets (RDS, RDS Proxy, Read Replica)
        Private cache subnets (Redis)
    - Attach Internet Gateway
    - Create NAT Gateway
    - Configure public and private route tables

  **2. Security Groups**
    - ALB SG: Allow HTTP/HTTPS from the internet
    - Bastion SG: Allow SSH from your IP
    - App SG: Allow traffic only from ALB and Bastion
    - DB SG: Allow MySQL traffic only from App SG
    - Redis SG: Allow Redis traffic only from App SG

  **3. Compute Layer**
    - Deploy a Bastion Host in a public subnet
    - Create an Application Load Balancer
    - Create a Target Group with /health endpoint
    - Create a Launch Template
    - Create an Auto Scaling Group using private app subnets

  **4. Database Layer**
    - Create RDS MySQL (Multi-AZ)
    - Create a Read Replica
    - Create a DB Subnet Group
    - Create and attach Amazon RDS Proxy

  **5. Secrets Management**
    - Store DB credentials in AWS Secrets Manager
    - Enable automatic secret rotation
    - Attach an IAM role to EC2 instances to allow secret access

  **6. Cache Layer**
    - Create an ElastiCache Redis cluster
    - Place Redis in private cache subnets
    - Enable caching in Spring Boot

  **7. Edge & DNS**
    - Create a CloudFront distribution in front of ALB
    - Request an ACM certificate
    - Configure Route 53 with a custom domain

### Author
Built as a hands-on project to learn real-world AWS architecture and DevOps practices using Java and Spring Boot.