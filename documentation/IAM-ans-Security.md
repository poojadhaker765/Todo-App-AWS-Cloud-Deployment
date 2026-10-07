# 02. IAM and Security

## Objective

## 1. Create IAM Role
## 2. Attach EC2 Permissions
## 3. Create Backend Security Group
## 4. Create MongoDB Security Group
## 5. Create ALB Security Group
## 6. Configure Security Group Rules
## 7. Security Best Practices
## 8. Verification
## Result

# 02. IAM and Security

## Objective
Short explanation.

## 1. IAM Role
- Role name
- Trusted entity
- Policy
- Purpose
- attached to EC2

## 2. Security Groups

### ALB Security Group
| Type | Port | Source |
|---|---:|---|
| HTTP | 80 | 0.0.0.0/0 |

### Backend Security Group
| Type | Port | Source |
|---|---:|---|
| Custom TCP | 5000 | ALB Security Group |
| SSH | 22 | My IP |

### MongoDB Security Group
| Type | Port | Source |
|---|---:|---|
| Custom TCP | 27017 | Backend Security Group |

## 3. Security Architecture

Internet
↓
ALB : 80
↓
Backend : 5000
↓
MongoDB : 27017

## 4. Security Practices
- SSH restricted to My IP
- MongoDB not publicly exposed
- IAM Role used instead of access keys
- Credentials not committed to GitHub

## Result
Short conclusion.