# **3-Tier Web Application Architecture on AWS using CloudFormation with SNS Notification**

## 🚀 Project Title: **3-Tier Web Application Architecture on AWS using CloudFormation with SNS Notification**


### 🎯 **Objective**

To build and automate the deployment of a **secure, scalable, and highly available 3-tier architecture** on AWS using **CloudFormation**, integrating:

* Public/Private Subnets
* EC2 Web & App Instances
* RDS (MySQL) Database
* Load Balancer
* NAT Gateway
* 📩 SNS Email Notifications for stack creation completion

---

### 🧠 **Project Overview**

This project demonstrates the use of **Infrastructure as Code (IaC)** to deploy a fully functional **3-tier architecture**, commonly used for modern web applications.

💡 Components Deployed:

* **VPC** with multiple Availability Zones
* **Public Subnets** for Load Balancer and Web Tier
* **Private Subnets** for Application Tier and Database
* **Internet Gateway** and **NAT Gateway** for secure access
* **Elastic Load Balancer** to distribute traffic to Web Instances
* **RDS MySQL Database** in isolated private subnets
* **SNS Topic** for stack creation status email notifications

This setup ensures:

* ✅ High availability
* ✅ Fault tolerance
* ✅ Secure architecture
* ✅ Cost-effective automation

---

### 🛠️ **Tech Stack**

| Service                   | Description                           |
| ------------------------- | ------------------------------------- |
| **AWS CloudFormation**    | Infrastructure as Code                |
| **EC2 (Web & App)**       | Web Server and Application Tier       |
| **RDS (MySQL)**           | Managed Relational Database           |
| **Elastic Load Balancer** | Distributes traffic to web tier       |
| **SNS**                   | Stack creation notification via email |
| **VPC, Subnets, NAT, SG** | Network & Security architecture       |

---

### ✅ **Prerequisites**

Before deploying the stack, ensure the following:

* 🔐 **AWS Account** with admin permissions (EC2, RDS, SNS, VPC, etc.)
* 🖥️ Familiarity with **CloudFormation** or basic YAML scripting
* 📧 **Verified Email** to receive SNS notifications
* 🌐 Valid **Amazon Linux 2 AMI ID** for your region (`ami-0c02fb55956c7d316`)
* 🧾 An IAM Role or User with appropriate **CloudFormation stack privileges**

---

