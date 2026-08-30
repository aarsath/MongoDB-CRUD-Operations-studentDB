# MongoDB CRUD Operations — studentDB

Understanding the basics of MongoDB commands through a Student Database, and applying them in a real-world Library Management System use case.

## 1. Database & Collection Setup

```javascript
C:\Users\hp>mongosh

test> use studentdb
studentdb> db.createCollection("students")
studentdb> show dbs
studentdb> show collections
```

## 2. Insert Operations

### Insert One Document
```javascript
studentdb> db.students.insertOne({
  name: "Abdul",
  age: 21,
  course: "MERN Stack",
  status: "ongoing"
})
```
**Output:**
```javascript
{
  acknowledged: true,
  insertedId: ObjectId('6a93d33f3001b7bc9f5eecac')
}
```

### Insert Multiple Documents
```javascript
studentdb> db.students.insertMany([
  { name: "Shankar", age: 22, course: "MERN Stack", status: "ongoing" },
  { name: "Thoufiq", age: 20, course: "Data Science", status: "completed" },
  { name: "Arsath", age: 23, course: "MERN Stack", status: "completed" },
  { name: "Karthik", age: 19, course: "UI/UX Design", status: "ongoing" }
])
```
**Output:**
```javascript
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('6a93d37d3001b7bc9f5eecad'),
    '1': ObjectId('6a93d37d3001b7bc9f5eecae'),
    '2': ObjectId('6a93d37d3001b7bc9f5eecaf'),
    '3': ObjectId('6a93d37d3001b7bc9f5eecb0')
  }
}
```

## 3. Read Operations

### Fetch All Documents
```javascript
studentdb> db.students.find()
```
**Output:**
```javascript
[
  { _id: ObjectId('6a93d33f3001b7bc9f5eecac'), name: 'Abdul', age: 21, course: 'MERN Stack', status: 'ongoing' },
  { _id: ObjectId('6a93d37d3001b7bc9f5eecad'), name: 'Shankar', age: 22, course: 'MERN Stack', status: 'ongoing' },
  { _id: ObjectId('6a93d37d3001b7bc9f5eecae'), name: 'Thoufiq', age: 20, course: 'Data Science', status: 'completed' },
  { _id: ObjectId('6a93d37d3001b7bc9f5eecaf'), name: 'Arsath', age: 23, course: 'MERN Stack', status: 'completed' },
  { _id: ObjectId('6a93d37d3001b7bc9f5eecb0'), name: 'Karthik', age: 19, course: 'UI/UX Design', status: 'ongoing' }
]
```

### Pretty Print
```javascript
studentdb> db.students.find().pretty()
```

### Filter: MERN Stack Students
```javascript
studentdb> db.students.find({ course: "MERN Stack" })
```
**Output:**
```javascript
[
  { name: 'Abdul', age: 21, course: 'MERN Stack', status: 'ongoing' },
  { name: 'Shankar', age: 22, course: 'MERN Stack', status: 'ongoing' },
  { name: 'Arsath', age: 23, course: 'MERN Stack', status: 'completed' }
]
```

### Fetch One Student
```javascript
studentdb> db.students.findOne({ name: "Abdul" })
```
**Output:**
```javascript
{
  _id: ObjectId('6a93d33f3001b7bc9f5eecac'),
  name: 'Abdul',
  age: 21,
  course: 'MERN Stack',
  status: 'ongoing'
}
```

## 4. Update Operations

### Update One Document
```javascript
studentdb> db.students.updateOne(
  { name: "Abdul" },
  { $set: { status: "completed" } }
)
```
**Output:**
```javascript
{ acknowledged: true, matchedCount: 1, modifiedCount: 1 }
```

### Update Multiple Documents
```javascript
studentdb> db.students.updateMany(
  { course: "MERN Stack" },
  { $set: { status: "completed" } }
)
```
**Output:**
```javascript
{ acknowledged: true, matchedCount: 3, modifiedCount: 2 }
```
> matchedCount is 3 since 3 students take MERN Stack; modifiedCount is 2 because Arsath was already "completed".

## 5. Delete Operations

### Delete One Document
```javascript
studentdb> db.students.deleteOne({ name: "Karthik" })
```
**Output:**
```javascript
{ acknowledged: true, deletedCount: 1 }
```

### Delete All Documents (practice only)
```javascript
studentdb> db.students.deleteMany({})
```
**Output:**
```javascript
{ acknowledged: true, deletedCount: 4 }
```
> ⚠️ Practice only — this clears the entire collection. Avoid running this on real project data.

## 6. Query Operators

> For this section, assume the collection has been repopulated with the original 5 student records (Abdul, Shankar, Thoufiq, Arsath, Karthik) to demonstrate each operator clearly.

