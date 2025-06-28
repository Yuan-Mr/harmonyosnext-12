### 🌟 Guide to Using Huawei Cloud Database (CloudDB) in HarmonyOS 🌟  

Hi, developers! Today, let's explore the integration and usage techniques of Huawei Cloud Database (CloudDB) in HarmonyOS applications. Whether you're new to HarmonyOS development or looking to optimize existing data management logic, this guide will walk you through CRUD (Create, Read, Update, Delete) operations and practical advanced query features!  


### 🔥 Core Functions and Use Cases  
Huawei Cloud Database (CloudDB) provides lightweight, high-performance cloud data storage with real-time synchronization and data encryption. Through simple API calls, you can quickly implement data persistence, complex queries, and multi-device synchronization. Below is a detailed breakdown of frequent operations 👇  


### 📥 Writing Data (Upsert)  
#### Function Description  
The `upsert()` method writes or updates data:  
- Updates records if the primary key exists.  
- Inserts new records if the primary key does not exist.  
- Supports single or batch writes (atomic operations: all succeed or fail).  

#### Notes  
- For batch writes, all objects must belong to the **same type**.  
- Total data size must not exceed **2MB**, with a maximum of **1,000 records** per batch.  

#### Code Example  
```typescript  
async function upsertBook() {  
  try {  
    const record = await cloud  
      .database({  
        objectTypeInfo: schema,  
        zoneName: "QuickStartDemo"  
      })  
      .collection("BookInfo")  
      .upsert({  
        "id": 2000,  
        "bookName": "book_name",  
        "author": "huawei",  
        "price": 1020  
      });  
    console.log("Write successful, count:", record);  
  } catch (err) {  
    console.error("Write failed:", JSON.stringify(err));  
  }  
}  
```  


### 🔍 Querying Data (Query)  
#### 1. Simple Queries  
##### Query All Data  
```typescript  
async function queryAllBooks() {  
  const result = await cloud.database(...).collection("BookInfo").query().get();  
  console.log("All books:", result);  
}  
```  

##### Conditional Query (e.g., book name equals "Zuo Zhuan")  
```typescript  
query().equalTo("bookName", "左传").get();  
```  

#### 2. Compound Queries  
##### Multi-condition Combinations (AND/OR Logic)  
```typescript  
// Query books related to "database" with prices between 20 and 50  
query()  
  .contains("bookName", "数据库")  
  .greaterThan("price", 20)  
  .and()  
  .lessThan("price", 50)  
  .get();  

// Query "database" books with prices <20 or >50  
query()  
  .contains("bookName", "数据库")  
  .lessThan("price", 20)  
  .or()  
  .greaterThan("price", 50)  
  .get();  
```  

#### 3. Sorting and Pagination  
##### Sort by Price in Descending Order  
```typescript  
query()  
  .lessThan("price", 50)  
  .orderByDesc("price")  
  .get();  
```  

##### Paged Query (skip first 5 records, fetch 10)  
```typescript  
query()  
  .lessThan("price", 50)  
  .orderByDesc("price")  
  .limit(10, 5)  // limit(count, start position)  
  .get();  
```  


### 🗑️ Deleting Data (Delete)  
#### Function Description  
Delete single or batch data by primary key (atomic operation).  

#### Code Example  
```typescript  
async function deleteBook() {  
  try {  
    const count = await cloud.database(...).collection("BookInfo").delete({ "id": 2000 });  
    console.log("Delete successful, count:", count);  
  } catch (err) {  
    console.error("Delete failed:", err);  
  }  
}  
```  


### 🚀 Best Practices and Pitfall Guide  
#### Performance Optimization  
- Avoid frequent small-data writes; prioritize batch operations.  
- Pre-filter complex queries on the server to reduce data transfer.  

#### Error Handling  
- Wrap all operations in try-catch to catch asynchronous exceptions.  

#### Security Recommendations  
- Enable field-level encryption for sensitive data (e.g., user phone numbers).  


### 🌈 Conclusion  
Huawei Cloud Database (CloudDB) simplifies data management for HarmonyOS applications! With the code examples and techniques in this article, you should now master core CRUD operations. Put them into practice—if you encounter issues, feel free to ask questions in the comments and discuss with other developers!  

Happy developing, and may your code be bug-free! 🚀  

Hope this guide becomes a powerful tool in your development journey. If you find it helpful, don't forget to share it with your peers! 😊
