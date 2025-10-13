# AWS EBS Hands-on Lab: Create, Attach, Mount, Snapshot, Restore

**Goal:** Practice the full lifecycle of an Amazon EBS data disk on an EC2 instance—create a volume, attach it, format and mount it, write a file, snapshot it, create a new volume from the snapshot, attach and mount the restored disk, and verify the file is still there.

> **Time:** ~30–45 minutes  
> **Level:** Beginner–Intermediate  
> **OS used in examples:** Amazon Linux 2 (works similarly on most Linux AMIs)  
> **Costs:** EBS and snapshots incur small charges while provisioned. Clean up at the end.

---

## Prerequisites

- An AWS account with permission to use EC2, EBS, and Snapshots (e.g., `ec2:*` on volumes/snapshots/instances).  
- A running **EC2 instance** (t2.micro/t3.micro or similar) in your chosen region, with SSH access. (Refer to the Linux folder [README](/Linux/README.md) for the step-by-step instructions on launching an EC2 intance) 
- Security group allowing inbound SSH (`tcp/22`) from your IP.  
- (Optional) AWS CLI v2 configured (`aws configure`) with the **same region** as your instance.

> **Tip (device names):** On Nitro-based instances (most modern types), attached EBS volumes appear as **/dev/nvme\* ** (e.g., `/dev/nvme1n1`). Don’t worry if you specified `/dev/sdf` during attach—Linux will map it to an NVMe name. Use `lsblk` to see what actually appeared.

---

## Variables to keep handy

Replace these with your own values before running commands.

```bash
# Required
REGION="ap-southeast-1"          # e.g., ap-southeast-1 (Singapore)
AZ="ap-southeast-1a"             # Must match your EC2 instance's Availability Zone
INSTANCE_ID="i-0123456789abcdef0"
VOLUME_SIZE=2                    # GiB for the first data volume
FILESYSTEM="xfs"                 # xfs or ext4 are common

# Optional names/tags
TAG_KEY="Name"
TAG_VALUE="ebs-lab-disk-1"
```

Check your instance’s AZ:

```bash
aws ec2 describe-instances \
  --region "$REGION" \
  --instance-ids "$INSTANCE_ID" \
  --query "Reservations[0].Instances[0].Placement.AvailabilityZone" \
  --output text
```

---

## Step 1 — Create an EBS Volume

**Option A: Console**
1. EC2 Console → **Elastic Block Store** → **Volumes** → **Create volume**.
2. Type: General Purpose SSD (gp3) is fine.
3. Size: `2 GiB` (or more), **Availability Zone** must match your EC2 instance’s AZ.
4. Add tag: `Name=ebs-lab-disk-1` → **Create volume**.

**Option B: AWS CLI**

```bash
VOLUME_ID=$(aws ec2 create-volume \
  --region "$REGION" \
  --availability-zone "$AZ" \
  --size $VOLUME_SIZE \
  --volume-type gp3 \
  --tag-specifications "ResourceType=volume,Tags=[{Key=$TAG_KEY,Value=$TAG_VALUE}]" \
  --query "VolumeId" --output text)

echo "Created volume: $VOLUME_ID"
```

Wait until available:

```bash
aws ec2 wait volume-available --region "$REGION" --volume-ids "$VOLUME_ID"
```

---

## Step 2 — Attach the Volume to the EC2 Instance

**Console:** Volumes → select your volume → **Actions → Attach volume** → choose your instance → device name (e.g., `/dev/sdf`) → **Attach**.

**CLI:**

```bash
aws ec2 attach-volume \
  --region "$REGION" \
  --volume-id "$VOLUME_ID" \
  --instance-id "$INSTANCE_ID" \
  --device /dev/sdf

# Wait until "in-use"
aws ec2 wait volume-in-use --region "$REGION" --volume-ids "$VOLUME_ID"
```

On the instance, confirm the new disk:

```bash
lsblk
# or
sudo nvme list   # on Nitro instances
```

You should see something like `/dev/nvme1n1` (size ~2G).

---

## Step 3 — Format and Mount the New Volume

> You can format the **whole device** (no partition) for labs.

```bash
# Replace /dev/nvme1n1 with whatever lsblk shows
DISK="/dev/nvme1n1"

# Create filesystem
if [[ "$FILESYSTEM" == "xfs" ]]; then
  sudo mkfs.xfs -f "$DISK"
else
  sudo mkfs.ext4 -F "$DISK"
fi

# Create a mount point and mount
sudo mkdir -p /data1
sudo mount "$DISK" /data1

# Verify
df -hT /data1
```

