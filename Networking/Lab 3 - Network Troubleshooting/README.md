# Lab 3: Network Troubleshooting Tools

In the real world, "it's always a networking issue." This lab teaches you the essential Linux tools to diagnose connectivity problems.

---

## 🛠️ Essential Commands

### 1. `ping` - Check Reachability
Tests if a host is alive and measures the round-trip time.
```bash
ping google.com
ping 8.8.8.8
```
*Note: Many AWS resources (like EC2) block ICMP (ping) by default in their Security Groups.*

### 2. `dig` or `nslookup` - DNS Lookup
Checks if a domain name is correctly resolving to an IP address.
```bash
dig google.com
nslookup amazon.com
```

### 3. `curl` - Web Connectivity
Tests if a web server is responding.
```bash
curl -I http://localhost
curl -v http://google.com
```

### 4. `ip addr` & `ip route` - Local Config
Check your instance's internal IP and its routing table.
```bash
ip addr show
ip route show
```

---

## 🧪 Practice Challenge: The Broken Connection

Imagine you cannot reach your web server. Run these checks in order:

1. **Local Check:** Is the service running?
   ```bash
   sudo systemctl status httpd
   ```
2. **Internal Check:** Can the server talk to itself?
   ```bash
   curl localhost:80
   ```
3. **External Check:** Use `curl -v <Public-IP>` from your local computer.
   - If it hangs: It's likely a **Security Group** or **Route Table** issue.
   - If it says "Connection Refused": The service is down or blocked by a local firewall (like `iptables`).

---

## ✅ Summary
By mastering these tools, you can quickly identify if a failure is happening at the **Application**, **OS**, or **AWS Network** level.
