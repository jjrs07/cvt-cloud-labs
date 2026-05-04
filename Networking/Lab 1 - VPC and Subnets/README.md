# Lab 1: Building Your Virtual Private Cloud (VPC)

In this lab, you will manually build the network foundation required to host any application on AWS.

---

## 🎯 Objectives
- Create a custom **VPC**.
- Create a **Public Subnet**.
- Attach an **Internet Gateway (IGW)**.
- Configure a **Route Table** to allow internet traffic.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create the VPC
1. Go to the **VPC Dashboard** in the AWS Console.
2. Click **Create VPC**.
3. Select **VPC only**.
4. **Name tag:** `my-lab-vpc`
5. **IPv4 CIDR block:** `10.0.0.0/16`
6. Click **Create VPC**.

### Step 2: Create a Public Subnet
1. In the left sidebar, click **Subnets** -> **Create subnet**.
2. **VPC ID:** Select `my-lab-vpc`.
3. **Subnet name:** `public-subnet-01`
4. **Availability Zone:** Choose the first one (e.g., `us-east-1a`).
5. **IPv4 CIDR block:** `10.0.1.0/24`
6. Click **Create subnet**.

### Step 3: Create and Attach an Internet Gateway (IGW)
*Without an IGW, your VPC is an isolated island with no internet access.*
1. Click **Internet gateways** -> **Create internet gateway**.
2. **Name tag:** `my-lab-igw`
3. Click **Create internet gateway**.
4. Click **Actions** -> **Attach to VPC**.
5. Select `my-lab-vpc` and click **Attach**.

### Step 4: Configure the Route Table
1. Click **Route tables**. You will see a "Main" route table created automatically.
2. Select it and click the **Routes** tab -> **Edit routes**.
3. Click **Add route**:
   - **Destination:** `0.0.0.0/0` (This means "all internet traffic")
   - **Target:** Select **Internet Gateway** and pick `my-lab-igw`.
4. Click **Save changes**.

---

## ✅ Knowledge Check
1. What does a CIDR block of `10.0.0.0/16` represent?
2. Why did we need to add `0.0.0.0/0` to the route table?
3. Is our subnet "Public" yet? 
   *Answer: Yes, because it is associated with a route table that points to an Internet Gateway!*

---

## 🚀 Next Step
Now that your network is ready, let's put something in it!
Go to [**Lab 2: Launch a Web Server**](../Lab%202%20-%20Public%20Subnet%20EC2/README.md).
