# 🟢 Mongo - Cheatsheet

* <mark style="color:purple;background-color:purple;">**We use pymongo**</mark>
* <mark style="color:purple;background-color:purple;">**`_id`**</mark>
  * <mark style="color:purple;background-color:purple;">**Mandatory primary key**</mark>
  * <mark style="color:purple;background-color:purple;">**Auto-generated if not provided**</mark>

| Operation                                                                    | Command                                                                                                         |
| ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| <mark style="color:purple;background-color:purple;">**Insert one**</mark>    | `db.col.insertOne({ name: "Amit", age: 32 })`                                                                   |
| <mark style="color:purple;background-color:purple;">**Insert many**</mark>   | `db.col.insertMany([{ name: "Ravi" }, { name: "Neha" }])`                                                       |
| <mark style="color:purple;background-color:purple;">**Get all**</mark>       | `db.col.find({})`                                                                                               |
| Get one                                                                      | `db.col.findOne({ name: "Amit" })`                                                                              |
| Filter                                                                       | `db.col.find({ city: "Bangalore" })`                                                                            |
| Greater than                                                                 | `db.col.find({ age: { $gt: 30 } })`                                                                             |
| <mark style="color:purple;background-color:purple;">**AND condition**</mark> | `db.col.find({ city: "Bangalore", age: { $gte: 30 } })`                                                         |
| <mark style="color:purple;background-color:purple;">**OR condition**</mark>  | `db.col.find({ $or: [{ city: "Mumbai" }, { age: { $lt: 30 } }] })`                                              |
| Projection                                                                   | `db.col.find({ city: "Bangalore" }, { name: 1, age: 1, _id: 0 })`                                               |
| Sort                                                                         | `db.col.find({}).sort({ age: -1 })`                                                                             |
| Limit                                                                        | `db.col.find({}).limit(5)`                                                                                      |
| <mark style="color:purple;background-color:purple;">**Update one**</mark>    | `db.col.updateOne({ name: "Amit" }, { $set: { age: 33 } })`                                                     |
| Update many                                                                  | `db.col.updateMany({ city: "Delhi" }, { $inc: { age: 1 } })`                                                    |
| Delete one                                                                   | `db.col.deleteOne({ name: "Ravi" })`                                                                            |
| Delete many                                                                  | `db.col.deleteMany({ age: { $lt: 25 } })`                                                                       |
| Array match                                                                  | `db.col.find({ skills: "Python" })`                                                                             |
| Push to array                                                                | `db.col.updateOne({ name: "Amit" }, { $push: { skills: "GenAI" } })`                                            |
| Count                                                                        | `db.col.countDocuments({ city: "Bangalore" })`                                                                  |
| Create index                                                                 | `db.col.createIndex({ email: 1 }, { unique: true })`                                                            |
| Aggregation                                                                  | `db.col.aggregate([{ $match: { city: "Bangalore" } }, { $group: { _id: "$city", avgAge: { $avg: "$age" } } }])` |
