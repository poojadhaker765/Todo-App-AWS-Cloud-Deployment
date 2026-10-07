# 01. Network Configuration

## Objective

The first step of the AWS deployment is to create the network
infrastructure for the application.

The network consists of:

- VPC
- Two Subnets
- Internet Gateway
- Route Table

The following steps explain how to create this network from scratch.

---

## 1. Create a VPC

### Step 1

Open the AWS Management Console and search for:

**VPC**

Open the **VPC** service.

### Step 2

From the left menu, select:

**Your VPCs → Create VPC**

### Step 3

Under **Resources to create**, select:

**VPC only**

# 01. Network Configuration

## Objective

The first step of the AWS deployment is to create the network
infrastructure for the application.

The network consists of:

- VPC
- Two Subnets
- Internet Gateway
- Route Table

The following steps explain how to create this network from scratch.

---

## 1. Create a VPC

### Step 1

Open the AWS Management Console and search for:

**VPC**

Open the **VPC** service.

### Step 2

From the left menu, select:

**Your VPCs → Create VPC**

### Step 3

Under **Resources to create**, select:

**VPC only**

### Step 4

Enter the following details:

| Setting | Value |
|---|---|
| Name tag | Todo-Backend-VPC |
| IPv4 CIDR block | 10.0.0.0/16 |
| IPv6 CIDR block | No IPv6 CIDR block |
| Tenancy | Default |

### Step 5

Click:

**Create VPC**

The VPC provides the main isolated network environment for the
application.

---

# 2. Create the First Subnet

### Step 1

From the VPC console, select:

**Subnets → Create subnet**

### Step 2

Select the VPC:

```text
Todo-Backend-VPC
Step 3
Enter:
Setting
Value
Subnet name
Todo-Public-Subnet-1
Availability Zone
ap-south-1a
IPv4 subnet CIDR block
10.0.0.0/24
Step 4
Click:
Create subnet
The first subnet is now created.
3. Create the Second Subnet
Create another subnet in a different Availability Zone.
Step 1
Go to:
VPC → Subnets → Create subnet
Step 2
Select:
VPC: Todo-Backend-VPC
Step 3
Enter:
Setting
Value
Subnet name
Todo-Public-Subnet-2
Availability Zone
ap-south-1c
IPv4 subnet CIDR block
10.0.2.0/24
Step 4
Click:
Create subnet
The VPC now contains two subnets in different Availability Zones.
Subnet Configuration
Subnet
CIDR
Availability Zone
Todo-Public-Subnet-1
10.0.0.0/24
ap-south-1a
Todo-Public-Subnet-2
10.0.2.0/24
ap-south-1c
Using different Availability Zones provides better availability and allows resources to be distributed across separate zones.
4. Create an Internet Gateway
An Internet Gateway is required to provide internet connectivity to resources in the public subnets.
Step 1
From the VPC console, select:
Internet Gateways
Step 2
Click:
Create internet gateway
Step 3
Enter:
Name tag: Todo-IGW
Step 4
Click:
Create internet gateway
5. Attach the Internet Gateway to the VPC
The Internet Gateway must be attached to the VPC.
Step 1
Select:
Todo-IGW
Step 2
Select:
Actions → Attach to a VPC
Step 3
Select:
Todo-Backend-VPC
Step 4
Click:
Attach internet gateway
The Internet Gateway is now attached to the VPC.
6. Create a Route Table
A route table controls where network traffic from the subnets is directed.
Step 1
From the VPC console, select:
Route Tables → Create route table
Step 2
Enter:
Setting
Value
Name
Todo-Public-RT
VPC
Todo-Backend-VPC
Step 3
Click:
Create route table
7. Add an Internet Route
The route table needs a default route to the Internet Gateway.
Step 1
Select:
Todo-Public-RT
Step 2
Open:
Routes → Edit routes
Step 3
Click:
Add route
Enter:
Setting
Value
Destination
0.0.0.0/0
Target
Internet Gateway
Internet Gateway
Todo-IGW
Step 4
Click:
Save changes
The route table should contain:
10.0.0.0/16 → local
0.0.0.0/0 → Todo-IGW
The 0.0.0.0/0 route allows internet-bound traffic to use the Internet Gateway.
8. Associate the Route Table with Subnet 1
Step 1
Select:
Todo-Public-RT
Step 2
Open:
Subnet associations → Edit subnet associations
Step 3
Select:
Todo-Public-Subnet-1
Step 4
Click:
Save associations
9. Associate the Route Table with Subnet 2
Repeat the same process.
Select:
Todo-Public-Subnet-2
Then click:
Save associations
The same public route table is now associated with both subnets.

10. Final Network Configuration
After completing the above steps, the network structure is:
                    Internet
                       |
                       |
                Internet Gateway
                   Todo-IGW
                       |
                       |
              Todo-Backend-VPC
                10.0.0.0/16
                       |
          +------------+------------+
          |                         |
          |                         |
   Public Subnet 1           Public Subnet 2
   10.0.0.0/24               10.0.2.0/24
   ap-south-1a               ap-south-1c
          |                         |
          +------------+------------+
                       |
                       |
                Todo-Public-RT
11. Verification
After completing the configuration, verify the following:
Todo-Backend-VPC exists.
VPC CIDR is 10.0.0.0/16.
Todo-Public-Subnet-1 exists with 10.0.0.0/24.
Todo-Public-Subnet-2 exists with 10.0.2.0/24.
The two subnets are in different Availability Zones.
Todo-IGW is attached to Todo-Backend-VPC.
Todo-Public-RT is associated with both subnets.
The route 0.0.0.0/0 → Todo-IGW is present.
