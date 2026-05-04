# Lab 2: Route 53 (DNS Management)

**Goal:** Understand how the "Phonebook of the Internet" works by exploring Hosted Zones and DNS records.

---

## 📖 Key Concepts
- **A Record:** Maps a name (example.com) to an IPv4 address.
- **CNAME:** Maps a name to another name (blog.example.com -> example.com).
- **Alias Record:** An AWS-specific record that maps a name to an AWS resource (like an ALB or S3 Bucket).
- **TTL (Time to Live):** How long a DNS record is cached by a browser.

---

## 🛠️ Interactive Exploration

### Step 1: Explore a Public Hosted Zone (Conceptual)
If you do not own a domain, you can still view how Route 53 works:
1. Go to **Route 53** -> **Hosted zones**.
2. Click **Create hosted zone**.
3. **Domain name:** Enter a fake name (e.g., `mycloudlab.com`).
4. **Type:** Public Hosted Zone.
5. Click **Create**.

### Step 2: Create a Record
1. Click **Create record**.
2. **Record name:** `www`
3. **Record type:** `A - Routes traffic to an IPv4 address`.
4. **Value:** Enter a dummy IP like `1.2.3.4`.
5. Click **Create records**.

### Step 3: Testing with `dig`
Open your terminal (WSL or Linux) and see what happens when you query a real domain:
```bash
dig amazon.com
```
Look for the **ANSWER SECTION** to see the IP addresses.

---

## ✅ Knowledge Check
1. What happens if you set a very high TTL (e.g., 24 hours)?
2. What is the difference between a Registrar and a DNS Provider?
3. Why use an **Alias** record instead of a standard **CNAME** for AWS resources?

---

## 🧹 Cleanup
1. Select the hosted zone you created and click **Delete zone**. 
   *(Note: You must delete all custom records first before deleting the zone itself).*
