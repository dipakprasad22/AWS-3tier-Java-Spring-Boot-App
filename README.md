# AWS 3-Tier Architecture – Java Spring Boot Application

This project demonstrates a **production-ready AWS 3-tier architecture** deploying a Java Spring Boot application using secure networking, high availability, and best practices.

---

## 🏗️ Architecture Overview

- **Presentation Layer**
  - Application Load Balancer (Public Subnets)
- **Application Layer**
  - EC2 instances running Spring Boot (Private Subnets)
  - Auto Scaling Group
- **Database Layer**
  - Amazon RDS MySQL (Private Subnets)

Additional components:
- Bastion Host for secure SSH access
- NAT Gateway for outbound internet access
- Security Groups for tier isolation

---

## 🚀 Technology Stack

- Java 11
- Spring Boot
- Maven
- Amazon EC2
- Amazon RDS (MySQL)
- Application Load Balancer
- Auto Scaling Group
- Bastion Host
- NAT Gateway

---

## 🔄 Request Flow

1. User sends request via browser
2. ALB routes traffic to EC2 instances
3. Spring Boot application processes request
4. Data is stored/retrieved from RDS
5. Response sent back to user

---

## 🔐 Security Design

- No public access to EC2 or RDS
- Bastion Host used for admin access
- RDS accessible only from app tier
- NAT Gateway for outbound traffic only

---

## ▶️ How to Run the Application

```bash
mvn clean package
java -jar target/java-3tier-sample-app-1.0.jar

## Create User
```bash
curl -X POST http://<ALB-DNS>/users \
-H "Content-Type: application/json" \
-d '{"name":"John","email":"john@example.com"}'

## Get Users
curl http://<ALB-DNS>/users
curl http://<ALB-DNS>/hello

By Dipak Prasad