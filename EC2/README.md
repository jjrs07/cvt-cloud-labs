# 💻 Amazon EC2 (Elastic Compute Cloud) Lab Module

Welcome to the EC2 module! This section covers the fundamental compute service of AWS. You will learn how to provision, configure, and manage virtual servers in the cloud.

## 🗺️ Lab Roadmap

| Lab | Goal | Description |
| :--- | :--- | :--- |
| [**Lab 1: Launching a Linux Instance**](./Lab%201%20-%20Launching%20a%20Linux%20Instance/README.md) | **Fundamentals** | Learn how to launch an Amazon Linux 2023 instance and connect via SSH/EC2 Instance Connect. |
| [**Lab 2: User Data & Web Servers**](./Lab%202%20-%20User%20Data%20and%20Web%20Servers/README.md) | **Automation** | Use User Data scripts to automatically install and configure an Apache web server on boot. |

---

## 🛠️ What You'll Learn
- Selecting the right **AMI (Amazon Machine Image)**.
- Choosing **Instance Types** (t2.micro/t3.micro for Free Tier).
- Configuring **Security Groups** for SSH and HTTP traffic.
- Understanding **Key Pairs** for secure access.
- Using **User Data** for automated bootstrapping.

---

## ⚠️ Cost & Safety
- **Free Tier:** Use `t2.micro` or `t3.micro` instances.
- **Cleanup:** Always **Terminate** your instances when finished. Stopping an instance still incurs costs for the attached EBS volume.
