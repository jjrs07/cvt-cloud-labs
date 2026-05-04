# Lab 1: CloudWatch Alarms & Notifications

**Goal:** Create an automated "Watchdog" that emails you if your EC2 instance is struggling or if your bill is too high.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create an SNS Topic
1. Go to **SNS** -> **Topics** -> **Create topic**.
2. **Type:** Standard. **Name:** `MyCloudAlerts`.
3. Click **Create topic**.
4. **Create subscription:** Protocol: **Email**, Endpoint: **Your Email**.
5. **CRITICAL:** Go to your email inbox and click **Confirm Subscription**.

### Step 2: Create a CPU Utilization Alarm
1. Go to the **CloudWatch Dashboard** -> **Alarms** -> **All alarms** -> **Create alarm**.
2. **Select metric:** EC2 -> Per-Instance Metrics -> Select your instance's **CPUUtilization**.
3. **Conditions:** Whenever CPUUtilization is... **Greater than 80%**.
4. **Notification:** Select "In alarm" and pick your `MyCloudAlerts` SNS topic.
5. **Name:** `High-CPU-Alarm`.
6. Click **Create alarm**.

### Step 3: Create a Billing Alarm
1. Go to **CloudWatch** -> **Alarms** -> **Create alarm**.
2. **Select metric:** Billing -> Total Estimated Charge.
3. **Conditions:** Greater than **$5** (or any small amount).
4. **Notification:** Select your `MyCloudAlerts` SNS topic.
5. **Name:** `Budget-Alert-5USD`.
6. Click **Create alarm**.

---

## ✅ Knowledge Check
1. What is the default monitoring interval for EC2 (Standard vs. Detailed)?
2. Why is SNS required for CloudWatch to send emails?
3. What are "CloudWatch Logs" used for?

---

## 🧹 Cleanup
1. **Delete** the CloudWatch Alarms.
2. **Delete** the SNS Topic and Subscription.
