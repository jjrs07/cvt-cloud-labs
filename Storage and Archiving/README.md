# AWS EBS Lab: Create → Attach → Mount → Snapshot → Restore

Hands-on lab for teaching the full lifecycle of an **Amazon EBS** data disk on **EC2**.  
You will: create an EBS volume, attach it to an instance, format & mount it, write a file, take a **snapshot**, create a **new volume from that snapshot**, attach & mount it, and finally **verify the file persists**.

> **Time:** ~30–45 minutes • **Level:** Beginner–Intermediate • **Costs:** Small EBS & snapshot charges — clean up after the lab.  
> **OS in examples:** Amazon Linux 2 (commands are similar on Ubuntu; swap `yum` ↔ `apt` where relevant).

---

---

## Storage & Archiving Labs Suite (Index)

This repo will host a **3‑part lab series** under a single **Storage & Archiving** folder:

- **Lab 1 — EBS (this file):** Create → Attach → Mount → Snapshot → Restore.
- **Lab 2 — EFS (placeholder):** Shared filesystem for multiple EC2 instances. *Coming soon.*  
  - Draft file: [`efs-lab.md`](./efs-lab.md)
- **Lab 3 — S3 Backup & Archiving (placeholder):** Buckets, versioning, lifecycle policies to Glacier, and restore. *Coming soon.*  
  - Draft file: [`s3-backup-archive-lab.md`](./s3-backup-archive-lab.md)

> Plan: all three labs will live under a **`storage-and-archiving/`** folder for easy navigation in class demos.


