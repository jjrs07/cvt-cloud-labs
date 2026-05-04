# ☁️ Networking Fundamentals Lab

Welcome to the Networking module! In these labs, you will move from manual console clicks to understanding how AWS networking components connect to form a secure and functional environment.

## 🗺️ Lab Roadmap

| Lab | Goal | Description |
| :--- | :--- | :--- |
| [**Lab 1: Build Your VPC**](./Lab%201%20-%20VPC%20and%20Subnets/README.md) | **Foundation** | Create a VPC, Subnets, and Internet Gateway from scratch. |
| [**Lab 2: Launch a Web Server**](./Lab%202%20-%20Public%20Subnet%20EC2/README.md) | **Connectivity** | Deploy an EC2 instance and configure Security Groups for web access. |
| [**Lab 3: Troubleshooting**](./Lab%203%20-%20Network%20Troubleshooting/README.md) | **Maintenance** | Use CLI tools to diagnose connectivity and DNS issues. |

---

## 🛠️ What You'll Learn
- How to design a Virtual Private Cloud (VPC).
- The difference between **Public** and **Private** subnets.
- How **Route Tables** and **Internet Gateways** enable internet access.
- Securing traffic using **Security Groups** (Stateful firewall).

---

## ⚠️ Important Note
Networking resources like VPCs and Subnets are mostly free in the AWS Free Tier, but **NAT Gateways** and **Public IPv4 addresses** can incur charges. 
- In these labs, we will use an **Internet Gateway** (Free) and a **Public Subnet** to keep costs at zero.
- Always delete your VPC and EC2 instances after finishing the labs to avoid unexpected costs.
