# Lab 1: Launching a Linux Instance

## Goal
To launch your first Amazon EC2 instance using the AWS Management Console and establish a connection to it.

## Steps
1. Navigate to the **EC2 Dashboard**.
2. Click **Launch Instance**.
3. **Name:** `My-First-Server`.
4. **AMI:** Amazon Linux 2023.
5. **Instance Type:** `t2.micro` (or `t3.micro`).
6. **Key Pair:** Create a new key pair or use an existing one.
7. **Network Settings:** Ensure "Allow SSH traffic from Anywhere" is selected (for lab purposes).
8. Click **Launch**.

## Validation
- Once the instance state is "Running", select the instance and click **Connect**.
- Use **EC2 Instance Connect** to open a terminal in your browser.
- Run `uname -a` to verify you are on a Linux server.
