# Lab 2: User Data and Web Servers

## Goal
To automate the installation of a web server using EC2 User Data.

## Steps
1. Launch a new EC2 instance (`Web-Server`).
2. Follow the same steps as Lab 1, but stop at **Advanced Details**.
3. Scroll down to **User Data** and paste the following script:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<h1>Hello from CVT Cloud Labs!</h1>" > /var/www/html/index.html
   ```
4. In **Security Groups**, add a rule to **Allow HTTP (Port 80)** traffic from Anywhere.
5. Launch the instance.

## Validation
- Copy the **Public IPv4 address** of your instance.
- Paste it into a new browser tab.
- You should see the message: "Hello from CVT Cloud Labs!"
