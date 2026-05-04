# Lab 1: DynamoDB with Python (Boto3)

**Goal:** Use Python to interact with a NoSQL database. You will create a "Book Inventory" table and perform CRUD operations.

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create the DynamoDB Table
1. Go to the **DynamoDB Dashboard** -> **Create table**.
2. **Table name:** `BookInventory`.
3. **Partition key:** `ISBN` (String).
4. **Sort key:** `Title` (String).
5. Leave settings as **Default** and click **Create table**.

### Step 2: The Python Script (`dynamo_lab.py`)
Ensure you have `boto3` installed (`pip install boto3`) and your AWS credentials configured.

```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('BookInventory')

# 1. ADD AN ITEM
def add_book(isbn, title, author):
    table.put_item(
        Item={
            'ISBN': isbn,
            'Title': title,
            'Author': author
        }
    )
    print(f"Added: {title}")

# 2. GET AN ITEM
def get_book(isbn, title):
    response = table.get_item(
        Key={'ISBN': isbn, 'Title': title}
    )
    return response.get('Item')

if __name__ == "__main__":
    add_book('978-01', 'Cloud Engineering 101', 'James Santos')
    book = get_book('978-01', 'Cloud Engineering 101')
    print(f"Retrieved from DB: {book}")
```

### Step 3: Run the Script
1. Run `python dynamo_lab.py`.
2. Go back to the AWS Console -> **Explore items**.
3. You will see your new book entry in the table!

---

## ✅ Knowledge Check
1. Does DynamoDB require a fixed schema like SQL?
2. What is the difference between a Partition Key and a Sort Key?
3. When would you use DynamoDB instead of RDS?

---

## 🧹 Cleanup
1. Select the `BookInventory` table in the console.
2. Click **Delete** and type `delete` to confirm.
