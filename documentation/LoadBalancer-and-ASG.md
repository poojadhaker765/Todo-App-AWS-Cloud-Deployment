# 05. Load Balancer and Auto Scaling Group

## Objective

Configure an Application Load Balancer (ALB) to distribute traffic
across backend EC2 instances and use an Auto Scaling Group (ASG) to
maintain backend availability.

---

## 1. Create Target Group

Open:

**EC2 → Target Groups → Create target group**

Configure:

| Setting | Value |
|---|---|
| Target type | Instances |
| Protocol | HTTP |
| Port | 5000 |
| VPC | Todo-Backend-VPC |
| Health check path | `/` |

Register the backend EC2 instances as targets.

---

## 2. Create Application Load Balancer

Go to:

**EC2 → Load Balancers → Create Load Balancer**

Select:

**Application Load Balancer**

Configure:

| Setting | Value |
|---|---|
| Name | Todo-Backend-ALB |
| Scheme | Internet-facing |
| IP address type | IPv4 |
| VPC | Todo-Backend-VPC |
| Subnets | Todo-Public-Subnet-1, Todo-Public-Subnet-2 |
| Security Group | Todo-ALB-SG |

Create an HTTP listener:

**HTTP : 80 → Todo-Backend-TG-New**

---

## 3. Create Launch Template

Go to:

**EC2 → Launch Templates → Create launch template**

Configure the template with:

- Name: **Todo-Backend-TL**
- Instance type: **t3.micro**
- Key pair: **Todo-Backend-Key**
- Security Group: **Todo-EC2-SG**
- IAM Role: **TodoEC2Role**
- Root volume: **8 GiB gp3**

Use the backend EC2 configuration required to start the Node.js
application.

---

## 4. Create Auto Scaling Group

Go to:

**EC2 → Auto Scaling Groups → Create Auto Scaling Group**

Configure:

| Setting | Value |
|---|---|
| Name | Todo-Backend-ASG |
| Launch Template | Todo-Backend-TL |
| VPC | Todo-Backend-VPC |
| Subnets | Public Subnet 1 and Public Subnet 2 |

Attach the existing target group:

**Todo-Backend-TG-New**

Enable:

- EC2 health checks
- ELB health checks

Set the required minimum, desired, and maximum capacity.

---

## 5. Verify Load Balancer

Open:

**EC2 → Target Groups → Todo-Backend-TG-New → Targets**

Verify that the backend instances show:

**Healthy**

Open the ALB DNS name in a browser and verify that the backend
application is accessible.

---

## 6. Verify Auto Scaling

Check:

**EC2 → Auto Scaling Groups → Todo-Backend-ASG**

Verify that:

- Instances are running.
- Instances are registered with the target group.
- Health checks are passing.
- Instances are distributed across the configured Availability Zones.

---

## Architecture

```text
                 Internet
                    |
                    ↓
             Application Load
                Balancer
                    |
              HTTP : 80
                    |
                    ↓
            Target Group
             Port : 5000
                    |
             +------+------+
             |             |
             ↓             ↓
          EC2 #1         EC2 #2
             \             /
              \           /
               Auto Scaling
                  Group