### 🖼️ **Architecture Diagram**
![](https://github.com/gaurav3972/gaurav3972-AWS-CloudFormation-3-Tier-application/blob/main/Images/1.png)
---

## 🚀 Enhanced Step-by-Step CloudFormation Walkthrough

This enriched version explains your CloudFormation code **inline with AWS best practices**, giving extra info about each resource’s role in the architecture.
### 🔧 Step 1: Parameters – Make the Template Reusable

CloudFormation `Parameters` let you make the template **flexible and dynamic**.

```yaml
Parameters:
  VpcCidr:
    Type: String
    Default: 10.0.0.0/16
```
![](https://github.com/gaurav3972/gaurav3972-AWS-CloudFormation-3-Tier-application/blob/main/Images/Screenshot%202025-06-13%20231944.png)
🔹 Use `NoEcho: true` for sensitive parameters like passwords
🔹 You can add `AllowedPattern`, `MinLength`, and `MaxLength` to validate inputs

🟢 *Tip:* Use `AWS::SSM::Parameter::Value<String>` for pulling secrets from Parameter Store in secure setups.

---

### 🏗️ Step 2: VPC & Subnet Design – Core Network Foundation

VPC is the **isolated virtual network** where all resources live.

```yaml
MyVPC:
  Type: AWS::EC2::VPC
```

* Two **public subnets** for web-tier and load balancer
* Two **private subnets** for app-tier and RDS
* Subnets are spread across **Availability Zones** for high availability

🟢 *Tip:* Use `MapPublicIpOnLaunch: true` to auto-assign public IPs to EC2s in public subnets.

---

### 🌐 Step 3: Internet Gateway & NAT – Public & Private Traffic Routing

```yaml
InternetGateway:
  Type: AWS::EC2::InternetGateway
```

* Public subnets route through the **Internet Gateway**
* Private subnets route through the **NAT Gateway** (so they can access the internet securely)

🟢 *Tip:* Consider using **NAT Gateway in each AZ** for fault tolerance in production.

---

### 📡 Step 4: Route Tables – Traffic Flow Controllers

Each subnet must be associated with a **Route Table** to control outbound traffic.

```yaml
PublicRoute:
  DestinationCidrBlock: 0.0.0.0/0
  GatewayId: !Ref InternetGateway
```

* Public RT → Internet Gateway
* Private RT → NAT Gateway

🟢 *Tip:* Always verify the `RouteTableAssociation` to make sure subnets are correctly linked.

---

### 🔐 Step 5: Security Groups – Layered Security

```yaml
WebSG:
  Type: AWS::EC2::SecurityGroup
  Properties:
    GroupDescription: Allow HTTP from anywhere
    SecurityGroupIngress:
      - FromPort: 80
        ToPort: 80
```

* Web tier SG: Allows HTTP from the internet
* App tier SG: Allows traffic **only from Web tier SG**
* DB SG: Allows MySQL traffic only from App SG

🟢 *Tip:* Use security groups instead of IP ranges for tighter control.

---

### ⚖️ Step 6: Load Balancer – Traffic Distribution
![](https://github.com/gaurav3972/gaurav3972-AWS-CloudFormation-3-Tier-application/blob/main/Images/Screenshot%202025-06-13%20231420.png)
```yaml
LoadBalancer:
  Type: AWS::ElasticLoadBalancingV2::LoadBalancer
```

* ALB handles **external requests** and routes to web EC2
* DNS is automatically assigned
* **TargetGroup** defines which instances get traffic

🟢 *Tip:* Use **health checks** in the TargetGroup to remove unhealthy instances automatically.

---

### 🖥️ Step 7: EC2 Instances – Web & App Layers
![](https://github.com/gaurav3972/gaurav3972-AWS-CloudFormation-3-Tier-application/blob/main/Images/Screenshot%202025-06-13%20231333.png)
```yaml
WebInstance:
  SubnetId: !Ref PublicSubnet1
  SecurityGroupIds: [!Ref WebSG]
```

* **AMI** ID used is Amazon Linux 2 (`ami-0c02fb55956c7d316`)
* EC2 types are `t2.micro` for free-tier eligible usage
* Tags like `Name: WebInstance` make resources easy to identify

🟢 *Tip:* Add **UserData scripts** to auto-install Apache or run your web app during launch.

Example:

```yaml
UserData:
  Fn::Base64: !Sub |
    #!/bin/bash
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
```

---

### 🛢️ Step 8: RDS – Secure Database Tier

```yaml
MyDB:
  Type: AWS::RDS::DBInstance
  Properties:
    Engine: mysql
```

* Placed in **private subnets** for security
* `DBSubnetGroup` specifies the subnets for high availability
* `VPCSecurityGroups` lock down access to App tier only

🟢 *Tip:* Set `MultiAZ: true` and `BackupRetentionPeriod: 7` in production.

---

### 📢 Step 9: SNS Notification – Email on Stack Creation

```yaml
StackNotificationTopic:
  Type: AWS::SNS::Topic
```

* Sends email to `3972gauravpatil@gmail.com` after stack completion
* Requires email confirmation to activate the subscription

🟢 *Tip:* You can also send CloudWatch alarm alerts to the same topic.

---

### 📤 Step 10: Outputs – Stack Results

```yaml
Outputs:
  LoadBalancerDNS:
    Description: Public DNS of Load Balancer
    Value: !GetAtt LoadBalancer.DNSName
```

* `LoadBalancerDNS`: Use to test public access
* `SNSTopicArn`: Helpful for automation or integration with other stacks

🟢 *Tip:* Include RDS endpoint and EC2 public IPs for debugging purposes.

---
![](https://github.com/gaurav3972/gaurav3972-AWS-CloudFormation-3-Tier-application/blob/main/Images/Screenshot%202025-06-13%20214541.png)
## 🎯 Final Notes

### 🔄 Reusability Tips

* Move AMI ID to parameter for cross-region use
* Use `Mappings` to map AMI per region
* Add conditions to optionally deploy RDS or ALB

### 🔐 Security Considerations

* Never hardcode passwords – use SSM Parameter Store
* Keep EC2s in private subnets for app-tier if behind ALB
---
## 📋 Summary

This CloudFormation template provisions a robust 3-tier architecture on AWS, featuring a custom VPC with both public and private subnets spread across multiple Availability Zones for high availability and fault tolerance. It deploys an internet-facing Application Load Balancer to distribute incoming traffic to web-tier EC2 instances located in public subnets, while the application-tier instances and an RDS MySQL database reside securely within private subnets. Security Groups enforce strict access control between tiers, ensuring secure communication paths. Additionally, the template includes an SNS topic that sends email notifications upon successful stack creation, helping administrators stay informed about deployment status. Overall, this automated infrastructure setup follows AWS best practices for scalability, security, and ease of management.

---
Here’s a simple, clear **License** section you can add to your project documentation:

---

## 🔐 License

This project is licensed under the **MIT License** — see the [LICENSE](https://opensource.org/licenses/MIT) file for details.
You are free to use, modify, and distribute this code with proper attribution. No warranty is provided.

---
