# 🧪 Lab 7: Working with Linux Commands

In this lab, you'll practice using powerful Linux command-line tools to manipulate files and data. You'll explore `tee`, `sort`, `cut`, `sed`, and the pipe (`|`) operator.

---

## 🎯 Objectives

- Use the `tee` command to direct output to a file
- Use the `sort` command to reorder contents of a `.csv` file
- Use the `cut` command to extract specific fields from a file
- Use the `sed` command for stream editing
- Use the pipe `|` operator to chain commands

---

## 🛠️ Prerequisites

- Users from **Lab 2** are created (e.g., `arosalez`, `mmajor`, etc.)
- Folder structure from **Lab 4** is in place
- Create sample files inside `/home/ec2-user/CompanyA/HR/` if not present

---

## 📝 Step-by-Step Instructions

### 1️⃣ Create a Sample CSV File for Practice

```bash
cat <<EOF | sudo tee /home/ec2-user/CompanyA/HR/employees.csv
Name,Role,Salary
Jane Doe,Analyst,50000
John Smith,Manager,75000
Alice Wu,HR Specialist,60000
Bob Cruz,Recruiter,55000
EOF
```

---

### 2️⃣ Using the `tee` Command

Redirect output to a file **and** display it on screen:

```bash
echo "Backup complete for HR data" | tee /home/ec2-user/CompanyA/HR/log.txt
```

Append mode:

```bash
echo "Another log entry" | tee -a /home/ec2-user/CompanyA/HR/log.txt
```

---

### 3️⃣ Using the `sort` Command

Sort employees by name (alphabetically):

```bash
sort /home/ec2-user/CompanyA/HR/employees.csv
```

Skip header row (use `tail`):

```bash
tail -n +2 /home/ec2-user/CompanyA/HR/employees.csv | sort
```

---

### 4️⃣ Using the `cut` Command

Extract just the names (1st column):

```bash
cut -d',' -f1 /home/ec2-user/CompanyA/HR/employees.csv
```

Extract names and roles:

```bash
cut -d',' -f1,2 /home/ec2-user/CompanyA/HR/employees.csv
```

---

### 5️⃣ Using the `sed` Command

Replace all commas with a pipe symbol:

```bash
sed 's/,/|/g' /home/ec2-user/CompanyA/HR/employees.csv
```

Replace the word "Manager" with "Team Lead":

```bash
sed 's/Manager/Team Lead/g' /home/ec2-user/CompanyA/HR/employees.csv
```

---

### 6️⃣ Using Pipes (`|`)

Chain commands for powerful one-liners.

Sort by salary column and output just name and salary:

```bash
tail -n +2 /home/ec2-user/CompanyA/HR/employees.csv | sort -t',' -k3 -n | cut -d',' -f1,3
```

Log top earners to a file:

```bash
tail -n +2 /home/ec2-user/CompanyA/HR/employees.csv | sort -t',' -k3 -nr | head -n 1 | tee /home/ec2-user/CompanyA/HR/top_earner.txt
```

---

## 🧪 Practice Tasks

1. Use `tee` to log a backup operation for each department.
2. Sort each department's `.csv` file by salary or name.
3. Use `cut` to extract and display just the names of employees.
4. Replace all commas in a `.csv` with a semicolon using `sed`.
5. Combine `tail`, `sort`, and `cut` using pipes to output clean summaries.

---

## 📌 Tips

- Use `man tee`, `man sort`, etc., to learn more.
- Pipes are essential for chaining small, single-purpose commands into powerful workflows.

---

Command-line power, unlocked! 💥
