# Lab 2: Launching a Web Server in Your VPC

Now that your VPC is ready, you will launch an EC2 instance into your public subnet and verify that you can reach it via the internet.

---

## 🎯 Objectives
- Launch an EC2 instance into a custom VPC.
- Configure a **Security Group** for HTTP and SSH access.
- Install a web server (Apache) and verify connectivity.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Launch the EC2 Instance
1. Go to the **EC2 Dashboard** -> **Launch instance**.
2. **Name:** `my-web-server`
3. **AMI:** Amazon Linux 2023.
4. **Instance type:** `t2.micro` (Free Tier).
5. **Network Settings** (Click **Edit**):
   - **VPC:** Select `my-lab-vpc`.
   - **Subnet:** Select `public-subnet-01`.
   - **Auto-assign public IP:** Select **Enable** (Crucial!).
6. **Security Group:**
   - Create a new security group named `web-sg`.
   - **Rule 1 (SSH):** Port 22, Source: `My IP`.
   - **Rule 2 (HTTP):** Port 80, Source: `Anywhere (0.0.0.0/0)`.

### Step 2: Install Apache Web Server
Once the instance is "Running," connect via SSH (refer to the [Linux Module](../../Linux/README.md) if you forgot how) and run:

```bash
# Update system
sudo dnf update -y

# Install Apache
sudo dnf install -y httpd

# Start and enable Apache
sudo systemctl start httpd
sudo systemctl enable httpd

# Create a simple landing page
echo "<h1>Hello from my custom AWS VPC!</h1>" | sudo tee /var/www/html/index.html
```

### Step 3: Verify Access
1. Copy the **Public IPv4 address** of your instance from the EC2 console.
2. Paste it into your web browser.
3. You should see: **"Hello from my custom AWS VPC!"**

---

## 🔍 Troubleshooting Tips
- **Can't see the page?** Check if your Security Group allows Port 80 (HTTP).
- **Can't SSH?** Check if you enabled "Auto-assign public IP" and that your Route Table points to the IGW.
- **Still stuck?** Move to [Lab 3: Troubleshooting Tools](../Lab%203%20-%20Network%20Troubleshooting/README.md).

---

## 🧹 Cleanup
To avoid charges:
1. **Terminate** the EC2 instance.
2. **Delete** the VPC (this will automatically delete subnets, IGWs, and route tables).
