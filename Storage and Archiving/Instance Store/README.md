# Lab: EC2 Instance Store (Ephemeral Storage)

**Goal:** Understand the concept of Instance Store, how it differs from EBS, and how to identify it on a Linux instance.

---

## 📖 What is Instance Store?
Unlike EBS (which is network-attached and persistent), **Instance Store** is physically attached to the host computer. It offers **very high performance** but is **ephemeral**—meaning data is lost if the instance is stopped or terminated.

---

## 🛠️ Step-by-Step Guide

### Step 1: Choose an Instance Type with Instance Store
Most Free Tier instances (`t2.micro`, `t3.micro`) **do not** have instance store. To see this in action, you would typically need a "d" series instance (e.g., `m5d.large`, `c5d.large`) or older types like `m3.medium`.

### Step 2: Identify Instance Store on Linux
If you are logged into an instance with Instance Store, run the following command:

```bash
lsblk
```

Look for disks that are NOT your root volume. They often appear as `/dev/sdb` or `/dev/nvme1n1` and won't have a "partition" yet.

### Step 3: Testing the Ephemeral Nature (Conceptual)
If you had an instance store volume:
1. You would format it: `sudo mkfs -t xfs /dev/sdb`
2. You would mount it: `sudo mount /dev/sdb /mnt/ephemeral`
3. You would write a file: `echo "temp data" > /mnt/ephemeral/test.txt`

**What happens if you STOP the instance?**
- When you start it again, the `/mnt/ephemeral` directory will be **empty**. The data is gone forever!

---

## 📊 EBS vs. Instance Store

| Feature | EBS | Instance Store |
| :--- | :--- | :--- |
| **Persistence** | Data stays even if stopped | Data lost if stopped/terminated |
| **Speed** | Good (Network-attached) | Ultra-fast (Physically attached) |
| **Snapshots** | Supports snapshots | No native snapshots |
| **Cost** | Charged per GB | Included in instance price |

---

## ✅ Key Takeaway
Use **Instance Store** for temporary data like:
- Caches
- Buffers
- Scratch space
- Replicated data (where you don't mind losing one node)

Use **EBS** for anything you need to keep (Operating System, Databases, etc.).

---

## 🧹 Cleanup
No specific cleanup is needed if you are just observing, but remember to **Terminate** any non-free-tier instances immediately!
