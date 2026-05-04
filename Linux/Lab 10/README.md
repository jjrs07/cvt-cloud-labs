# 🧪 Lab 10: The Bash Shell

In this lab, you'll explore the power of the Bash shell by working with aliases and modifying the `PATH` environment variable. You'll also create a reusable alias for backing up an entire folder using `tar`.

---

## 🎯 Objectives

- Create and use an alias to back up a complete folder
- Understand and modify the `PATH` environment variable
- Make changes persistent across sessions

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Create an Alias to Backup a Folder

Create a shortcut alias to back up the `/home/ec2-user/CompanyA` directory.

```bash
alias backup_companya='tar -czf /home/ec2-user/CompanyA_backup_$(date +%F).tar.gz /home/ec2-user/CompanyA'
```

### Test the Alias:

```bash
backup_companya
ls -l /home/ec2-user/CompanyA_backup_*.tar.gz
```

---

### 2️⃣ Make the Alias Permanent

Add the alias to your shell profile file so it persists after reboot:

```bash
echo "alias backup_companya='tar -czf /home/ec2-user/CompanyA_backup_\$(date +%F).tar.gz /home/ec2-user/CompanyA'" >> ~/.bashrc
source ~/.bashrc
```

> ✅ Use `~/.bash_profile` on Amazon Linux 2 if `.bashrc` doesn't exist.

---

## 📦 Working with the `PATH` Variable

### 3️⃣ View the Current PATH

```bash
echo $PATH
```

This shows directories where your shell looks for executable files.

---

### 4️⃣ Create a Custom Script Folder

```bash
mkdir -p /home/ec2-user/scripts
```

Add it to your `PATH`:

```bash
export PATH=$PATH:/home/ec2-user/scripts
```

Make it permanent by adding it to your profile:

```bash
echo 'export PATH=$PATH:/home/ec2-user/scripts' >> ~/.bashrc
source ~/.bashrc
```

---

### 5️⃣ Create a Sample Script in Your New PATH

```bash
echo -e '#!/bin/bash\necho "Custom script from new PATH!"' > /home/ec2-user/scripts/hello.sh
chmod +x /home/ec2-user/scripts/hello.sh
```

Now try running it from anywhere:

```bash
hello.sh
```

---

## 🧪 Practice Tasks

1. Create an alias called `quicklist` that runs `ls -lAh`.
2. Add `/opt/tools` to your PATH (create it first if needed).
3. Create a script called `greet.sh` in your `scripts` folder that echoes "Hello from bash!"
4. Restart your terminal and test the persistence of your alias and PATH changes.

---

## 📌 Tips

- Use `alias` to view all currently defined aliases.
- Use `unalias name` to remove a temporary alias.
- Always `source ~/.bashrc` to apply changes immediately.

---

Command like a pro in the Bash shell! 🐚