### `$gt` — Greater Than
```javascript
studentdb> db.students.find({ age: { $gt: 21 } })
```
**Output:**
```javascript
[
  { name: 'Shankar', age: 22, course: 'MERN Stack', status: 'ongoing' },
  { name: 'Arsath', age: 23, course: 'MERN Stack', status: 'completed' }
]
```

### `$lt` — Less Than
```javascript
studentdb> db.students.find({ age: { $lt: 21 } })
```
**Output:**
```javascript
[
  { name: 'Thoufiq', age: 20, course: 'Data Science', status: 'completed' },
  { name: 'Karthik', age: 19, course: 'UI/UX Design', status: 'ongoing' }
]
```

### `$in` — Match Multiple Values
```javascript
studentdb> db.students.find({ course: { $in: ["MERN Stack", "Data Science"] } })
```
**Output:**
```javascript
[
  { name: 'Abdul', course: 'MERN Stack' },
  { name: 'Shankar', course: 'MERN Stack' },
  { name: 'Thoufiq', course: 'Data Science' },
  { name: 'Arsath', course: 'MERN Stack' }
]
```

### `$and` — Both Conditions
```javascript
studentdb> db.students.find({
  $and: [ { course: "MERN Stack" }, { status: "completed" } ]
})
```
**Output:**
```javascript
[
  { name: 'Arsath', age: 23, course: 'MERN Stack', status: 'completed' }
]
```

### `$or` — Either Condition
```javascript
studentdb> db.students.find({
  $or: [ { status: "completed" }, { age: { $gt: 22 } } ]
})
```
**Output:**
```javascript
[
  { name: 'Thoufiq', age: 20, course: 'Data Science', status: 'completed' },
  { name: 'Arsath', age: 23, course: 'MERN Stack', status: 'completed' }
]
```

### `$exists` — Check Field Exists
```javascript
studentdb> db.students.find({ status: { $exists: true } })
```
**Output:**
```javascript
[
  { name: 'Abdul', status: 'ongoing' },
  { name: 'Shankar', status: 'ongoing' },
  { name: 'Thoufiq', status: 'completed' },
  { name: 'Arsath', status: 'completed' },
  { name: 'Karthik', status: 'ongoing' }
]
```

## 7. Use Case: Library Management System

**Scenario:** A small library wants to track its books, who borrowed them, and their availability.

### Setup
```javascript
use libraryDB
```

### Insert Book Records
```javascript
db.books.insertMany([
  { title: "The Alchemist", author: "Paulo Coelho", genre: "Fiction", copies: 5, issuedTo: null },
  { title: "Clean Code", author: "Robert Martin", genre: "Programming", copies: 2, issuedTo: null },
  { title: "Atomic Habits", author: "James Clear", genre: "Self-help", copies: 0, issuedTo: "Ravi" },
  { title: "The Pragmatic Programmer", author: "Andy Hunt", genre: "Programming", copies: 3, issuedTo: null }
])
```

### Search: Find Available Programming Books
```javascript
db.books.find({ genre: "Programming", copies: { $gt: 0 } })
```
**Output:**
```javascript
[
  { title: 'Clean Code', author: 'Robert Martin', genre: 'Programming', copies: 2, issuedTo: null },
  { title: 'The Pragmatic Programmer', author: 'Andy Hunt', genre: 'Programming', copies: 3, issuedTo: null }
]
```

### Update: Issue a Book
```javascript
db.books.updateOne(
  { title: "Clean Code" },
  { $inc: { copies: -1 }, $set: { issuedTo: "Divya" } }
)
```
> Clean Code now has `copies: 1`, `issuedTo: "Divya"`.

### Update: Return a Book
```javascript
db.books.updateOne(
  { title: "Atomic Habits" },
  { $inc: { copies: 1 }, $set: { issuedTo: null } }
)
```
> Atomic Habits now has `copies: 1`, `issuedTo: null`.

### Search: Books Currently Issued
```javascript
db.books.find({ issuedTo: { $ne: null } })
```
**Output:**
```javascript
[
  { title: 'Clean Code', issuedTo: 'Divya' }
]
```

### Delete: Remove a Book Record
```javascript
db.books.deleteOne({ title: "The Alchemist" })
```
**Output:**
```javascript
{ acknowledged: true, deletedCount: 1 }
```

## Outcome

By completing this task, we can confidently perform CRUD operations in MongoDB, use query operators (`$gt`, `$lt`, `$in`, `$and`, `$or`, `$exists`), and understand how to structure and access data efficiently — demonstrated through both the Student Database and the Library Management System use case.
