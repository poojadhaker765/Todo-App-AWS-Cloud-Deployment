# 04. MongoDB Database

## Objective

Deploy MongoDB on a separate EC2 instance and connect it securely
with the Node.js backend.

## 1. Launch MongoDB EC2

Create an EC2 instance with:

| Setting | Value |
|---|---|
| OS | Ubuntu |
| Instance Type | t3.micro |
| VPC | Todo-Backend-VPC |
| Subnet | Todo-Public-Subnet-2 |
| Security Group | Todo-MongoDB-SG |
| Storage | 8 GiB gp3 |

## 2. Install MongoDB

Connect to the MongoDB EC2 instance using SSH.

Install MongoDB and verify:

```bash
mongosh --version
Start and enable MongoDB:
sudo systemctl enable mongod
sudo systemctl start mongod
Check the service:
sudo systemctl status mongod
3. Configure MongoDB
Enable authentication and configure MongoDB to accept connections from the backend server.
MongoDB uses:
Port: 27017
The MongoDB Security Group allows port 27017 only from:
Todo-EC2-SG
MongoDB is not publicly exposed.
4. Create Database and User
Create the application database:
todoapp
Create a dedicated database user:
todo_user
The user is given the required permissions for the application database.
5. Connect Backend to MongoDB
Configure the backend .env file:
MONGO_URI=<MongoDB-connection-string>
PORT=5000
The backend connects to MongoDB using the private IP address of the MongoDB EC2 instance.
6. Verify MongoDB
Check MongoDB service:
sudo systemctl status mongod
Check MongoDB connection:
mongosh
Verify the application database and collections.
Data Flow
Node.js Backend
      |
      | TCP 27017
      ↓
MongoDB EC2
      |
    todoapp
      |
    Todo Data
Result
MongoDB is successfully deployed on a separate EC2 instance.
The database is protected by a Security Group and is accessible only from the backend EC2 instances.