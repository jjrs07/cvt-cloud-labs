# 🧪 Lab 13: Managing Log Files

In this lab, you’ll explore how to review and interpret important Linux log files, focusing specifically on `lastlog` and the `secure` log. These logs are crucial for auditing login activities and security-related events.

---

## 🎯 Objectives

- View user login history using `lastlog`
- Analyze authentication and access attempts using the `/var/log/secure` log
- Understand how to filter and interpret log file content

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ View Login Records with `lastlog`

The `lastlog` command displays the most recent login of all users.

```bash
lastlog
```

🔍 Look for:
- User accounts with **Never logged in**
- Last login date and time
- Source of login (IP or hostname)

---

### 2️⃣ Analyze the `/var/log/secure` Log

This log records authentication attempts, sudo activity, SSH logins, and other security events.

#### View the log with `less`

```bash
sudo less /var/log/secure
```

Use:
- `/sshd` to search for SSH login events
- `/failed` to search for failed login attempts
- `q` to quit

#### Filter with `grep`

```bash
sudo grep "sshd" /var/log/secure
sudo grep "Failed password" /var/log/secure
sudo grep "sudo" /var/log/secure
```

#### Tail the log in real time

```bash
sudo tail -f /var/log/secure
```

---

## 🧪 Practice Tasks

1. Run `lastlog` and identify users that have never logged in.
2. Use `grep` to view all SSH connection attempts in `/var/log/secure`.
3. Try a failed SSH login from another machine (if safe to do so), and verify the event appears in `secure`.
4. Find all `sudo` activity recorded in the log.
5. Monitor `/var/log/secure` live while performing login or `sudo` commands in another terminal.

---

## 📌 Tips

- `lastlog` reads from `/var/log/lastlog` (a binary file, don't open with a text editor).
- For Ubuntu, use `/var/log/auth.log` instead of `/var/log/secure`.
- You can archive logs using `logrotate` (covered in previous labs).

---

Audit like an admin, log like a legend! 📜🛡️
