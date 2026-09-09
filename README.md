# ShopEasy – Highly Available Web Application on AWS

## 📌 Project Overview

ShopEasy is a highly available and secure web application architecture designed and deployed on AWS.

The project demonstrates how to deploy an application across multiple Availability Zones using private EC2 instances behind an Application Load Balancer, with Amazon RDS MySQL as the private database backend.

The architecture also uses Amazon S3 for static files/images, Amazon CloudFront for content delivery, AWS Systems Manager for secure instance management, and Amazon CloudWatch for monitoring.

---

## 🏗️ Architecture

Internet
   |
   v
CloudFront
   |
   v
Application Load Balancer
   |
   +-------------------+
   |                   |
   v                   v
EC2 - AZ1          EC2 - AZ2
Private Subnet     Private Subnet
   |                   |
   +---------+---------+
             |
             v
        Amazon RDS
        MySQL
        Private

CloudFront ---> Private S3
                  |
                  +-- Images
                  +-- Static Files

CloudWatch ---> Monitoring

---

## ☁️ AWS Services Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG)
- Amazon RDS for MySQL
- Amazon S3
- Amazon CloudFront
- AWS Systems Manager (SSM)
- AWS IAM
- Amazon CloudWatch
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Amazon Machine Image (AMI)
- EC2 Launch Template

---

## 🌐 Network Architecture

The application is deployed inside a custom VPC.

### VPC

CIDR:

10.0.0.0/16

### Public Subnets

- Public Subnet AZ-1: 10.0.1.0/24
- Public Subnet AZ-2: 10.0.2.0/24

Used for:

- Application Load Balancer
- NAT Gateway

### Private Application Subnets

- Private App AZ-1: 10.0.11.0/24
- Private App AZ-2: 10.0.12.0/24

Used for:

- EC2 application servers

### Private Database Subnets

- Private DB AZ-1: 10.0.21.0/24
- Private DB AZ-2: 10.0.22.0/24

Used for:

- Amazon RDS MySQL

---

## 🔐 Security Design

Security Groups were configured using least-privilege communication.

### ALB Security Group

Allows:

- HTTP 80 from the internet
- HTTPS 443 from the internet

### EC2 Security Group

Allows:

- HTTP 80 only from the ALB Security Group

SSH access is not exposed publicly.

EC2 instances are managed using AWS Systems Manager.

### RDS Security Group

Allows:

- MySQL TCP 3306 only from the EC2 Security Group

The RDS database is not publicly accessible.

---

## ⚖️ High Availability

The application servers are deployed across two Availability Zones.

Auto Scaling Group configuration:

- Minimum instances: 2
- Desired instances: 2
- Maximum instances: 4

The Application Load Balancer distributes traffic between healthy EC2 instances.

This provides improved availability and allows the application tier to scale horizontally.

---

## 🗄️ Database

Amazon RDS for MySQL is deployed in private database subnets.

Database configuration includes:

- MySQL 8.4
- Private access
- 20 GiB GP3 storage
- Automated backups
- Encryption enabled
- Security Group restricted to application servers

Connectivity was tested successfully from a private EC2 instance to RDS on TCP port 3306.

---

## 📦 Amazon S3

A private S3 bucket is used for:

- Static files
- Product images

Public access to the bucket remains blocked.

CloudFront is authorized to access the bucket using secure CloudFront-to-S3 access.

---

## 🚀 CloudFront

Amazon CloudFront is configured in front of the private S3 bucket.

CloudFront provides:

- HTTPS delivery
- Content caching
- Global content distribution
- Secure access to private S3 objects

CloudFront Origin Access Control is used so users do not need direct public access to the S3 bucket.

CloudFront-to-S3 connectivity was tested successfully.

---

## 📊 Monitoring

Amazon CloudWatch is used to monitor the Auto Scaling Group.

Configured alarm:

`shopeasy-asg-high-cpu`

Condition:

- Average CPU utilization >= 70%
- 2 consecutive 5-minute evaluation periods

This provides basic monitoring of application server resource utilization.

---

## 🧪 Testing Performed

### EC2 → RDS

Successfully connected from a private EC2 instance to the RDS MySQL database.

### CloudFront → S3

Successfully accessed a test object through the CloudFront distribution while keeping the S3 bucket private.

### ALB → EC2

Application successfully served through the Application Load Balancer.

### Auto Scaling

Two application instances are running across separate Availability Zones.

### CloudWatch

CPU utilization alarm created and verified in OK state.

---

## 💰 Cost Optimization

The project intentionally avoids unnecessary production costs.

- No custom domain was purchased.
- Route 53 was skipped.
- AWS WAF was not enabled.
- A single NAT Gateway was used for the learning environment.
- RDS is configured for development/testing.

For production, multiple NAT Gateways, WAF, Route 53, and additional monitoring could be considered.

---

## 🎯 Key Learning Outcomes

This project demonstrates practical knowledge of:

- AWS VPC networking
- Public vs private subnets
- Route tables
- Internet Gateway
- NAT Gateway
- Security Groups
- EC2
- AMIs
- Launch Templates
- Auto Scaling
- Application Load Balancer
- RDS
- S3
- CloudFront
- IAM
- Systems Manager
- CloudWatch
- High availability
- AWS security and cost optimization

---

## 👨‍💻 Author

Gaurav Ware

AWS | DevOps | Cloud Computing
