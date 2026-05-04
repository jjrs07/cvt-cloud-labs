# Lab 1: CloudFront Content Delivery Network (CDN)

**Goal:** Take the S3 Static Website you created earlier and make it load faster globally by caching it at AWS Edge Locations.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create a CloudFront Distribution
1. Go to the **CloudFront Dashboard** -> **Create distribution**.
2. **Origin domain:** Select the S3 bucket you used for your static website.
3. **Origin access:** Select **Origin access control settings (recommended)**.
   - Click **Create control setting** -> **Create**.
4. **Viewer protocol policy:** Select **Redirect HTTP to HTTPS** (Security first!).
5. **Default root object:** Type `index.html`.
6. Scroll down and click **Create distribution**.

### Step 2: Update S3 Bucket Policy
1. After the distribution is created, you will see a banner: "The S3 bucket policy needs to be updated."
2. Click **Copy policy**.
3. Go to your **S3 Bucket** -> **Permissions** -> **Bucket policy** -> **Edit**.
4. Paste the policy and click **Save changes**. 
   *This allows CloudFront to read your bucket even if the bucket itself is private.*

### Step 3: Verify Global Delivery
1. Wait for the distribution status to change from "Deploying" to "Enabled" (this can take 5-10 minutes).
2. Copy the **Distribution domain name** (e.g., `d12345.cloudfront.net`).
3. Paste it into your browser.
4. Your website is now being served through a global CDN!

---

## ✅ Knowledge Check
1. What is an **Edge Location**?
2. Why is HTTPS better than HTTP for your users?
3. What is "Caching" and how does it save you money?

---

## 🧹 Cleanup
1. **Disable** the CloudFront distribution first.
2. Once disabled, you can **Delete** it.
3. (Optional) Delete the S3 bucket if you no longer need it.
