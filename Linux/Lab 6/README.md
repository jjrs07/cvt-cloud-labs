# 🧪 Lab 6: Managing File Permissions

In this lab, you'll practice managing file and folder permissions in Linux using the `chown` and `chmod` commands. You will assign ownership and control access to the `CompanyA` directory and its contents using the users created in **Lab 2** and the folder structure from **Lab 4**.

---

## 🧰 Prerequisites

Ensure you have:

- Created the `/home/ec2-user/CompanyA` folder structure (Lab 4)
- Created user accounts such as `arosalez`, `eowusu`, `mmajor`, etc. (Lab 2)

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Change Ownership of Department Folders

Assign each department folder to the corresponding user:

```bash
sudo chown arosalez:sales /home/ec2-user/CompanyA/Finance
sudo chown ljuan:hr /home/ec2-user/CompanyA/HR
sudo chown mmajor:finance /home/ec2-user/CompanyA/Finance
sudo chown mjackson:ceo /home/ec2-user/CompanyA/Management
```

> Format: `chown [user]:[group] [path]`

---

### 2️⃣ View Current Ownership and Permissions

```bash
ls -ld /home/ec2-user/CompanyA/*
```

---

### 3️⃣ Change Permissions Using `chmod`

#### Make files readable and writable by the owner, readable by the group, and not accessible to others:

```bash
chmod 640 /home/ec2-user/CompanyA/Finance/*.csv
chmod 640 /home/ec2-user/CompanyA/HR/*.csv
chmod 640 /home/ec2-user/CompanyA/Management/*.csv
```

#### Make department folders accessible only to the owner and group:

```bash
chmod 750 /home/ec2-user/CompanyA/Finance
chmod 750 /home/ec2-user/CompanyA/HR
chmod 750 /home/ec2-user/CompanyA/Management
```

---

### 4️⃣ Test Access by Switching Users

Use `su` to switch and test user access:

```bash
su - arosalez
cd /home/ec2-user/CompanyA/Finance
ls
exit
```

Try accessing another department’s folder and verify access is denied.

---

## 🧪 Practice Tasks

1. Set ownership of:
   - HR folder to `ljuan:hr`
   - Management folder to `mjackson:ceo`

2. Set file permissions so that:
   - Only the owner and group can access files
   - Others are denied access

3. Test permissions by:
   - Logging in as different users
   - Trying to read/write files in each department

---

## 🧰 Bonus: Recursively Apply Permissions

To apply permissions to folders and their contents:

```bash
sudo chmod -R 750 /home/ec2-user/CompanyA/Finance
sudo chown -R arosalez:sales /home/ec2-user/CompanyA/Finance
```

---

## 📌 Tips

- `ls -l` shows file permissions and ownership.
- `chmod u=rwx,g=rx,o= /path` is another way to set permissions.
- Use `man chmod` or `man chown` for more details.

---

Secure your files like a pro! 🔐
