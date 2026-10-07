# 09. Testing and Results

## Objective

Verify that all components of the AWS Todo application are working
correctly after deployment.

## 1. Backend Testing

Verify the Node.js backend:

```bash
curl http://localhost:5000/
Expected result: Backend returns a successful response.

2. MongoDB Testing
Verify that:
MongoDB service is running.
todoapp database exists.
Todo data can be stored and retrieved.
Backend can connect to MongoDB.

3. Load Balancer Testing
Open the Application Load Balancer DNS name in a browser.
Verify that the backend application is accessible through the ALB.
Expected result: ALB target instances show Healthy.

4. Frontend Testing
Open the S3 frontend website.
Test the main Todo operations:
Test
Expected Result
Open application
Application loads
Add Todo
Todo is created
View Todo
Todo is displayed
Update Todo
Todo is updated
Delete Todo
Todo is deleted

5. Auto Scaling Testing
Generate application traffic and monitor the CloudWatch alarm.
Verify that:
CloudWatch alarm changes state.
Auto Scaling launches an additional EC2 instance.
New instance becomes healthy in the Target Group.

6. Backup Testing
Run the Lambda backup function.
Verify:
Lambda execution is successful.
MongoDB backup is created.
Backup file appears in the S3 backup bucket.

7. Security Testing
Verify that:
MongoDB port 27017 is not publicly accessible.
Backend port 5000 is controlled by the ALB security group.
SSH access is restricted.
IAM Role is attached to EC2.
Final Architecture
React Frontend
      ↓
Amazon S3
      ↓
Application Load Balancer
      ↓
Node.js Backend EC2
      ↓
MongoDB EC2
      ↓
MongoDB Data

CloudWatch → Auto Scaling
Lambda → SSM → MongoDB Backup → S3



Final Result
The AWS Todo application was successfully deployed and tested.
The deployment provides:
Scalable backend infrastructure
Load balancing
MongoDB database
CloudWatch monitoring
Auto Scaling
Automated MongoDB backups
S3-hosted frontend
IAM and security controls
All major application components and AWS services were verified after deployment.