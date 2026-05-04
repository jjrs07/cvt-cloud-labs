# 🧪 Lab 2: Managing Users and Groups in Linux

In this lab, you will learn how to create users, manage groups, modify user and group settings, and simulate user logins in a Linux environment.

---

## 🧰 Scenario Overview

You are tasked with setting up user accounts for a fictional organization. The details of users, their roles, and the default password are provided.

### 👥 Users to Create

| First Name | Last Name | User ID     | Job Role               |
|------------|-----------|-------------|------------------------|
| Alejandro  | Rosalez   | arosalez    | Sales Manager          |
| Efua       | Owusu     | eowusu      | Shipping               |
| Jane       | Doe       | jdoe        | Shipping               |
| Li         | Juan      | ljuan       | HR Manager             |
| Mary       | Major     | mmajor      | Finance Manager        |
| Mateo      | Jackson   | mjackson    | CEO                    |
| Nikki      | Wolf      | nwolf       | Sales Representative   |
| Paulo      | Santos    | psantos     | Shipping               |
| Sofia      | Martinez  | smartinez   | HR Specialist          |
| Saanvi     | Sarkar    | ssarkar     | Finance Specialist     |

Default password for all users: `P@ssword1234!`

---

## 🏷️ Groups to Create

Create the following groups to represent departments and roles:

- sales
- hr
- finance
- personnel
- ceo
- shipping
- managers

---

## 🛠️ Lab Instructions

### 👤 View Existing Users

You can list current users on the system by checking the `/etc/passwd` file:

```bash
cut -d: -f1 /etc/passwd
```

Or filter only actual human (non-system) users by checking for UID >= 1000:

```bash
awk -F: '$3>=1000{print $1}' /etc/passwd
```

You can also use:

```bash
getent passwd
```


### 1️⃣ Create Groups

```bash
sudo groupadd sales
sudo groupadd hr
sudo groupadd finance
sudo groupadd personnel
sudo groupadd ceo
sudo groupadd shipping
sudo groupadd managers
```

---

### 2️⃣ Create Users and Set Passwords

Use the following pattern to create users:

```bash
sudo useradd -m -s /bin/bash arosalez
echo "P@ssword1234!" | sudo passwd --stdin arosalez
```

> 📝 For systems without `--stdin`, use:
```bash
echo "arosalez:P@ssword1234!" | sudo chpasswd
```

Repeat for each user using their respective User ID.

---

### 3️⃣ Add Users to Groups

Use the following command format:

```bash
sudo usermod -aG sales,managers arosalez
sudo usermod -aG shipping eowusu
sudo usermod -aG shipping jdoe
sudo usermod -aG hr,managers ljuan
sudo usermod -aG finance,managers mmajor
sudo usermod -aG ceo mjackson
sudo usermod -aG sales nwolf
sudo usermod -aG shipping psantos
sudo usermod -aG hr smartinez
sudo usermod -aG finance ssarkar
```

---

### 4️⃣ Verify Group Membership

```bash
groups arosalez
groups ljuan
```

---

### 5️⃣ Modify User Info

Change user's shell or additional settings:

```bash
sudo usermod -s /bin/zsh jdoe
sudo usermod -c "Finance Manager" mmajor
```

---

### 6️⃣ Simulate User Login

Use `su` to simulate login as a created user:

```bash
su - arosalez
whoami
exit
```

---

## ✅ Bonus Activities

- Try removing a user from a group using `gpasswd` or by editing `/etc/group`
- Lock and unlock a user account:
  ```bash
  sudo passwd -l jdoe
  sudo passwd -u jdoe
  ```

---

## 🧹 Cleanup (Optional)

To remove users and their home directories:

```bash
sudo userdel -r arosalez
```

To delete a group:

```bash
sudo groupdel sales
```

---

Happy managing! 🔐
