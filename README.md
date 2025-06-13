<<<<<<< HEAD
# gaurav3972-AWS-CloudFormation-3-Tier-application
=======
# **3-Tier Web Application Architecture on AWS using CloudFormation with SNS Notification**
Sure! Here's a **more attractive and professional version** of your documentation content, ideal for showcasing on **GitHub**, **LinkedIn**, or a portfolio\*\*. It includes emoji styling, structured markdown formatting, and crisp language:

---

## 🚀 Project Title: **3-Tier Web Application Architecture on AWS using CloudFormation with SNS Notification**

---

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

Here’s a visual representation of the deployed infrastructure:

📎 *(Click to enlarge if hosted on GitHub or portfolio)*
![3-Tier Architecture on AWS](./A_diagram_illustrates_a_three-tier_architecture_on.png)

---
>>>>>>> bc6e26d (adddd)