(Optional) Make it persistent across reboots by adding to `/etc/fstab`:

```bash
# Get the UUID of the device
sudo blkid "$DISK"

# Example fstab line (XFS):
# UUID=<the-uuid>  /data1  xfs  defaults,nofail  0  2

# Example fstab line (EXT4):
# UUID=<the-uuid>  /data1  ext4 defaults,nofail  0  2
```

---

## Step 4 — Create a Sample File on the Mounted Disk

```bash
echo "EBS snapshot lab file $(date)" | sudo tee /data1/hello-from-ebs.txt
sudo ls -lah /data1
sudo cat /data1/hello-from-ebs.txt
```

---

## Step 5 — Create a Snapshot of the Volume

**Console:** Volumes → select data volume → **Actions → Create snapshot** → add description → **Create snapshot**.

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

## Step 6 — Create a New Volume from the Snapshot

**Console:** Snapshots → select your snapshot → **Actions → Create volume from snapshot** → choose **same AZ** as your instance → **Create**.

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

Attach it to the same instance:

```bash
aws ec2 attach-volume \
  --region "$REGION" \
  --volume-id "$RESTORED_VOL_ID" \
  --instance-id "$INSTANCE_ID" \
  --device /dev/sdg

aws ec2 wait volume-in-use --region "$REGION" --volume-ids "$RESTORED_VOL_ID"
```

On the instance, identify the **new** device (likely `/dev/nvme2n1`):

```bash
lsblk
```

Mount it to a **different** mount point to avoid confusion:

```bash
RESTORED_DISK="/dev/nvme2n1"   # update based on lsblk output

sudo mkdir -p /data2
sudo mount "$RESTORED_DISK" /data2
df -hT /data2
```

---

## Step 7 — Verify the File Exists on the Restored Volume

```bash
sudo ls -lah /data2
sudo cat /data2/hello-from-ebs.txt
```

You should see the same file and contents created in **Step 4**. ✅

---

## (Optional) Step 8 — Clean Up

> Avoid unexpected charges by removing lab resources when done.

On the instance:

```bash
# Unmount the mount points
sudo umount /data2 || true
sudo umount /data1 || true
```

Using CLI (adjust IDs if you didn’t use variables earlier):

```bash
# Detach volumes (ignore errors if already detached)
aws ec2 detach-volume --region "$REGION" --volume-id "$RESTORED_VOL_ID"
aws ec2 detach-volume --region "$REGION" --volume-id "$VOLUME_ID"

# Wait for detachment
aws ec2 wait volume-available --region "$REGION" --volume-ids "$RESTORED_VOL_ID" "$VOLUME_ID"

# Delete volumes
aws ec2 delete-volume --region "$REGION" --volume-id "$RESTORED_VOL_ID"
aws ec2 delete-volume --region "$REGION" --volume-id "$VOLUME_ID"

# Delete snapshot
aws ec2 delete-snapshot --region "$REGION" --snapshot-id "$SNAPSHOT_ID"
```

If you added `/etc/fstab` entries, remove them before unmounting/detaching to avoid boot issues later.

---

## Troubleshooting

- **I don’t see the device I attached.**  
  Run `lsblk` and `sudo dmesg | tail -50`. On Nitro, devices show as `/dev/nvmeXn1`. Try re-attaching or using a different device letter (`/dev/sdf`, `/dev/sdg`).

- **`mount: wrong fs type` or similar.**  
  Ensure you formatted the correct disk (`mkfs.xfs` or `mkfs.ext4`) and that you mounted the **device** (e.g., `/dev/nvme1n1`), not a partition name that doesn’t exist.

- **`umount: target is busy`.**  
  Close any shells or processes using the mount, or use `sudo fuser -vm /data1` to identify, then unmount again.

- **Snapshot stuck “pending”.**  
  Wait a bit more or check the EBS **status checks** and **events**. Ensure the volume is in a healthy state before snapshotting.

---

## What You Learned

- Creating and attaching EBS volumes that match the instance’s AZ  
- Formatting, mounting, and verifying a Linux filesystem on EBS  
- Creating snapshots and restoring a volume from a snapshot  
- Safely detaching and cleaning up resources

---

### License

MIT — feel free to copy this into your repo and adapt for your students.
