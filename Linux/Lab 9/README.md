# 🧪 Lab 9: Managing Services and Monitoring in Linux and AWS

In this lab, you'll learn how to manage services using `systemctl`, deploy a simple static website with `httpd`, and monitor system performance using `top` and **AWS CloudWatch**.

---

## 🎯 Objectives

- Install and manage the `httpd` (Apache) web server
- Check service status using `systemctl`
- Verify the web service via browser or curl
- Monitor system performance using `top`
- Set up CloudWatch monitoring for your EC2 instance

---

## 🛠️ Part 1: Deploy a Static Website Using Apache (`httpd`)

### 1️⃣ Install Apache Web Server

```bash
sudo yum update -y
sudo yum install httpd -y
```

### 2️⃣ Start and Enable the Service

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

### 3️⃣ Verify Service Status

```bash
sudo systemctl status httpd
```

Look for:
- `Active: active (running)`
- No errors in logs

---

### 4️⃣ Configure Firewall (If Not Already Open)

Open port 80 in your security group from the AWS EC2 console.

---

### 5️⃣ Create a Static Web Page

```bash
echo "<h1>Welcome to CompanyA Static Site</h1>" | sudo tee /var/www/html/index.html
```

---

### 6️⃣ Test the Web Page

Use a browser or `curl` with your EC2 instance’s public IP:

```bash
curl http://<your-public-ip>
```

---

## 📈 Part 2: Monitor EC2 Instance

### 🔍 Using `top`
---

### 🔁 Simulate CPU Usage for Monitoring

You can simulate CPU load using a stress script to test `top` and CloudWatch metrics.

#### Option A: Create a new script

```bash
cat <<EOF > /home/ec2-user/stress.sh
#!/bin/bash
while :
do
  echo "CPU stress..." > /dev/null
done
EOF

chmod +x /home/ec2-user/stress.sh
```

Run it in the background:

```bash
./stress.sh &
```

#### Option B: Reuse previous script (`cpu_test.sh`) from Lab 8

```bash
./cpu_test.sh &
```

Then monitor with:

```bash
top
```

✅ To stop it after testing:

```bash
pkill -f stress.sh
pkill -f cpu_test.sh
```


```bash
top
```

- View CPU/memory usage
- Press `q` to exit
- Sort by CPU: Press `P`
- Sort by memory: Press `M`

---

### 📊 Using AWS CloudWatch

#### 1️⃣ Confirm EC2 Monitoring is Enabled

By default, EC2 enables basic CloudWatch monitoring (5-min interval).

#### 2️⃣ View Metrics

- Go to **EC2 Console**
- Select your instance
- Click **Monitoring** tab
- View metrics like:
  - CPU Utilization
  - Network In/Out
  - Disk reads/writes

#### 3️⃣ Enable Detailed Monitoring (Optional)

- Go to **Actions > Monitor and troubleshoot > Enable detailed monitoring**

> This allows 1-minute intervals (may incur charges)

---

## 🧪 Practice Tasks

1. Deploy a basic web page and confirm it's accessible.
2. Start, stop, and restart the `httpd` service using `systemctl`:
   ```bash
   sudo systemctl stop httpd
   sudo systemctl start httpd
   sudo systemctl restart httpd
   ```
3. Check system performance using `top`.
4. View your EC2 instance metrics in AWS CloudWatch.

---

## 📌 Tips

- To troubleshoot Apache:
  ```bash
  sudo journalctl -xe
  sudo systemctl status httpd
  ```
- Use `htop` for enhanced process monitoring (if installed).
- Use CloudWatch alarms to notify you of high CPU or memory usage.

---

Master services and monitor like a pro! 🔍


---

## 🌟 Bonus: Set Up a CloudWatch Alarm for High CPU Usage

1. Go to the **CloudWatch** service in the AWS Console.
2. Click **Alarms > Create alarm**
3. Select **Metric**
   - Browse: EC2 > Per-Instance Metrics
   - Choose your instance and select **CPUUtilization**
4. Set the condition:
   - Threshold type: Static
   - Whenever **CPUUtilization is >= 80%** for 2 consecutive periods
5. Choose an action:
   - Send a notification via an **SNS topic**
   - You can create a new topic and subscribe your email
6. Name your alarm and create it.

📩 You'll receive an email when the alarm triggers. Confirm your subscription.

---

## 🔁 Bonus: Automatically Restart httpd if it Fails

Enable the `Restart=always` policy in the systemd service file:

1. Create an override file for `httpd`:

```bash
sudo systemctl edit httpd
```

2. Add the following lines:

```
[Service]
Restart=always
RestartSec=5
```

3. Save and exit the editor. Then reload the service daemon:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart httpd
```

✅ Now, if `httpd` fails, it will automatically attempt to restart after 5 seconds.

---

Stay online, stay informed! 🛠️📈
