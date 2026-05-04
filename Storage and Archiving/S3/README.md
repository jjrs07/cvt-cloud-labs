# Lab: Amazon S3 (Simple Storage Service)

**Goal:** Learn the fundamentals of Amazon S3, including bucket creation, object management, versioning, and hosting a static website.

---

## 🎯 Objectives
- Create an S3 Bucket with a globally unique name.
- Upload and manage objects (files).
- Enable and test **S3 Versioning**.
- Configure **Static Website Hosting**.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create an S3 Bucket
1. Go to the **S3 Dashboard** in the AWS Console.
2. Click **Create bucket**.
3. **Bucket name:** Enter a unique name (e.g., `my-restart-lab-unique-id`). 
   *Remember: S3 bucket names must be globally unique!*
4. **Region:** Select your preferred region (e.g., `us-east-1`).
5. Leave other settings as default and click **Create bucket**.

### Step 2: Upload and Manage Objects
1. Click on your new bucket name.
2. Click **Upload** -> **Add files**.
3. Select a simple text file from your computer and click **Upload**.
4. Once uploaded, click the file name to see its **Object URL**. 
   *Note: By default, this URL will return "Access Denied" because buckets are private by default.*

### Step 3: Enable S3 Versioning
*Versioning protects you from accidental deletions or overwrites.*
1. Go to the **Properties** tab of your bucket.
2. Under **Bucket Versioning**, click **Edit** -> **Enable** -> **Save changes**.
3. Now, upload a new version of the same text file (keep the filename identical).
4. Go to the **Objects** tab and toggle the **List versions** switch. You will see both versions of the file stored!

### Step 4: Static Website Hosting
1. Create a file named `index.html` on your computer:
   ```html
   <html>
     <body>
       <h1>Welcome to my S3 Static Website!</h1>
     </body>
   </html>
   ```
2. Upload `index.html` to your bucket.
3. Go to **Permissions** tab -> **Block public access (bucket settings)** -> click **Edit**.
4. Uncheck **Block all public access** and click **Save changes** (Type "confirm" to proceed).
5. Go to **Properties** tab -> **Static website hosting** -> **Edit**.
6. Select **Enable**, specify `index.html` as the index document, and click **Save changes**.
7. Go to **Permissions** tab -> **Bucket policy** -> **Edit** and paste this policy (Replace `YOUR-BUCKET-NAME`):
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
           }
       ]
   }
   ```
8. Click **Save changes**. 
9. Scroll down to the bottom of the **Properties** tab to find the **Bucket website endpoint**. Click it to see your live website!

---

## ✅ Knowledge Check
1. Why do S3 bucket names have to be globally unique?
2. What is the difference between S3 and EBS? (Hint: Object vs. Block storage).
3. How does versioning help in disaster recovery?

---

## 🧹 Cleanup
1. Go to the **Objects** tab and delete all files (including versions).
2. Go back to the S3 Dashboard, select the bucket, and click **Delete**.
