
# 07. Lambda Backup Automation

## Objective

Automate MongoDB backups from the MongoDB EC2 instance to Amazon S3
using AWS Lambda, Systems Manager (SSM), and EventBridge.

## 1. Create S3 Backup Bucket

Create an S3 bucket to store MongoDB backup files.

**Bucket:** `todo-backend-backup-2026`

---

## 2. Create Lambda Function

Create a Lambda function:

**Name:** `Todo-lambda-backup`

**Runtime:** Python

The Lambda function uses SSM to execute a backup command on the
MongoDB EC2 instance.

### Lambda Backup Code

```python
import boto3
import time

ssm = boto3.client("ssm")

INSTANCE_ID = "YOUR_MONGODB_INSTANCE_ID"
BUCKET = "YOUR_S3_BUCKET_NAME"

def lambda_handler(event, context):

    command = f"""
    rm -rf /tmp/mongodb-backup
    mkdir -p /tmp/mongodb-backup

    mongodump \
        --host 127.0.0.1 \
        --port 27017 \
        --username todo_user \
        --password 'YOUR_PASSWORD' \
        --authenticationDatabase todoapp \
        --db todoapp \
        --out /tmp/mongodb-backup

    tar -czf /tmp/mongodb-backup.tar.gz -C /tmp mongodb-backup

    aws s3 cp /tmp/mongodb-backup.tar.gz \
        s3://{BUCKET}/mongodb-backup-$(date +%Y-%m-%d-%H-%M-%S).tar.gz
    """

    response = ssm.send_command(
        InstanceIds=[INSTANCE_ID],
        DocumentName="AWS-RunShellScript",
        Parameters={
            "commands": [command]
        }
    )

    command_id = response["Command"]["CommandId"]

    for _ in range(30):
        time.sleep(2)

        result = ssm.get_command_invocation(
            CommandId=command_id,
            InstanceId=INSTANCE_ID
        )

        if result["Status"] in ["Success", "Failed", "Cancelled", "TimedOut"]:
            break

    return {
        "statusCode": 200,
        "command_status": result["Status"]
    }
Security: Never commit the real MongoDB password to GitHub. In the actual Lambda function, use a secure secret-management solution instead of hard-coding credentials.
3. Configure Lambda IAM Permissions
The Lambda execution role requires permission to send commands to the MongoDB EC2 instance and check the command status.
Required permissions:
ssm:SendCommand
ssm:GetCommandInvocation
4. Configure SSM
The MongoDB EC2 instance must:
Have the SSM Agent running.
Appear as Online in Systems Manager.
Have the required IAM permissions.
Create an SSM VPC endpoint:
com.amazonaws.ap-south-1.ssm
Allow HTTPS traffic on port 443 from the required security groups.
5. Backup Flow
EventBridge
     ↓
Lambda
     ↓
SSM SendCommand
     ↓
MongoDB EC2
     ↓
mongodump
     ↓
tar.gz backup
     ↓
Amazon S3
6. Schedule Automatic Backup
Create an EventBridge scheduled rule.
Set the Lambda function as the target:
Todo-lambda-backup
The rule automatically triggers the MongoDB backup according to the configured schedule.
7. Verify Backup
Run the Lambda function manually or wait for the EventBridge schedule.
Check:
Lambda → Monitor → Invocations
Then open:
S3 → todo-backend-backup-2026
Verify that a new backup file has been created.
Example:
mongodb-backup-2026-10-07-12-30-00.tar.gz
Result
MongoDB data is automatically backed up from the MongoDB EC2 instance and stored in Amazon S3.
The complete automation uses:
Lambda + SSM + mongodump + S3 + EventBridge

**One important change:** I used placeholders like `YOUR_MONGODB_INSTANCE_ID`, `YOUR_S3_BUCKET_NAME`, and `YOUR_PASSWORD` rather than putting your real credentials/IDs into public GitHub documentation. Your actual deployed Lambda can still contain the real configuration privately.