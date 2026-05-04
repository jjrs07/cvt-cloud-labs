# Lab: Amazon EFS (Elastic File System)

**Goal:** Create a shared file system (EFS) and mount it on two different EC2 instances simultaneously to demonstrate shared storage.

---

## 🎯 Objectives
- Create an Amazon EFS file system.
- Configure Security Groups for EFS access (Port 2049).
- Mount EFS on two EC2 instances.
- Verify that a file created on one instance is visible on the other.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create a Security Group for EFS
1. Go to **EC2** -> **Security Groups** -> **Create security group**.
2. **Name:** `efs-sg`
3. **VPC:** Select your lab VPC.
4. **Inbound Rule:** Add **NFS** (Port 2049) and set the Source to your EC2 Security Group (or your VPC CIDR).
5. Click **Create security group**.

### Step 2: Create the EFS File System
1. Go to the **EFS Dashboard** -> **Create file system**.
2. **Name:** `my-shared-storage`
3. **VPC:** Select your lab VPC.
4. Click **Customize**.
5. In the **Network** tab, ensure each Availability Zone uses the `efs-sg` you created.
6. Click **Create**.

### Step 3: Launch Two EC2 Instances
1. Launch two EC2 instances (Amazon Linux 2023) in the same VPC.
2. Ensure they have a Security Group that allows SSH access.

### Step 4: Mount EFS on Instance A
1. SSH into the first instance.
2. Install the EFS utilities:
   ```bash
   sudo dnf install -y amazon-efs-utils
   ```
3. Create a directory for the mount:
   ```bash
   sudo mkdir /mnt/efs
   ```
4. Get the **Attach** command from the EFS console (click your file system -> **Attach** -> **Mount via IP/DNS**). It looks like this:
   ```bash
   sudo mount -t efs -o tls fs-12345678:/ /mnt/efs
   ```
5. Create a test file:
   ```bash
   echo "Hello from Instance A!" | sudo tee /mnt/efs/shared-file.txt
   ```

### Step 5: Mount EFS on Instance B and Verify
1. SSH into the second instance.
2. Install EFS utilities and create the `/mnt/efs` directory.
3. Run the **same mount command** as in Step 4.
4. Verify the file exists:
   ```bash
   ls /mnt/efs
   cat /mnt/efs/shared-file.txt
   ```
   *You should see the message created on Instance A!*

---

## ✅ Knowledge Check
1. Can an EBS volume be attached to two instances at once? (Hint: Usually no, unless using Multi-Attach).
2. Why did we need to allow Port 2049 in the security group?
3. What is a primary use case for EFS?

---

## 🧹 Cleanup
1. **Unmount** the file system on both instances: `sudo umount /mnt/efs`.
2. **Delete** the EFS file system.
3. **Terminate** the EC2 instances.