## Table of Contents
- [Storage & Archiving Labs Suite (Index)](#storage--archiving-labs-suite-index)
  - [Lab 1 — EBS (this file)](#aws-ebs-lab-create--attach--mount--snapshot--restore)
  - [Lab 2 — EFS (Placeholder)](#lab-2--efs-placeholder)
  - [Lab 3 — S3 Backup & Archiving (Placeholder)](#lab-3--s3-backup--archiving-placeholder)

- [Prerequisites](#prerequisites)
- [Important Notes](#important-notes)
- [Quickstart (TL;DR)](#quickstart-tldr)
- [Variables](#variables)
- [Step 1 — Create an EBS Volume](#step-1--create-an-ebs-volume)
- [Step 2 — Attach the Volume to EC2](#step-2--attach-the-volume-to-ec2)
- [Step 3 — Format and Mount](#step-3--format-and-mount)
- [Step 4 — Create a Sample File](#step-4--create-a-sample-file)
- [Step 5 — Snapshot the Volume](#step-5--snapshot-the-volume)
- [Step 6 — Create Volume from Snapshot](#step-6--create-volume-from-snapshot)
- [Step 7 — Attach & Mount Restored Volume](#step-7--attach--mount-restored-volume)
- [Step 8 — Verify the File](#step-8--verify-the-file)
- [Clean Up](#clean-up)
- [Troubleshooting](#troubleshooting)
- [What You Learned](#what-you-learned)
- [License](#license)

---

## Prerequisites
- AWS account with permissions for **EC2**, **EBS**, and **Snapshots**.
- A running **EC2 instance** (e.g., t2.micro/t3.micro) with SSH access in your chosen **Region**.
- Security Group allows **SSH (TCP/22)** from your IP.
- (Optional) **AWS CLI v2** installed and configured for the same **Region** as your instance.

## Important Notes
- **AZ must match:** EBS volumes attach only within the same **Availability Zone** as your EC2 instance.
- **NVMe naming:** On Nitro-based instances (most modern types), a volume attached as `/dev/sdf` often shows up as `/dev/nvme1n1` inside Linux. Use `lsblk` to confirm real device names.
- **fstab caution:** Always use **UUIDs** in `/etc/fstab`. A wrong entry can break boot.

---

## Quickstart (TL;DR)
```bash
# 0) SSH into your EC2 first, then run the steps below as needed

# 1) Create volume (Console or CLI), attach to instance
# 2) On the instance, find device and format:
lsblk
sudo mkfs.xfs -f /dev/nvme1n1
sudo mkdir -p /data1 && sudo mount /dev/nvme1n1 /data1

# 3) Make a test file
echo "EBS snapshot lab $(date)" | sudo tee /data1/hello-from-ebs.txt

# 4) Snapshot the source volume (Console or CLI)

# 5) Create a new volume from that snapshot, attach, and mount
lsblk
sudo mkdir -p /data2 && sudo mount /dev/nvme2n1 /data2

# 6) Verify file exists on restored volume
sudo ls -lah /data2
sudo cat /data2/hello-from-ebs.txt
```

---

## Variables
Replace with your values before running CLI commands.
```bash
REGION="ap-southeast-1"          # e.g., ap-southeast-1
AZ="ap-southeast-1a"             # Must match your EC2 instance’s AZ
INSTANCE_ID="i-0123456789abcdef0"
VOLUME_SIZE=2                    # GiB for the first data volume
FILESYSTEM="xfs"                 # xfs or ext4

TAG_KEY="Name"
TAG_VALUE="ebs-lab-disk-1"
```

To confirm your instance’s Availability Zone:
```bash
aws ec2 describe-instances \
  --region "$REGION" \
  --instance-ids "$INSTANCE_ID" \
  --query "Reservations[0].Instances[0].Placement.AvailabilityZone" \
  --output text
```

---

## Step 1 — Create an EBS Volume
**Console:** EC2 → Elastic Block Store → **Volumes** → **Create volume** → Type: **gp3**, Size: `2 GiB`, **AZ:** same as instance → Tag as `Name=ebs-lab-disk-1` → **Create**.

**CLI:**
```bash
VOLUME_ID=$(aws ec2 create-volume \
  --region "$REGION" \
  --availability-zone "$AZ" \
  --size $VOLUME_SIZE \
  --volume-type gp3 \
  --tag-specifications "ResourceType=volume,Tags=[{Key=$TAG_KEY,Value=$TAG_VALUE}]" \
  --query "VolumeId" --output text)
echo "Created volume: $VOLUME_ID"

aws ec2 wait volume-available --region "$REGION" --volume-ids "$VOLUME_ID"
```

---

## Step 2 — Attach the Volume to EC2
**Console:** Volumes → select your volume → **Actions → Attach volume** → select instance → device name `/dev/sdf` → **Attach**.

**CLI:**
```bash
aws ec2 attach-volume \
  --region "$REGION" \
  --volume-id "$VOLUME_ID" \
  --instance-id "$INSTANCE_ID" \
  --device /dev/sdf

aws ec2 wait volume-in-use --region "$REGION" --volume-ids "$VOLUME_ID"
```

On the instance, validate the new device:
```bash
lsblk
# Nitro:
sudo nvme list
```

---

## Step 3 — Format and Mount
> Format the **whole device** (no partition) for simplicity.

```bash
DISK="/dev/nvme1n1"              # Replace based on lsblk/nvme list

if [[ "$FILESYSTEM" == "xfs" ]]; then
  sudo mkfs.xfs -f "$DISK"
else
  sudo mkfs.ext4 -F "$DISK"
fi

sudo mkdir -p /data1
sudo mount "$DISK" /data1
df -hT /data1
```

**Persist across reboots (optional):**
```bash
sudo blkid "$DISK"
# Add to /etc/fstab using the UUID printed above:
# UUID=<uuid>  /data1  xfs  defaults,nofail  0  2
# or for ext4:
# UUID=<uuid>  /data1  ext4 defaults,nofail  0  2
```

---

## Step 4 — Create a Sample File
```bash
echo "EBS snapshot lab file $(date)" | sudo tee /data1/hello-from-ebs.txt
sudo ls -lah /data1
sudo cat /data1/hello-from-ebs.txt
```

---

## Step 5 — Snapshot the Volume
**Console:** Volumes → select the data volume → **Actions → Create snapshot** → add description → **Create**.

**CLI:**
```bash
SNAPSHOT_ID=$(aws ec2 create-snapshot \
  --region "$REGION" \
  --volume-id "$VOLUME_ID" \
  --description "Snapshot of $VOLUME_ID for EBS lab" \
  --query "SnapshotId" --output text)
echo "Created snapshot: $SNAPSHOT_ID"

aws ec2 wait snapshot-completed --region "$REGION" --snapshot-ids "$SNAPSHOT_ID"
```

---

## Step 6 — Create Volume from Snapshot
**Console:** Snapshots → select your snapshot → **Actions → Create volume from snapshot** → choose the **same AZ** → **Create**.

**CLI:**
```bash
RESTORED_VOL_ID=$(aws ec2 create-volume \
  --region "$REGION" \
  --availability-zone "$AZ" \
  --snapshot-id "$SNAPSHOT_ID" \
  --volume-type gp3 \
  --tag-specifications "ResourceType=volume,Tags=[{Key=$TAG_KEY,Value=ebs-lab-disk-1-restored}]" \
  --query "VolumeId" --output text)
echo "Created restored volume: $RESTORED_VOL_ID"

aws ec2 wait volume-available --region "$REGION" --volume-ids "$RESTORED_VOL_ID"
```

---

## Step 7 — Attach & Mount Restored Volume
Attach to the same instance:
```bash
aws ec2 attach-volume \
  --region "$REGION" \
  --volume-id "$RESTORED_VOL_ID" \
  --instance-id "$INSTANCE_ID" \
  --device /dev/sdg

aws ec2 wait volume-in-use --region "$REGION" --volume-ids "$RESTORED_VOL_ID"
```

On the instance, identify the **new** device (likely `/dev/nvme2n1`) and mount to a different path:
```bash
lsblk
RESTORED_DISK="/dev/nvme2n1"     # update based on lsblk output
sudo mkdir -p /data2
sudo mount "$RESTORED_DISK" /data2
df -hT /data2
```

---

## Step 8 — Verify the File
```bash
sudo ls -lah /data2
sudo cat /data2/hello-from-ebs.txt
```
You should see the same contents from Step 4. ✅

---

## Clean Up
```bash
# On the instance
sudo umount /data2 || true
sudo umount /data1 || true

# Detach
aws ec2 detach-volume --region "$REGION" --volume-id "$RESTORED_VOL_ID"
aws ec2 detach-volume --region "$REGION" --volume-id "$VOLUME_ID"
aws ec2 wait volume-available --region "$REGION" --volume-ids "$RESTORED_VOL_ID" "$VOLUME_ID"

# Delete
aws ec2 delete-volume --region "$REGION" --volume-id "$RESTORED_VOL_ID"
aws ec2 delete-volume --region "$REGION" --volume-id "$VOLUME_ID"
aws ec2 delete-snapshot --region "$REGION" --snapshot-id "$SNAPSHOT_ID"
```

If you edited `/etc/fstab`, remove those entries before detaching to avoid boot issues.

---

## Troubleshooting
- **Device not showing up:** `lsblk`, `sudo dmesg | tail -50`. On Nitro, expect `/dev/nvmeXn1`. Try reattach with another letter (`/dev/sdf`, `/dev/sdg`).
- **Wrong FS type / mount error:** Ensure you formatted the correct device and used `mkfs.xfs` or `mkfs.ext4` before mounting.
- **`umount: target is busy`:** Close shells/programs using the mount. Use `sudo fuser -vm /data1` to find processes, then unmount again.
- **Snapshot stuck pending:** Wait or check EBS events/health; don’t snapshot a volume with issues.

---

## What You Learned
- Matching EBS volume **AZ** to the instance
- Creating, attaching, formatting, and mounting EBS on Linux
- Taking **snapshots** and restoring a **new volume** from a snapshot
- Verifying **data persistence** across volumes
- Cleaning up to avoid extra costs

---

## License
MIT — feel free to adapt for your classes and labs.

---

## Lab 2 — EFS (Placeholder)

**Goal:** Set up Amazon EFS, mount it on one or more EC2 instances, write/read shared files, and explore performance modes.

**Draft Outline:**
- Create EFS in the same VPC/Subnets as your EC2 instances
- Security Groups (NFS 2049) and mount targets
- Mount helper on Amazon Linux/Ubuntu
- Basic read/write and permissions test
- Performance & throughput modes overview
- **Cleanup** steps

**Draft File:** [`storage-and-archiving/efs-lab.md`](./storage-and-archiving/efs-lab.md) *(to be added)*


---

## Lab 3 — S3 Backup & Archiving (Placeholder)

**Goal:** Use Amazon S3 for backups with **Versioning**, **Lifecycle** to **Glacier** storage classes, and object **Restore** flow.

**Draft Outline:**
- Create S3 bucket (naming, region)
- Enable **Versioning** and optional **Default encryption**
- Upload test data and simulate deletes/overwrites
- Configure **Lifecycle** rules to move older versions to **Glacier Instant Retrieval / Flexible Retrieval / Deep Archive**
- Start an **Object Restore** and verify data access windows
- (Optional) **S3 Glacier Vault Lock**/compliance notes
- **Cleanup** steps

**Draft File:** [`storage-and-archiving/s3-backup-archive-lab.md`](./storage-and-archiving/s3-backup-archive-lab.md) *(to be added)*

