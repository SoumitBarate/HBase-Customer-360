# HBase Use Case – Customer 360 / Customer Profile

## 1. Business Problem

A telecom company has millions of customers and needs to retrieve customer information quickly.

The application should be able to search for a customer using their **Customer ID** and retrieve the following information:

- Customer ID
- Name
- City
- Mobile Number
- Plan
- Status
- Data Usage

**Why HBase?**

HBase is a distributed NoSQL database designed for handling large amounts of data and providing fast read/write operations.

By using Customer ID as the Row Key, customer information can be retrieved efficiently.

---

## 2. Architecture / Data Flow

```text
        Application
             |
             | Customer ID
             |
             v
      +------------------+
      |      HBase       |
      |                  |
      | customer_profiles|
      |                  |
      | RowKey: CUST1001 |
      | RowKey: CUST1002 |
      | RowKey: CUST1003 |
      +------------------+
             |
             v
      Fast Customer Lookup
```

---

## 3. HBase Table Design

**Table Name:** `customer_profiles`

| Row Key | Column Family | Column Qualifier |
|---|---|---|
| Customer ID | personal | name |
| Customer ID | personal | city |
| Customer ID | personal | mobile |
| Customer ID | service | plan |
| Customer ID | service | status |
| Customer ID | usage | data_usage |

### Column Families

- `personal` – Customer personal information.
- `service` – Customer plan and account status.
- `usage` – Customer data consumption.

---

## 4. HBase Commands

### Step 1: Create Table

```sql
create 'customer_profiles', 'personal', 'service', 'usage'
```

### Step 2: Insert Customer Data

```sql
put 'customer_profiles', 'CUST1001', 'personal:name', 'Rahul Sharma'
put 'customer_profiles', 'CUST1001', 'personal:city', 'Pune'
put 'customer_profiles', 'CUST1001', 'personal:mobile', '9876543210'
put 'customer_profiles', 'CUST1001', 'service:plan', '5G Premium'
put 'customer_profiles', 'CUST1001', 'service:status', 'Active'
put 'customer_profiles', 'CUST1001', 'usage:data_usage', '25GB'
```

Insert another customer:

```sql
put 'customer_profiles', 'CUST1002', 'personal:name', 'Priya Patel'
put 'customer_profiles', 'CUST1002', 'personal:city', 'Mumbai'
put 'customer_profiles', 'CUST1002', 'personal:mobile', '9876543211'
put 'customer_profiles', 'CUST1002', 'service:plan', 'Unlimited 5G'
put 'customer_profiles', 'CUST1002', 'service:status', 'Active'
put 'customer_profiles', 'CUST1002', 'usage:data_usage', '40GB'
```

### Step 3: Retrieve One Customer

```sql
get 'customer_profiles', 'CUST1001'
```

**Purpose:** Retrieve all available information for one customer using their Customer ID.

### Step 4: Retrieve Multiple Customers

```sql
scan 'customer_profiles'
```

**Purpose:** Display multiple customer records from the table.

### Step 5: Delete a Column

```sql
delete 'customer_profiles', 'CUST1001', 'personal:city'
```

**Purpose:** Delete the city information of a particular customer.

### Step 6: Delete an Entire Row

```sql
deleteall 'customer_profiles', 'CUST1002'
```

**Purpose:** Delete all column values belonging to a customer.

### Step 7: Count Customers

```sql
count 'customer_profiles'
```

**Purpose:** Count the number of rows in the table.

### Step 8: Describe Table

```sql
describe 'customer_profiles'
```

**Purpose:** Display table structure and column families.

### Step 9: Disable Table

```sql
disable 'customer_profiles'
```

**Purpose:** Disable the table before performing certain administrative operations.

### Step 10: Drop Table

```sql
drop 'customer_profiles'
```

**Purpose:** Permanently remove the table.

---

## 5. HBase Commands Summary

| Command | Purpose |
|---|---|
| `create` | Create a table |
| `put` | Insert or update data |
| `get` | Retrieve one row |
| `scan` | Retrieve multiple rows |
| `delete` | Delete a column value |
| `deleteall` | Delete an entire row |
| `count` | Count rows |
| `describe` | Display table metadata |
| `disable` | Disable a table |
| `drop` | Remove a table |

---

## 6. Expected Output

After inserting the customer records, executing:

```sql
get 'customer_profiles', 'CUST1001'
```

will return the customer's available information:

```text
CUST1001
personal:name       Rahul Sharma
personal:city       Pune
personal:mobile     9876543210
service:plan        5G Premium
service:status      Active
usage:data_usage    25GB
```

*Note: Actual HBase output also displays column timestamps and version information.*

---

## 7. Real-World Applications

This Customer 360 use case can be applied in:

- Telecom customer management systems.
- Customer profile lookup applications.
- Mobile network service platforms.
- Customer support applications.
- Real-time customer information retrieval.

## 8. Key Learning Outcomes

- Understanding HBase architecture.
- Creating tables and column families.
- Understanding Row Keys.
- Performing CRUD operations.
- Retrieving customer records using `get` and `scan`.
- Managing HBase tables.

---

**Technology:** Apache HBase  
**Domain:** Big Data / NoSQL Database  
**Use Case:** Customer 360 / Customer Profile Management
