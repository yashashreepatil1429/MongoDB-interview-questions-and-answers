# MongoDB-interview-questions-and-answers

## 20 Important MongoDB Interview Questions and Answers

### 1. What is MongoDB?

MongoDB is a document-oriented NoSQL database. It stores data as BSON documents in collections rather than rows in relational tables. Its flexible document model, indexing, aggregation, and horizontal scaling make it useful for many kinds of applications.

### 2. What are databases, collections, and documents in MongoDB?

A database contains collections, and a collection contains documents. A document is a set of field-and-value pairs, similar to a JSON object, while BSON is the binary format MongoDB uses to store documents. Collections are broadly comparable to tables, and documents to rows, but documents can have nested structures and different fields.

### 3. What is BSON, and how is it different from JSON?

BSON (Binary JSON) is MongoDB's binary-encoded document format. It supports JSON-like objects and additional data types such as dates, 64-bit integers, and binary data. BSON also carries type and length information, which helps MongoDB process documents efficiently.

### 4. What is the role of the `_id` field?

Every document in a standard MongoDB collection has a unique `_id` field. MongoDB creates an `ObjectId` for it when one is not provided. Applications may supply another supported value, but `_id` must remain unique within its collection.

### 5. What is the difference between SQL and MongoDB?

SQL databases typically organize data into tables with a defined schema and use joins to combine related rows. MongoDB stores flexible BSON documents in collections, often embedding related data together. MongoDB supports schema validation and joins through `$lookup`, but its document model encourages designing data around application access patterns.

### 6. How do you insert, query, update, and delete documents?

Common methods include `insertOne()` or `insertMany()` to create documents, `find()` or `findOne()` to query them, `updateOne()` or `updateMany()` to modify them, and `deleteOne()` or `deleteMany()` to remove them. For example:

```javascript
db.users.insertOne({ name: "Asha", active: true })
db.users.find({ active: true })
db.users.updateOne({ name: "Asha" }, { $set: { active: false } })
db.users.deleteOne({ name: "Asha" })
```

### 7. What is the MongoDB Aggregation Framework?

The Aggregation Framework processes documents through an ordered sequence of stages called a pipeline. Stages can filter (`$match`), reshape (`$project`), group (`$group`), sort (`$sort`), and join data (`$lookup`). It is used for reporting and more complex transformations than a basic query.

### 8. What are indexes, and why are they important?

Indexes are data structures that help MongoDB find documents without scanning every document in a collection. They can improve query and sort performance, but consume storage and add work to inserts, updates, and deletes. Indexes should be chosen based on actual query patterns and verified with query plans.

### 9. What is a compound index, and why does field order matter?

A compound index includes multiple fields, for example:

```javascript
db.orders.createIndex({ customerId: 1, createdAt: -1 })
```

Field order matters because MongoDB can efficiently use the index for queries on the leading fields and their prefixes. Put fields in an order that supports the workload's filters and sorts; consider equality, sort, and range needs when designing the index.

### 10. What is the difference between a unique index and a regular index?

A regular index speeds up lookups but permits duplicate indexed values. A unique index also enforces uniqueness for its indexed key or key combination. Before creating one, check for existing duplicates; the index build will fail if the data violates the uniqueness constraint.

### 11. How do you analyze a slow query in MongoDB?

Use `explain("executionStats")` with the query to inspect the winning plan, documents and index keys examined, and documents returned. A high number of documents examined relative to results can indicate a collection scan or an ineffective index. Check the filter, sort, and projection, then test a suitable index and measure again.

### 12. What is a replica set?

A replica set is a group of MongoDB servers that maintain the same dataset. One member is the primary and accepts writes; eligible secondary members replicate the primary's operations and can serve reads depending on the read preference. If the primary becomes unavailable, the remaining eligible members can elect a new primary.

### 13. What is sharding?

Sharding distributes a collection's data across multiple servers to support datasets or workloads that exceed the capacity of one server. MongoDB partitions data using a shard key. A good shard key distributes writes and data effectively while supporting common queries; a poor choice can create hotspots or scatter queries across shards.

### 14. What is a shard key?

A shard key is the field or set of fields MongoDB uses to partition a sharded collection's documents. Its values determine which shard stores each document. Choose a key with suitable cardinality and distribution that aligns with query patterns; changing a shard key later can require significant operational work.

### 15. Does MongoDB support transactions?

Yes. MongoDB supports atomic transactions for multiple operations across documents and, in supported configurations, across multiple shards. Transactions are useful when a set of changes must succeed or fail together, but they add overhead. Data modeling that keeps related updates within a single document can often use MongoDB's single-document atomicity instead.

### 16. What is the difference between embedding and referencing?

Embedding stores related data inside the same document, which can retrieve related information together and supports atomic updates to that document. Referencing stores related documents separately and connects them by identifiers, which can reduce duplication or document growth for independently updated or large relationships. The right choice depends on access patterns, data size, and update behavior.

### 17. What is `$lookup`?

`$lookup` is an aggregation stage that performs a left outer join between documents in one collection and documents in another collection in the same database. It adds matching documents as an array field. It is useful when data is referenced, though frequent joins may indicate that embedding should be considered for the relevant access pattern.

### 18. What is schema validation in MongoDB?

Schema validation lets a collection enforce rules on documents, such as required fields, BSON types, and value constraints. MongoDB commonly uses a JSON Schema validator. It helps maintain data quality while preserving the flexibility to evolve a schema over time.

### 19. What is a TTL index?

A TTL (time-to-live) index is a special single-field index that allows MongoDB to automatically remove documents after a specified period or at a specified time. It is commonly used for expiring sessions, temporary records, or logs. Expiration is handled by a background process, so deletion is not guaranteed to happen at the exact expiration second.

### 20. What is a change stream?

A change stream lets an application subscribe to database changes such as inserts, updates, replacements, and deletes. It is built on MongoDB's replication infrastructure and can be opened on a collection, database, or deployment, subject to deployment and permission requirements. Applications can use resume tokens to continue watching after interruptions, provided the required oplog history is still available.