# 06. CloudWatch and Auto Scaling

## Objective

Configure Amazon CloudWatch to monitor the backend load and trigger
Auto Scaling when the configured threshold is reached.

---

## 1. Create CloudWatch Alarm

Open:

**CloudWatch → Alarms → Create alarm**

Select the Application Load Balancer metric:

**AWS/ApplicationELB → RequestCountPerTarget**

Configure:

| Setting | Value |
|---|---|
| Metric | RequestCountPerTarget |
| Period | 1 minute |
| Statistic | Sum |
| Condition | Greater than 1 |

Create the alarm:

**Todo-Backend-Alarm**

---

## 2. Configure Scaling Policy

Open:

**EC2 → Auto Scaling Groups → Todo-Backend-ASG**

Create a scaling policy using the CloudWatch alarm.

Configure the step scaling policy:

**Todo-Backend-for-Autoscaling**

When the alarm threshold is reached, increase the Auto Scaling
Group capacity by:

**+1 instance**

---

## 3. Auto Scaling Flow

```text
User Traffic
     ↓
Application Load Balancer
     ↓
RequestCountPerTarget
     ↓
CloudWatch Alarm
     ↓
Scaling Policy
     ↓
Auto Scaling Group
     ↓
Launch Additional EC2 Instance

4. Verify Scaling
Generate requests to the application and monitor:
CloudWatch → Alarms
Verify that the alarm changes state when the configured threshold is reached.
Then check:
EC2 → Auto Scaling Groups → Todo-Backend-ASG
Verify that a new backend instance is launched when scaling is triggered.
5. Verify Recovery
After the traffic/load decreases, verify the Auto Scaling Group returns to the configured capacity according to its scaling configuration.
Check:
CloudWatch alarm state
Auto Scaling Group activity
EC2 instance count
Target Group health