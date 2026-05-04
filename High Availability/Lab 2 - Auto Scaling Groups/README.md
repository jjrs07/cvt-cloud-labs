# Lab 2: Introduction to Auto Scaling Groups (ASG)

In this lab, you will create a Launch Template and an Auto Scaling Group to ensure that at least two web server instances are always running.

---

## 🎯 Objectives
- Create an **EC2 Launch Template**.
- Configure an **Auto Scaling Group**.
- Test the "Self-Healing" capability of ASG.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create a Launch Template
1. In the **EC2 Dashboard**, click **Launch Templates** -> **Create launch template**.
2. **Name:** `my-web-template`.
3. **AMI:** Amazon Linux 2023.
4. **Instance type:** `t2.micro`.
5. **Security groups:** Select a group that allows HTTP (Port 80).
6. **Advanced details:** Paste the following into **User data**:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<h1>Hello from ASG Instance</h1>" > /var/www/html/index.html
   ```
7. Click **Create launch template**.

### Step 2: Create an Auto Scaling Group
1. In the sidebar, click **Auto Scaling Groups** -> **Create Auto Scaling group**.
2. **Name:** `my-asg`.
3. **Launch template:** Select `my-web-template`.
4. Click **Next**.
5. **Network:** Select your VPC and at least two Public Subnets.
6. Click **Next** until you reach **Group size**.
7. **Desired capacity:** 2.
8. **Minimum capacity:** 2.
9. **Maximum capacity:** 4.
10. Click **Skip to review** and then **Create Auto Scaling group**.

### Step 3: Verify and Test Self-Healing
1. Go to **Instances**. You should see two new instances being launched by the ASG.
2. Select one of the instances and **Terminate** it.
3. Wait a few minutes. Refresh the **Instances** list.
4. Notice that the ASG automatically detects the "unhealthy" terminated instance and launches a **replacement** to maintain the desired capacity of 2!

---

## ✅ Knowledge Check
1. What is the difference between a Launch Template and an Auto Scaling Group?
2. How does an ASG know when an instance has failed?
3. What are "Scaling Policies"?

---

## 🧹 Cleanup
1. Delete the **Auto Scaling Group** (this will automatically terminate the instances).
2. Delete the **Launch Template**.
