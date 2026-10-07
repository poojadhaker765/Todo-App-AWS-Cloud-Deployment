# 08. S3 Frontend Deployment

## Objective

Deploy the React frontend application to Amazon S3 and make it
accessible through the S3 static website endpoint.

## 1. Create S3 Bucket

Open:

**AWS Console → S3 → Create bucket**

Create a bucket:

**Bucket name:** `pooja-todo-frontend-2026`

Select the required AWS Region.

For static website hosting, configure the bucket for public
website access as required.

---

## 2. Build the React Application

Open the frontend project locally.

Install dependencies:

```bash
npm install
Build the production version:
npm run build
The production files will be generated in the build output directory used by the React project.
3. Configure Backend API URL
Update the frontend API configuration to use the Application Load Balancer endpoint.
Example:
const API_URL =
  "http://Todo-Backend-ALB-696804368.ap-south-1.elb.amazonaws.com";
This allows the frontend to communicate with the deployed Node.js backend.
4. Upload Frontend Files
Open:
S3 → pooja-todo-frontend-2026
Upload the contents of the production build directory.
Make sure the main frontend file is uploaded correctly.

5. Enable Static Website Hosting
Open:
S3 → Bucket → Properties
Find:
Static website hosting
Enable it and configure:
Index document: index.html
Error document: index.html
Save the changes.

6. Configure Bucket Access
Configure the required bucket policy and public access settings so that the frontend files can be served through the S3 website endpoint.
Only the required frontend objects should be publicly readable.

7. Test the Frontend
Open the S3 static website endpoint in a browser.
Verify:
React application loads successfully.
Todo data is displayed.
Create Todo works.
Update Todo works.
Delete Todo works.
Frontend successfully communicates with the backend ALB.
Architecture
User
 ↓
Amazon S3
 ↓
React Frontend
 ↓
Application Load Balancer
 ↓
Node.js Backend
Result
The React frontend is successfully built and deployed to Amazon S3.
The frontend communicates with the backend through the Application Load Balancer.