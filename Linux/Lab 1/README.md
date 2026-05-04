# Lab 1: Basic Linux Commands Practice

This lab introduces you to some of the most commonly used Linux commands that help you understand user context, system identity, uptime, and date/time settings.

---

## Commands You'll Practice

### `whoami`
> Shows the current logged-in user.

```bash
whoami
```

Try this:
```bash
sudo su
whoami
exit
```
---
### `hostname`
> Displays the system's hostname.

```bash
hostname
```

Useful options:
```bash
hostnamectl        # Show detailed system hostname info (works in systemd-based distros)
hostname -f        # Show the fully qualified domain name (FQDN)
```

---

### `uptime`
> Shows how long the system has been running.

```bash
uptime
```

Sample output:
```
14:52:35 up 2 hours, 3 users, load average: 0.00, 0.03, 0.01
```

Try adding:
```bash
uptime -p          # Pretty format (e.g., up 1 hour, 15 minutes)
uptime -s          # Shows system start time
```

---

### `TZ` (Timezone Setting / Variable)
> View or temporarily change the time zone for the current shell.

```bash
echo $TZ           # View current timezone variable (often empty if using system default)
timedatectl        # View system timezone info (for systemd distros)
```

Try setting a different timezone temporarily:

```bash
TZ=Asia/Manila date
TZ=UTC date
```

This only affects the current shell session.

---

### `cal`
> Displays a calendar.

```bash
cal                # Show current month
cal 2025           # Show full calendar for 2025
cal 06 2025        # Show calendar for June 2025
```

Bonus:
```bash
ncal               # Alternate calendar layout (if available)
```

---

### `id`
> Displays user ID, group ID, and group memberships.

```bash
id
```

Try:
```bash
id root            # View user ID info for another user
```

---

## Practice Tasks

Try the following on your EC2 instance or Linux terminal:

1. Print the current user
2. Check how long your machine has been running
3. View the system hostname and change it (if root)
4. Print the current date in UTC using `TZ`
5. Show the calendar for your birth month and year
6. Display your user’s UID and groups

---

## Bonus Challenge

If you're using an EC2 instance, SSH as a different user (if configured), and compare the output of `whoami`, `id`, and `hostname`.

---

## Notes

- Most commands work across distributions, but some options (like `hostnamectl` or `timedatectl`) may not exist on older/Minimal systems.
- You can check the manual page for any command using:
  ```bash
  man uptime
  ```

---

## **Happy practicing!** 🐧
