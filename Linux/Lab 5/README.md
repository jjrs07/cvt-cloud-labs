# 🧪 Lab 5: Working with Files — Backups Using `tar` and `cron`

In this lab, you'll practice backing up files and directories using the `tar` command, organizing backups in a shared folder, logging your backups, and scheduling automated backups using `cron`. You'll also explore an optional challenge to back up to an S3 bucket.

---

## 🏗️ Scenario Overview

You will back up the `/home/ec2-user/CompanyA` directory created in Lab 4 and store it in a shared backup folder.

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Create a Shared Backup Directory

```bash
mkdir -p /home/ec2-user/SharedFolders/backups
```

---

### 2️⃣ Create a Tar Backup of the CompanyA Folder

```bash
cd /home/ec2-user
tar -czvf backup.CompanyA.tar.gz CompanyA/
```

- `-c` : Create archive
- `-z` : Compress with gzip
- `-v` : Verbose (shows progress)
- `-f` : Filename of the archive

---

### 3️⃣ Move the Backup File to the Backup Folder

```bash
mv backup.CompanyA.tar.gz SharedFolders/backups/
```

---

### 4️⃣ Log the Backup Details

Log format: `date, time, filename`

```bash
echo "25 Aug 25 2021, 16:59, backup.CompanyA.tar.gz" | sudo tee -a SharedFolders/backups/backup.log
```

Use this to log the actual timestamp:

```bash
echo "$(date '+%d %b %Y, %H:%M'), backup.CompanyA.tar.gz" | sudo tee -a SharedFolders/backups/backup.log
```

---

## ⏰ Schedule Daily Backup Using Cron

### 5️⃣ Edit Crontab

```bash
crontab -e
```

### 6️⃣ Add the Following Cron Job

```
0 1 * * * tar -czf /home/ec2-user/SharedFolders/backups/backup.CompanyA.$(date +\%F).tar.gz /home/ec2-user/CompanyA >> /home/ec2-user/SharedFolders/backups/cron-backup.log 2>&1
```

This will:
- Run every day at 1:00 AM
- Create a timestamped backup
- Log output to a file called `cron-backup.log`

---

## ✅ Practice Tasks

1. Perform a manual backup using `tar`.
2. Move the backup to `SharedFolders/backups`.
3. Log the backup entry.
4. Set up a cron job to perform the backup daily.
5. Confirm it runs by checking `cron-backup.log`.

---

## 🌐 Optional Challenge: Back Up to an S3 Bucket

### 1️⃣ Create an S3 bucket (if you haven't already)

```bash
aws s3 mb s3://your-bucket-name
```

### 2️⃣ Copy the backup file to S3

```bash
aws s3 cp /home/ec2-user/SharedFolders/backups/backup.CompanyA.tar.gz s3://your-bucket-name/backups/
```

> 🔐 Make sure your EC2 instance has an IAM role with S3 write permissions or configure with `aws configure`.

---

## 📌 Tips

- Use `tar -tvf` to view contents of a `.tar.gz` file.
- Keep your `backup.log` to track all backup history.
- To list cron jobs: `crontab -l`

---

Happy backing up! 🗃️
