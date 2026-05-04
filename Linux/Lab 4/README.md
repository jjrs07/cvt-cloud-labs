# 🧪 Lab 4: Working with the Linux File System

In this lab, you'll practice navigating and organizing a file system using basic Linux commands such as `mkdir`, `ls`, `cd`, `touch`, `cp`, `mv`, and `rm`.

---

## 🏗️ Goal: Create the following directory and file structure

```
/home/ec2-user/CompanyA/
├── Finance/
│   ├── ProfitAndLossStatements.csv
│   └── Salary.csv
├── HR/
│   ├── Assessments.csv
│   └── TrialPeriod.csv
└── Management/
    ├── Managers.csv
    └── Schedule.csv
```

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Create the root directory

```bash
mkdir /home/ec2-user/CompanyA
```

### 2️⃣ Navigate into the directory

```bash
cd /home/ec2-user/CompanyA
```

### 3️⃣ Create subdirectories

```bash
mkdir Finance HR Management
```

### 4️⃣ Create empty files using `touch`

```bash
touch Finance/ProfitAndLossStatements.csv
touch Finance/Salary.csv

touch HR/Assessments.csv
touch HR/TrialPeriod.csv

touch Management/Managers.csv
touch Management/Schedule.csv
```

---

## 📂 Navigating and Viewing the File System

### List directories and files

```bash
ls
ls -l
ls -R          # Recursive listing
```

### Move between directories

```bash
cd HR
cd ..
cd Management
```

---

## 🧾 Copying and Moving Files

### Copy a file

```bash
cp Finance/Salary.csv HR/
```

### Move a file

```bash
mv Management/Schedule.csv HR/
```

---

## ❌ Deleting Files and Directories

### Delete a file

```bash
rm HR/TrialPeriod.csv
```

### Delete an empty directory

```bash
rmdir HR
```

### Delete a directory and its contents (⚠️ Be careful!)

```bash
rm -r HR
```

---

## ✅ Practice Tasks

1. Create a folder `Reports` under `Finance` and add a file `Q2.csv` to it.
2. Copy `Managers.csv` to the `Finance` directory.
3. Move `ProfitAndLossStatements.csv` to the new `Reports` folder.
4. Delete the `TrialPeriod.csv` file.
5. List all files and folders in `CompanyA` recursively using `ls -R`.

---

## 📌 Tips

- Always double-check before using `rm -r` to avoid accidental deletions.
- Use `tab` for auto-completion of paths in the terminal.
- Use `pwd` to print your current working directory.

---

Happy organizing! 📁
