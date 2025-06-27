🌟 华为云数据库（CloudDB）在HarmonyOS中的使用指南 🌟

​​嗨，开发者朋友们！​​
今天咱们来聊聊华为云数据库（CloudDB）在HarmonyOS应用中的集成和使用技巧。无论你是刚接触HarmonyOS开发，还是想优化现有的数据管理逻辑，这篇指南都会手把手带你玩转数据的增删改查，还有那些超实用的高级查询功能！

🔥 ​​核心功能与使用场景​​
华为云数据库（CloudDB）提供了轻量级、高性能的云端数据存储能力，支持实时同步和数据加密。通过简单的API调用，你可以快速实现数据持久化、复杂查询和多端同步。以下是高频操作详解👇

📥 ​​写入数据（Upsert）​​
​​功能说明​​：

upsert() 方法用于写入或更新数据：

若数据主键已存在，则更新记录；
若不存在，则新增记录。
支持单条或批量写入（原子性操作，全部成功或失败）。

​​注意事项​​：

批量写入时，所有对象必须属于​​同一类型​​。
数据总大小不超过​​2MB​​，单次最多写入​​1000条​​。
​​代码示例​​：

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
    console.log("写入成功，数量：", record);  
  } catch (err) {  
    console.error("写入失败：", JSON.stringify(err));  
  }  
}  
🔍 ​​查询数据（Query）​​
​​1. 简单查询​​
​​查询所有数据​​：

async function queryAllBooks() {  
  const result = await cloud.database(...).collection("BookInfo").query().get();  
  console.log("所有书籍：", result);  
}  
​​条件查询​​（如书名等于“左传”）：

query().equalTo("bookName", "左传").get();  
​​2. 复合查询​​
​​多条件组合​​（AND/OR逻辑）：

// 查询价格在20到50之间的“数据库”相关书籍  
query()  
  .contains("bookName", "数据库")  
  .greaterThan("price", 20)  
  .and()  
  .lessThan("price", 50)  
  .get();  

// 查询价格<20 或 >50的“数据库”书籍  
query()  
  .contains("bookName", "数据库")  
  .lessThan("price", 20)  
  .or()  
  .greaterThan("price", 50)  
  .get();  
​​3. 排序与分页​​
​​按价格降序排列​​：

query()  
  .lessThan("price", 50)  
  .orderByDesc("price")  
  .get();  
​​分页查询​​（跳过前5条，取10条）：

query()  
  .lessThan("price", 50)  
  .orderByDesc("price")  
  .limit(10, 5)  // limit(数量, 起始位置)  
  .get();  
🗑️ ​​删除数据（Delete）​​
​​功能说明​​：

根据主键删除单条或批量数据（原子性操作）。
​​代码示例​​：

async function deleteBook() {  
  try {  
    const count = await cloud.database(...).collection("BookInfo").delete({ "id": 2000 });  
    console.log("删除成功，数量：", count);  
  } catch (err) {  
    console.error("删除失败：", err);  
  }  
}  
🚀 ​​最佳实践与避坑指南​​
​​性能优化​​：

避免频繁小数据写入，优先批量操作。
复杂查询尽量在服务端预过滤，减少数据传输量。
​​错误处理​​：

所有操作建议包裹在try-catch中，捕获异步异常。
​​安全建议​​：

敏感数据启用字段级加密（如用户手机号）。
🌈 ​​总结​​
华为云数据库（CloudDB）让HarmonyOS应用的数据管理变得轻松又高效！通过本文的代码示例和技巧，相信你已经掌握了增删改查的核心操作。快去动手实践吧，遇到问题欢迎到评论区提问，和更多开发者一起探讨！

​​祝你开发顺利，代码无Bug！​​ 🚀

希望这篇指南能成为你的开发利器！如果觉得有用，别忘了转发给小伙伴哦~ 😊