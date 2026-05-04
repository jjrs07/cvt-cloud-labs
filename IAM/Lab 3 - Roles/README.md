# Lab 3: IAM Roles for EC2

**Goal:** Allow an EC2 instance to list your S3 buckets without using any access keys. This is the most secure way to handle permissions for applications.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create the IAM Role
1. Go to **IAM** -> **Roles** -> **Create role**.
2. **Trusted entity type:** AWS service.
3. **Service or use case:** **EC2**.
4. Click **Next**.
5. **Permissions:** Search for `AmazonS3ReadOnlyAccess` and check it.
6. Click **Next**. **Role name:** `EC2-S3-ReadOnly-Role`.
7. Click **Create role**.

### Step 2: Attach Role to EC2
1. Go to the **EC2 Dashboard** -> **Instances**.
2. Select your running Linux instance.
3. Click **Actions** -> **Security** -> **Modify IAM role**.
4. Select `EC2-S3-ReadOnly-Role` and click **Update IAM role**.

### Step 3: Verify on Instance
1. SSH into your EC2 instance.
2. Run the following command (no `aws configure` needed!):
   ```bash
   aws s3 ls
   ```
3. You should see a list of your buckets! The instance is using the temporary credentials provided by the Role.

---

## ✅ Knowledge Check
1. Why is using a Role more secure than putting Access Keys on an instance?
2. What service provides the temporary credentials to the instance? (Hint: Instance Metadata Service).
