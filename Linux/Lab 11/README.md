# 🧪 Lab 11: Bash Shell Scripts — Automating Folder Backups

In this lab, you'll create a reusable Bash script to automate the backup of a folder. You’ll build on knowledge from previous labs, including alias creation, `tar` usage, and folder/file structures.

---

## 🎯 Objective

- Create a Bash script that backs up the `/home/ec2-user/CompanyA` folder
- Store backups in `/home/ec2-user/SharedFolders/backups`
- Name backups using the current date
- Log each backup into a file called `backup.log`

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Create the Backup Script

```bash
nano /home/ec2-user/scripts/backup_companya.sh
```

Paste the following:

```bash
#!/bin/bash

# Define variables
SRC="/home/ec2-user/CompanyA"
DEST="/home/ec2-user/SharedFolders/backups"
DATE=$(date +%F_%H-%M-%S)
FILENAME="CompanyA_backup_$DATE.tar.gz"
LOGFILE="$DEST/backup.log"

# Ensure destination exists
mkdir -p $DEST

# Create backup
tar -czf $DEST/$FILENAME $SRC

# Log the backup
echo "$(date '+%Y-%m-%d %H:%M:%S'), $FILENAME" >> $LOGFILE

echo "Backup completed: $FILENAME"```

---

### 2️⃣ Make It Executable

```bash
chmod +x /home/ec2-user/scripts/backup_companya.sh
```

---

### 3️⃣ Run the Script

```bash
/home/ec2-user/scripts/backup_companya.sh
```

Check the backup:

```bash
ls -lh /home/ec2-user/SharedFolders/backups
cat /home/ec2-user/SharedFolders/backups/backup.log
```

---

### 4️⃣ Optional: Automate with Cron

Edit the crontab:

```bash
crontab -e
```

Add the following line to run daily at 2:00 AM:

```
0 2 * * * /home/ec2-user/scripts/backup_companya.sh
```

---

## 🧪 Practice Tasks

1. Modify the script to accept a custom folder as an argument.
2. Create a version that sends an email alert (using `mailx` or similar).
3. Set up a cron job to clean up backups older than 7 days.

---

## 📌 Tips

- Use `bash -x script.sh` to debug line-by-line.
- Always validate backup files with `tar -tvf`.
- Store logs in a central location for audit purposes.

---

Script your way to smarter backups! 💾
