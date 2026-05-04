# 🧪 Lab 12: Software Management in Linux

In this lab, you will manage software on your Linux machine using native package managers. You'll learn how to update packages, downgrade them, and install the AWS CLI.

---

## 🎯 Objectives

1. Update your Linux system using the package manager
2. Roll back or downgrade a previously updated package
3. Install the AWS Command Line Interface (AWS CLI)

---

## 🛠️ Step-by-Step Instructions

> These examples use **Amazon Linux 2 / 2023** and **Ubuntu** commands. Choose the appropriate one for your environment.

---

### 1️⃣ Update the System

#### On Amazon Linux 2

```bash
sudo yum update -y
```

#### On Amazon Linux 2023

```bash
sudo dnf update -y
```

#### On Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2️⃣ Roll Back or Downgrade a Package

#### On Amazon Linux 2

```bash
sudo yum downgrade packagename
```

#### On Amazon Linux 2023

```bash
sudo dnf downgrade packagename
```

To list available versions:

```bash
dnf --showduplicates list packagename
```

Then specify an older version:

```bash
sudo dnf install packagename-<version>
```

#### On Ubuntu (if supported by the package)

```bash
sudo apt install packagename=version
```

List versions:

```bash
apt list -a packagename
```

---

### 3️⃣ Install the AWS CLI

#### On Amazon Linux 2 / 2023

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

#### On Ubuntu

```bash
sudo apt update
sudo apt install awscli -y
```

#### Verify the Installation

```bash
aws --version
```

Expected output:

```
aws-cli/2.x.x Python/3.x.x Linux/x86_64 source
```

---

## 🧪 Practice Tasks

1. Update your system fully using `yum`, `dnf`, or `apt`.
2. Choose a non-critical package (e.g., `htop`) and downgrade it.
3. Install AWS CLI and verify using `aws --version`.
4. Configure AWS CLI with a test IAM user using:
   ```bash
   aws configure
   ```

---

## 📌 Tips

- Use `man yum`, `man dnf`, or `man apt` to explore more options.
- Always verify compatibility when downgrading packages.
- Back up critical systems before applying large-scale updates.

---

Stay updated, stay secure! 📦🔐


---

## 🌟 Bonus Tasks

### 📁 AWS CLI Basic Usage Examples

#### 1️⃣ List S3 Buckets

```bash
aws s3 ls
```

#### 2️⃣ Upload a File to S3

```bash
aws s3 cp /path/to/localfile.txt s3://your-bucket-name/
```

#### 3️⃣ Download a File from S3

```bash
aws s3 cp s3://your-bucket-name/file.txt /path/to/save/
```

#### 4️⃣ Describe EC2 Instances

```bash
aws ec2 describe-instances
```

> ✅ These commands require your AWS CLI to be configured via `aws configure`.

---

### 🧹 Clean Up Unused Packages and Cache

#### On Amazon Linux 2

```bash
sudo yum autoremove -y
sudo yum clean all
```

#### On Amazon Linux 2023

```bash
sudo dnf autoremove -y
sudo dnf clean all
```

#### On Ubuntu

```bash
sudo apt autoremove -y
sudo apt clean
```

> Use cleanup commands regularly to free up disk space.

---

Keep your system lean and your cloud skills sharp! ☁️🧼


---

## 📜 View Installed Updates History

Tracking update history is useful for troubleshooting and auditing changes to your system.

### On Amazon Linux 2

```bash
sudo yum history
```

View detailed info for a specific transaction:

```bash
sudo yum history info <transaction_id>
```

### On Amazon Linux 2023

```bash
sudo dnf history
```

Detailed transaction info:

```bash
sudo dnf history info <transaction_id>
```

### On Ubuntu

```bash
grep "upgrade " /var/log/dpkg.log
```

Or view logs:

```bash
less /var/log/apt/history.log
```

> 🔍 Use `zcat` or `zless` for older rotated logs like `history.log.1.gz`

---

Stay informed, stay in control! 🧾🛠️
