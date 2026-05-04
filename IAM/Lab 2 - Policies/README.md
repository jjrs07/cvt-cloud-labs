# Lab 2: IAM Policies and Least Privilege

IAM Policies are JSON documents that define "Who" can do "What" on "Which" resource.

---

## 📖 Anatomy of a Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::my-lab-bucket"
        }
    ]
}
```

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create a Custom Policy
1. Go to **IAM** -> **Policies** -> **Create policy**.
2. Select **JSON** tab and paste the following (allows ONLY listing S3 buckets):
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "s3:Get*",
                   "s3:List*"
               ],
               "Resource": "*"
           }
       ]
   }
   ```
3. Click **Next**. **Policy name:** `S3ReadOnlyLimited`.
4. Click **Create policy**.

### Step 2: Test the Policy
1. Create a new user `limited-user` and attach the `S3ReadOnlyLimited` policy directly to them.
2. Log in as `limited-user`.
3. Try to view S3 buckets (Success).
4. Try to create an S3 bucket (Fail - Access Denied).
5. Try to view EC2 instances (Fail - Access Denied).

---

## ✅ Knowledge Check
1. What does the `Effect` field do?
2. What is the **Principle of Least Privilege**?
