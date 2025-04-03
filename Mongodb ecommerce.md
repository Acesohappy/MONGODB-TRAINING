
---

# **Step 1: Create and Update the Database**  

### **Create Database**  
```sql
use ECommerceWebsite
```

### **Create Collections**  
```javascript
db.createCollection("Users")           
db.createCollection("Products")
db.createCollection("Orders")
db.createCollection("Payments")
```

### **Show All Collections**  
```javascript
show Collections
```
**Output:**  
```
Users  
Products  
Orders  
Payments  
```

### **Insert Data into Collections**  

#### **Users**  
```javascript
db.Users.insertMany([
  { u_id: 1, name: "Anubama", city: "Chennai" },
  { u_id: 2, name: "Deepika", city: "Chennai" },
  { u_id: 3, name: "Fanny", city: "Mumbai" },
  { u_id: 4, name: "Amirthavarshini", city: "Chennai" },
  { u_id: 5, name: "Akshitha", city: "Chennai" }
])
```

#### **Products**  
```javascript
db.Products.insertMany([
  { p_id: 1, name: "Laptop", price: 55000, category: "Electronics", stock: 10 },
  { p_id: 2, name: "Smartphone", price: 20000, category: "Electronics", stock: 15 },
  { p_id: 3, name: "Headphones", price: 3000, category: "Accessories", stock: 25 },
  { p_id: 4, name: "Backpack", price: 1200, category: "Bags", stock: 30 },
  { p_id: 5, name: "Wrist Watch", price: 5000, category: "Fashion", stock: 20 }
])
```

#### **Orders**  
```javascript
db.Orders.insertMany([
  { o_id: 101, u_id: 1, p_id: 2, qty: 1, status: "Delivered" },
  { o_id: 102, u_id: 2, p_id: 5, qty: 2, status: "Shipped" },
  { o_id: 103, u_id: 3, p_id: 3, qty: 1, status: "Pending" },
  { o_id: 104, u_id: 4, p_id: 4, qty: 1, status: "Delivered" },
  { o_id: 105, u_id: 5, p_id: 1, qty: 1, status: "Cancelled" }
])
```

#### **Payments**  
```javascript
db.Payments.insertMany([
  { pay_id: 201, o_id: 101, amount: 20000, status: "Completed", method: "Credit Card" },
  { pay_id: 202, o_id: 102, amount: 10000, status: "Processing", method: "UPI" },
  { pay_id: 203, o_id: 103, amount: 3000, status: "Pending", method: "Debit Card" },
  { pay_id: 204, o_id: 104, amount: 1200, status: "Completed", method: "Net Banking" },
  { pay_id: 205, o_id: 105, amount: 55000, status: "Refunded", method: "Credit Card" }
])
```

---

# **Step 2: Retrieve Data**  

### **Find All Users**  
```javascript
db.Users.find()
```

### **Find Orders Placed by "Anubama" (Using User ID)**  
```javascript
db.Orders.find({ u_id: 1 })
```

### **Find Payments for a Specific Order**  
```javascript
db.Payments.find({ o_id: 101 })
```

---

# **Step 3: Update Data**  

### **Update Order Status to "Delivered"**  
```javascript
db.Orders.updateOne({ o_id: 102 }, { $set: { status: "Delivered" } })
```

### **Decrease Stock After Purchase**  
```javascript
db.Products.updateOne({ p_id: 1 }, { $inc: { stock: -1 } })
```

### **Mark Payment as Completed**  
```javascript
db.Payments.updateOne({ pay_id: 203 }, { $set: { status: "Completed" } })
```

---

# **Step 4: Delete Data**  

### **Remove a User (If Needed)**  
```javascript
db.Users.deleteOne({ u_id: 1 })
```
### **Delete an Order (If Canceled)**  
```javascript
db.Orders.deleteOne({ o_id: 105 })
```

# **Step 5: Search & Filtering**  

### **Find Orders Where Status is "Pending"**  
```javascript
db.Orders.find({ status: "Pending" })
```

### **Find Products with Price Greater than 600**  
```javascript
db.Products.find({ price: { $gt: 600 } })
```
(**Note:** I corrected `db.Product` to `db.Products` to match your previous collection name.)  

### **Find Users Whose Name Starts with "A"**  
```javascript
db.Users.find({ name: /^A/ })
```
---
