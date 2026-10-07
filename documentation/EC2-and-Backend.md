# 03. EC2 and Backend

## Objective

Deploy the Node.js backend on Amazon EC2 and keep it running using PM2.

## 1. Launch EC2

Create an EC2 instance with:

| Setting | Value |
|---|---|
| OS | Ubuntu |
| Instance Type | t3.micro |
| VPC | Todo-Backend-VPC |
| Subnet | Todo-Public-Subnet-2 |
| Security Group | Todo-EC2-SG |
| IAM Role | TodoEC2Role |
| Storage | 8 GiB gp3 |

## 2. Connect and Install Requirements

Connect to the EC2 instance using SSH and install:

```bash
sudo apt update
sudo apt install git -y
Install Node.js and npm, then verify:
node -v
npm -v
git --version
3. Deploy Node.js Backend
Clone the project:
git clone <repository-url>
cd <backend-folder>
npm install
Create the .env file:
MONGO_URI=<MongoDB-connection-string>
PORT=5000
Start the backend:
node server.js
Verify:
curl http://localhost:5000/
4. Configure PM2
Install PM2:
sudo npm install -g pm2
Start the backend:
pm2 start server.js --name todo-backend
Check the application:
pm2 status
Enable automatic startup:
pm2 startup
pm2 save
Result
The Node.js backend is deployed on EC2 and managed by PM2.
The backend runs on port 5000 and is ready to be connected to the MongoDB database and Application Load Balancer.

