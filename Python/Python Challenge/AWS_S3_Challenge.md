# 🐍 Python Cloud Challenge: AWS S3 Inventory Manager

Basic Python is great, but real Cloud Engineers use Python to automate AWS! In this challenge, you will write a script that interacts with your AWS environment using the `boto3` library.

---

## 🎯 The Mission
Your company has hundreds of S3 buckets. Your boss wants a script that:
1.  **Lists** all S3 buckets in the account.
2.  **Filters** and prints only the buckets that start with a specific prefix (e.g., `prod-`).
3.  **Logs** the total count of buckets found.

---

## 🛠️ Setup
1.  **Install Boto3:**
    ```bash
    pip install boto3
    ```
2.  **Configure Credentials:**
    Make sure you have run `aws configure` or have your environment variables set up.

---

## 💻 Starter Code (`s3_manager.py`)
Copy this into a new file and complete the missing logic:

```python
import boto3

def list_my_buckets(prefix=""):
    # 1. Initialize the S3 client
    s3 = boto3.client('s3')
    
    # 2. Call the list_buckets() method
    response = s3.list_buckets()
    
    print(f"Searching for buckets with prefix: '{prefix}'\n")
    count = 0
    
    # 3. YOUR TASK: Loop through the response and print bucket names
    # Hint: The bucket list is in response['Buckets']
    
    for bucket in response['Buckets']:
        name = bucket['Name']
        
        # 4. YOUR TASK: Add logic to filter by prefix
        if name.startswith(prefix):
            print(f" Found: {name}")
            count += 1
            
    print(f"\nTotal buckets found: {count}")

if __name__ == "__main__":
    # Example: list_my_buckets(prefix="my-lab-")
    list_my_buckets()
```

---

## 🏆 Level Up (Bonus)
If you finish early, try adding these features:
- **Delete Protection:** Modify the script so it *only* lists buckets, but warns you if a bucket is empty.
- **Export to CSV:** Save the list of bucket names into a `.csv` file using Python's built-in `csv` module.

---

## 📚 Resources
- [Boto3 S3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html)
- [Python 'startswith' Method](https://www.w3schools.com/python/ref_string_startswith.asp)
