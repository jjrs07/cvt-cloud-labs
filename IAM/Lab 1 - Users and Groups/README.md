# Lab 1: IAM Users and Groups

In this lab, you will learn how to move away from the "Root User" and create dedicated accounts for your team.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create an IAM Group
1. Go to the **IAM Dashboard** -> **User groups** -> **Create group**.
2. **Group name:** `AdminGroup`
3. **Attach permissions policies:** Search for and check `AdministratorAccess`.
4. Click **Create group**.

### Step 2: Create an IAM User
1. Go to **Users** -> **Create user**.
2. **User name:** `cloud-student-01`
3. Check **Provide user access to the AWS Management Console**.
4. Select **I want to create an IAM user**.
5. Set a custom password and uncheck "User must create a new password at next sign-in" for lab purposes.
6. Click **Next**.

### Step 3: Add User to Group
1. On the **Set permissions** page, select **Add user to group**.
2. Select the `AdminGroup` you created.
3. Click **Next** -> **Create user**.

### Step 4: Test Login
1. Copy the **Console sign-in URL** provided on the final page.
2. Open an **Incognito/Private** browser window and paste the URL.
3. Log in with `cloud-student-01` and your password.

---

## ✅ Knowledge Check
1. Why is it better to assign permissions to a Group rather than a User?
2. What is the benefit of the "Console sign-in URL"?
