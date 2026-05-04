# 🧪 Challenge Lab: Automated File Creation Script

In this challenge lab, you'll write a **Bash script** that automates the creation of uniquely named empty files. The script should be able to pick up where it left off, making it perfect for repeatable automation tasks.

---

## 🎯 Objective

Create a script that:
1. Generates **25 empty files** using the `touch` command.
2. Names the files using the format: `<yourName><number>` (e.g., `james1`, `james2`, ..., `james25`)
3. Detects the highest numbered file already present and continues numbering from there.
4. Does **not hardcode** any numbers — must determine them dynamically.
5. Displays a long listing (`ls -l`) of the directory contents after file creation.

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Create the Bash Script

```bash
nano create_files.sh
```

Paste the following script (replace `yourname` with your actual name):

```bash
#!/bin/bash

NAME="james"  # Change this to your name
DIR="."       # Current directory

# Find the highest numbered file
LAST_NUM=$(ls ${DIR}/${NAME}[0-9]* 2>/dev/null | sed -E "s/.*${NAME}([0-9]+)/\1/" | sort -n | tail -n 1)
START_NUM=$((LAST_NUM + 1))

# Create 25 new files
for ((i=START_NUM; i<START_NUM+25; i++))
do
  touch "${DIR}/${NAME}${i}"
done

# Display directory contents
ls -l ${DIR} | grep "${NAME}[0-9]"
```

---

### 2️⃣ Make It Executable

```bash
chmod +x create_files.sh
```

---

### 3️⃣ Run the Script

```bash
./create_files.sh
```

Run it **again** to test if it continues numbering properly.

---

## 🧪 Practice Tasks

1. Run the script twice and verify that the second batch starts from where the first stopped.
2. Change the `NAME` variable and repeat.
3. Modify the script to create 50 files instead of 25.
4. Redirect file list output to a log file named `file_creation.log`.

---

## 📌 Tips

- `ls james*` lets you verify file names quickly.
- `rm james*` removes all created files (use carefully).
- Script can be placed in your `$PATH` for reusability.

---

Automate smart. Track progress. Have fun scripting! 🧠💻


---

## 🌟 Bonus Challenge 2: Migrate Your WSL Linux Setup to an EC2 Instance

Assume your file automation activities were performed in **WSL (Windows Subsystem for Linux)** and now you want to migrate the environment to an EC2 instance.

---

### 🚚 Objective

Migrate your files, scripts, and environment settings from your local WSL Linux to an AWS EC2 instance.

---

### 🛠️ Step-by-Step Migration Instructions

#### 1️⃣ Launch an EC2 Instance (Amazon Linux or Ubuntu)

- Use the AWS Console to launch an EC2 instance
- Choose Amazon Linux 2 or Ubuntu
- Select a key pair and open **port 22 (SSH)**

#### 2️⃣ Locate Your WSL Files on Windows

WSL Linux files are typically stored under:

```
\wsl$\Ubuntu\home\<your_username>\
```

Copy all relevant scripts and files (e.g., `create_files.sh` and generated files).

---

#### 3️⃣ Transfer Files to EC2 Using `scp`

From your Windows terminal (PowerShell or WSL):

```bash
scp -i path/to/your-key.pem /path/to/local/files/* ec2-user@<your-ec2-ip>:/home/ec2-user/
```

For example:

```bash
scp -i ~/.ssh/mykey.pem create_files.sh ec2-user@3.122.45.67:/home/ec2-user/
```

> 📌 Use `ubuntu@<ip>` if using Ubuntu EC2 AMI

---

#### 4️⃣ Connect to Your EC2 Instance

```bash
ssh -i ~/.ssh/mykey.pem ec2-user@<your-ec2-ip>
```

---

#### 5️⃣ Run Your Script on EC2

```bash
chmod +x create_files.sh
./create_files.sh
```

Verify that your files were created correctly.

---

#### 6️⃣ Optional: Sync Entire Folder Using `rsync`

```bash
rsync -avz -e "ssh -i ~/.ssh/mykey.pem" ./ ec2-user@<your-ec2-ip>:/home/ec2-user/project_folder/
```

---

### 🧪 Practice Tasks

1. Transfer a folder from WSL to EC2 using `scp` or `rsync`
2. Recreate file automation workflows in the EC2 environment
3. Test running your backup script and logging output

---

## 📌 Tips

- Make sure your EC2 instance has appropriate permissions and ports open
- Clean up files after testing using `rm` to avoid clutter

---

From local dev to the cloud — migrate like a pro! ☁️🚀
