# Lab 1: Automated Backups with AWS Backup

**Goal:** Stop taking manual snapshots! Create a policy that automatically backs up your EBS volumes every day.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create a Backup Vault
1. Go to the **AWS Backup Dashboard** -> **Backup vaults**.
2. Click **Create backup vault**.
3. **Name:** `MyCloudLabVault`.
4. Click **Create backup vault**.

### Step 2: Create a Backup Plan
1. Go to **Backup plans** -> **Create Backup plan**.
2. Select **Build a new plan**.
3. **Plan name:** `Daily-EBS-Backup`.
4. **Backup rule name:** `DailyRetain7Days`.
5. **Frequency:** Daily.
6. **Retention period:** 7 Days.
7. Click **Create plan**.

### Step 3: Assign Resources
1. In your new plan, click **Assign resources**.
2. **Resource assignment name:** `My-EBS-Volumes`.
3. **Assign by:** Resource ID.
4. **Resource type:** `EBS`.
5. Select the EBS volume you created in the Storage lab.
6. Click **Assign resources**.

### Step 4: Verify (Conceptual)
AWS Backup will now automatically trigger a snapshot at the scheduled time. You can monitor this under **Jobs** -> **Backup jobs**.

---

## ✅ Knowledge Check
1. Why is a centralized Backup Vault better than individual snapshots?
2. What is a "Point-in-Time Recovery" (PITR)?
3. If you delete an EC2 instance, what happens to the backups in the vault?

---

## 🧹 Cleanup
1. **Remove** the resource assignment from your plan.
2. **Delete** the Backup Plan.
3. **Delete** the Backup Vault (you must delete the recovery points inside first).
