
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

# **Step 6: Advanced Queries**  

### **Find with Projection**  
_Show only `name` and `age`, exclude `_id`_  
```javascript
db.Users.find({}, { name: 1, city: 1})
```

### **Update Multiple Documents**  
_Increase `age` by 1 for all users_  
```javascript
db.Users.updateMany({}, { $inc: { age: 1 } })
```

### **Replace an Entire Document**  
_Replace a document where `name` is "Bob"_  
```javascript
db.Users.replaceOne({ name: "Fanny" }, { name: "Anubama", age: 30, city: "Berlin" })
```

### **Delete Multiple Documents**  
_Delete users whose `age` is less than 27_  
```javascript
db.Users.deleteMany({ age: { $lt: 27 } })
```

### **Delete All Documents in a Collection**  
```javascript
db.Users.deleteMany({})
```

---

# **Step 7: Filtering with Conditions**  

### **Find Users with Age Between 25 and 30**  
```javascript
db.Users.find({ age: { $gte: 25, $lte: 30 } })
```

### **Find Users Whose Name is Either "Alice" or "Bob"**  
```javascript
db.Users.find({ name: { $in: ["Alice", "Bob"] } })
```

### **Find Users Whose Name is Not "Alice"**  
```javascript
db.Users.find({ name: { $ne: "Alice" } })
```

### **Find Users Where `city` is Not Specified**  
```javascript
db.Users.find({ city: { $exists: false } })
```

### **Find Users Whose `city` Contains "on" (Case-Insensitive)**  
```javascript
db.Users.find({ city: { $regex: "on", $options: "i" } })
```

# **Step 8: Aggregation Queries (Fixed Output Format)**  

## **1. Join Collections (`$lookup`)**  
_Join `Orders` with `Users` to get order details along with user information._  
```javascript
db.Orders.aggregate([
    {
        $lookup: {
            from: "Users",    
            localField: "u_id",  
            foreignField: "u_id",  
            as: "user_info"  
        }
    },
    { $unwind: "$user_info" },
    {
        $project: {
            _id: 1,
            "user_info.name": 1,
            p_id: 1,
            qty: 1,
            status: 1
        }
    }
])
```
### **Output:**
```js
[
  { _id: ObjectId("67ee125427aa04639ab71240"), p_id: 2, qty: 1, status: "Delivered", user_info: { name: "Anubama" } },
  { _id: ObjectId("67ee125427aa04639ab71241"), p_id: 5, qty: 2, status: "Delivered", user_info: { name: "Deepika" } },
  { _id: ObjectId("67ee125427aa04639ab71243"), p_id: 4, qty: 1, status: "Delivered", user_info: { name: "Amirthavarshini" } },
  { _id: ObjectId("67ee3a3f13d701314cb71236"), p_id: 5, qty: 3, status: "Pending", user_info: { name: "Akshitha" } }
]
```

---

## **2. Group By (`$group`)**  
_Get the total number of orders placed by each user._  
```javascript
db.Orders.aggregate([
    {
        $group: {
            _id: "$u_id",  
            totalOrders: { $sum: 1 }  
        }
    }
])
```
### **Output:**
```js
[
  { _id: 2, totalOrders: 1 },
  { _id: 3, totalOrders: 1 },
  { _id: 1, totalOrders: 1 },
  { _id: 4, totalOrders: 1 },
  { _id: 5, totalOrders: 1 }
]
```

---

## **3. Sorting (`$sort`)**  
_Sort orders by quantity in descending order._  
```javascript
db.Orders.aggregate([
    { $sort: { qty: -1 } }  
])
```
### **Output:**
```js
[
  { _id: ObjectId("67ee3a3f13d701314cb71236"), o_id: 105, u_id: 5, p_id: 5, qty: 3, status: "Pending" },
  { _id: ObjectId("67ee125427aa04639ab71241"), o_id: 102, u_id: 2, p_id: 5, qty: 2, status: "Delivered" },
  { _id: ObjectId("67ee125427aa04639ab71240"), o_id: 101, u_id: 1, p_id: 2, qty: 1, status: "Delivered" },
  { _id: ObjectId("67ee125427aa04639ab71242"), o_id: 103, u_id: 3, p_id: 3, qty: 1, status: "Pending" },
  { _id: ObjectId("67ee125427aa04639ab71243"), o_id: 104, u_id: 4, p_id: 4, qty: 1, status: "Delivered" }
]
```

---

## **4. Combining Aggregation Operators**  
_Get the total revenue generated from each product._  
```javascript
db.Orders.aggregate([
    {
        $lookup: {
            from: "Products",
            localField: "p_id",
            foreignField: "p_id",
            as: "product_details"
        }
    },
    { $unwind: "$product_details" },
    {
        $group: {
            _id: "$p_id",
            totalRevenue: { $sum: { $multiply: ["$qty", "$product_details.price"] } }
        }
    },
    { $sort: { totalRevenue: -1 } }  
])
```
### **Output:**
```js
[
  { _id: 5, totalRevenue: 25000 },
  { _id: 2, totalRevenue: 20000 },
  { _id: 3, totalRevenue: 3000 },
  { _id: 4, totalRevenue: 1200 }
]
```

---

## **5. Filter & Aggregation (`$match` + `$group`)**  
_Get the total number of sales for only "Delivered" orders._  
```javascript
db.Orders.aggregate([
    { $match: { status: "Delivered" } },
    {
        $group: {
            _id: null,
            totalSales: { $sum: "$qty" }
        }
    }
])
```
### **Output:**
```js
[ { _id: null, totalSales: 4 } ]
```

---

