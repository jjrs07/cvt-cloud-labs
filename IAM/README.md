# 🔐 Identity and Access Management (IAM) Lab

Identity and Access Management (IAM) is the most critical service in AWS. It controls who can access your resources and what they can do with them.

## 🗺️ Lab Roadmap

| Lab | Goal | Description |
| :--- | :--- | :--- |
| [**Lab 1: Users & Groups**](./Lab%201%20-%20Users%20and%20Groups/README.md) | **Identity** | Create IAM Users, assign them to Groups, and manage passwords. |
| [**Lab 2: Policies & Permissions**](./Lab%202%20-%20Policies/README.md) | **Access Control** | Understand Managed vs. Inline Policies and the Principle of Least Privilege. |
| [**Lab 3: IAM Roles for EC2**](./Lab%203%20-%20Roles/README.md) | **Security** | Grant an EC2 instance permission to access S3 without using hardcoded keys. |

---

## 🛠️ What You'll Learn
- The difference between **Authentication** (who are you?) and **Authorization** (what can you do?).
- How to use **IAM Groups** to manage permissions at scale.
- Why **IAM Roles** are the gold standard for cross-service communication.
- Interpreting **IAM Policy Documents** (JSON).

---

## ⚠️ Important Security Note
- **Never share your Root User credentials.**
- **Never commit AWS Access Keys (`AKIA...`) to GitHub.**
- Always use **MFA (Multi-Factor Authentication)** for your users.
