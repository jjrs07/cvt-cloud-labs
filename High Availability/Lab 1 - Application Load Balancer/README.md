# Lab 1: Setting up an Application Load Balancer (ALB)

In this lab, you will create an Application Load Balancer to distribute traffic between two Amazon EC2 instances located in different Availability Zones.

---

## 🎯 Objectives
- Launch two EC2 instances with a simple web server.
- Create a **Target Group**.
- Configure an **Application Load Balancer**.
- Verify traffic distribution.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Launch Two EC2 Instances
1. Navigate to the **EC2 Dashboard**.
2. Click **Launch instance**.
3. **Name:** `Web-Server-01`
4. **AMI:** Amazon Linux 2023.
5. **Instance type:** `t2.micro` (or `t3.micro`).
6. **Key pair:** Proceed without a key pair (or use an existing one).
7. **Network settings:**
   - Select a VPC.
   - Choose a Public Subnet in **Availability Zone A**.
   - **Security group:** Create a new group allowing **HTTP (Port 80)** from anywhere.
8. **Advanced details:** Paste this script in **User data**:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<h1>Hello from Web Server 01</h1>" > /var/www/html/index.html
   ```
9. Click **Launch instance**.
10. **Repeat** the steps for `Web-Server-02`, but choose a Public Subnet in **Availability Zone B** and update the user data script to say `Web Server 02`.

### Step 2: Create a Target Group
1. In the EC2 left sidebar, under **Load Balancing**, click **Target Groups**.
2. Click **Create target group**.
3. **Target type:** Instances.
4. **Name:** `my-web-targets`.
5. **Protocol/Port:** HTTP / 80.
6. Click **Next**.
7. Select both `Web-Server-01` and `Web-Server-02`.
8. Click **Include as pending below**.
9. Click **Create target group**.

### Step 3: Create the Application Load Balancer
1. In the sidebar, click **Load Balancers**.
2. Click **Create load balancer**.
3. Under **Application Load Balancer**, click **Create**.
4. **Name:** `my-alb`.
5. **Network mapping:** Select your VPC and check **both** Availability Zones used for your instances.
6. **Security groups:** Select the same security group created for the EC2 instances (ensure port 80 is open).
7. **Listeners and routing:**
   - Protocol: HTTP, Port: 80.
   - Default action: Forward to `my-web-targets`.
8. Click **Create load balancer**.

### Step 4: Verify Traffic
1. Wait for the Load Balancer state to become **Active**.
2. Copy the **DNS name** of the ALB.
3. Paste it into your browser.
4. Refresh the page several times. You should see the message alternate between "Web Server 01" and "Web Server 02".

---

## ✅ Knowledge Check
1. Why should you place EC2 instances in different Availability Zones?
2. What is the role of a Target Group?
3. Which OSI layer does an Application Load Balancer operate at?

---

## 🧹 Cleanup
1. Delete the **Load Balancer**.
2. Delete the **Target Group**.
3. Terminate both **EC2 Instances**.
