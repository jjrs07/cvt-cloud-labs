# 🧪 Lab 8: Managing Processes in Linux

In this lab, you'll learn to inspect and monitor system processes, generate filtered process logs, simulate CPU usage, and schedule regular process audits with cron.

---

## 🎯 Objectives

- Create a new log file named `processes.csv` that excludes root-owned and kernel threads
- Use the `top` command to monitor CPU usage in real time
- Schedule a daily cron job to generate updated audit files

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Create a Filtered Process Listing (`processes.csv`)

Generate a list of currently running processes excluding:
- The `root` user
- Kernel threads (`[` or `]` in the command column)

```bash
ps aux | awk '$1 != "root" && $11 !~ /\[|\]/' > /home/ec2-user/processes.csv
```

🔍 Verify contents:

```bash
cat /home/ec2-user/processes.csv
```

---

### 2️⃣ Monitor CPU Usage Using `top`

#### Create a sample script that consumes CPU:

```bash
cat <<EOF > /home/ec2-user/cpu_test.sh
#!/bin/bash
while :
do
  echo "CPU stress..." > /dev/null
done
EOF

chmod +x /home/ec2-user/cpu_test.sh
```

#### Run the script in the background:

```bash
./cpu_test.sh &
```

#### Open `top` to monitor:

```bash
top
```

🧹 Exit `top` by pressing `q`, and stop the test script:

```bash
pkill -f cpu_test.sh
```

---

### 3️⃣ Create a Cron Job for Daily Process Auditing

You will schedule a cron job to run daily that:
- Logs non-root, non-kernel processes
- Stores them in a timestamped CSV
- Logs all `.csv` files created to an audit file with headers

#### Open root crontab:

```bash
sudo crontab -e
```

#### Add the following job:

```
0 2 * * * ps aux | awk '$1 != "root" && $11 !~ /\[|\]/' > /home/ec2-user/processes_$(date +\%F).csv && echo "##### Process Audit Files #####" > /home/ec2-user/process_audit.log && ls /home/ec2-user/*.csv >> /home/ec2-user/process_audit.log
```

This cron job:
- Runs at 2:00 AM daily
- Creates a CSV file with the current date
- Logs all `.csv` files in `/home/ec2-user/` into `process_audit.log`

---

## 🧪 Practice Tasks

1. Generate a new `processes.csv` excluding root and kernel threads.
2. Run a CPU-intensive script and monitor it with `top`.
3. Verify the PID of the test script using `ps aux | grep cpu_test`.
4. Create and test the cron job. Review `/home/ec2-user/process_audit.log` the next day.

---

## 📌 Tips

- Use `htop` for a more interactive view (install if needed).
- Always kill CPU stress scripts when done to free system resources.
- Cron environment is limited — always use full paths in cron jobs.

---

Process mastery unlocked! ⚙️


---

## 🌟 Bonus: Use `htop` for Interactive Process Monitoring

`htop` is an enhanced alternative to `top`.

### Install `htop`:

#### Amazon Linux 2 / 2023:
```bash
sudo yum install htop -y    # Amazon Linux 2
sudo dnf install htop -y    # Amazon Linux 2023
```

#### Ubuntu:
```bash
sudo apt install htop -y
```

### Launch `htop`:

```bash
htop
```

- Use arrow keys to navigate
- F3 to search
- F9 to kill a process
- F10 to exit

---

## 🔄 Bonus: Log Rotation for Process Logs

To prevent your `.csv` files and logs from consuming too much space over time, you can set up log rotation.

### Step 1: Create a logrotate config

Create a file:

```bash
sudo nano /etc/logrotate.d/process_logs
```

Paste the following:

```
/home/ec2-user/*.csv {
    daily
    rotate 7
    compress
    missingok
    notifempty
    create 0644 ec2-user ec2-user
}
```

This will:
- Rotate logs daily
- Keep the last 7 backups
- Compress old logs
- Skip empty/missing files

---

Stay in control of your processes and logs! 🌀
