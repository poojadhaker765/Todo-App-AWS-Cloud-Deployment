
# Todo Application - AWS Cloud Deployment

A cloud-based Todo application deployed on AWS using a scalable,
secure, and automated architecture.

## Project Overview

This project demonstrates the deployment of a full-stack Todo
application on AWS.

The application consists of a React frontend, Node.js/Express
backend, and MongoDB database.

AWS services are used for hosting, networking, load balancing,
auto scaling, monitoring, and automated database backups.

## Architecture

```text
React Frontend
      ↓
Amazon S3
      ↓
Application Load Balancer
      ↓
Node.js / Express Backend
      ↓
MongoDB EC2
Additional AWS services:
CloudWatch → Auto Scaling
Lambda → SSM → MongoDB Backup → S3
Tech Stack
Application
React.js
Node.js
Express.js
MongoDB
REST API
AWS Services
Amazon VPC
Amazon EC2
Amazon S3
Application Load Balancer
Auto Scaling Group
IAM
CloudWatch
AWS Lambda
AWS Systems Manager
Amazon EventBridge
AWS Architecture
The application is deployed inside a custom VPC with two Availability Zones.
The backend runs on EC2 instances behind an Application Load Balancer.
MongoDB is deployed on a separate EC2 instance and is accessible only from the backend security group.
CloudWatch monitors application traffic and triggers Auto Scaling when the configured threshold is reached.
MongoDB backups are automated using Lambda, Systems Manager, EventBridge, and Amazon S3.
Key Features
Todo CRUD operations
React frontend
Node.js and Express backend
MongoDB database
Custom AWS VPC
Application Load Balancer
Auto Scaling
CloudWatch monitoring
Automated MongoDB backups
IAM-based access
Security Groups for controlled communication
Security
The project follows basic AWS security practices:
MongoDB is not publicly exposed.
MongoDB port 27017 accepts traffic only from the backend.
Backend traffic is controlled through the Load Balancer.
SSH access is restricted.
EC2 uses an IAM Role instead of storing AWS access keys.
Database credentials are not committed to GitHub.
Documentation
Detailed deployment steps are available in the documentation/ directory.
documentation/
├── 01-Network-Configuration.md
├── 02-IAM-and-Security.md
├── 03-EC2-and-Backend.md
├── 04-MongoDB-Database.md
├── 05-Load-Balancer-and-ASG.md
├── 06-CloudWatch-and-Autoscaling.md
├── 07-Lambda-Backup-Automation.md
├── 08-S3-Frontend-Deployment.md
└── 09-Testing-and-Results.md
Screenshots
AWS configuration screenshots are available in the screenshots/ directory.
Project Structure
Todo-App-AWS-Cloud-Deployment/
│
├── architecture/
├── documentation/
├── screenshots/
└── README.md
Result
The Todo application was successfully deployed on AWS with load balancing, auto scaling, monitoring, and automated MongoDB backup.
This project demonstrates practical experience with AWS cloud infrastructure, backend deployment, networking, security, and automation.

