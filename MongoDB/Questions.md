MongoDB Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. What is MongoDB?
~~~~~~~~~~~~~~~~~~~

A document-oriented, NoSQL database
Stores data as flexible, JSON-like documents (BSON) instead of tables/rows
Schema-less by design — documents in the same collection can have different structures

2. SQL vs. NoSQL
~~~~~~~~~~~~~~~~

SQL — structured tables, fixed schema, relationships via foreign keys, uses SQL query language, strong ACID compliance, vertically scalable
NoSQL — flexible/dynamic schema, various data models (document, key-value, graph, column), horizontally scalable, eventual consistency common
SQL suits complex relational data; NoSQL suits large-scale, evolving, or unstructured data

3. Document & Collection vs. SQL tables/rows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Document — a single record, stored as a JSON-like object (BSON), equivalent to a row in SQL
Collection — a group of documents, equivalent to a table in SQL
Key difference: documents in the same collection don't need identical structure, unlike SQL rows in a table

4. BSON vs. JSON
~~~~~~~~~~~~~~~~

BSON = Binary JSON, MongoDB's internal storage format
Differences from JSON: binary-encoded (not human-readable text), supports more data types (like Date, ObjectId, Binary), faster to parse/traverse, slightly larger size than plain JSON due to extra type info

5. The "_id" field
~~~~~~~~~~~~~~~~~~

Automatically generated unique identifier for every document
Acts as the primary key for the collection
Data type: ObjectId by default — a 12-byte identifier encoding timestamp, machine, process, and counter info (can be overridden with a custom value)

6. Embedded vs. Reference Documents
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Embedded/Nested — related data stored directly inside the parent document (denormalization)
Reference — related data stored in a separate collection, linked via an _id reference (normalization)
Embedding — faster reads (single query), but can bloat documents; Referencing — normalized, avoids duplication, but requires extra queries/joins ($lookup)

7. When to embed vs. reference
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Embed when: data is tightly coupled, accessed together frequently, doesn't grow unbounded (e.g. address inside a user document) — good for 1:few relationships
Reference when: data is large, reused across multiple documents, grows unbounded, or updated independently (e.g. blog posts and authors) — better for 1:N or N:M relationships
Rule of thumb: embed for "contains" relationships, reference for "belongs to/relates to" relationships

8. CRUD operations
~~~~~~~~~~~~~~~~~~

Create: insertOne(), insertMany()
Read: find(), findOne()
Update: updateOne(), updateMany()
Delete: deleteOne(), deleteMany()
Example:
js
  db.users.insertOne({ name: "John", age: 25 });
  db.users.find({ age: { $gt: 20 } });
  db.users.updateOne({ name: "John" }, { $set: { age: 26 } });
  db.users.deleteOne({ name: "John" });

9. updateOne() vs. updateMany() vs. replaceOne()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

updateOne() — updates the first document matching the filter, modifies only specified fields
updateMany() — updates all documents matching the filter
replaceOne() — replaces the entire document (except _id) with a new one, rather than just modifying fields

10. $set, $unset, $push, $pull operators
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

$set — sets/updates the value of a field
$unset — removes a field from a document
$push — adds an element to an array field
$pull — removes element(s) matching a condition from an array field
js
db.users.updateOne({ _id: id }, { $set: { name: "Jane" } });
db.users.updateOne({ _id: id }, { $push: { tags: "new" } });

11. Indexes in MongoDB
~~~~~~~~~~~~~~~~~~~~~~

Special data structures that improve query lookup speed by avoiding full collection scans
Important for performance because without an index, MongoDB scans every document to find matches — indexes make lookups much faster, especially at scale
Trade-off: indexes speed up reads but add overhead to writes and use extra storage

12. Types of indexes
~~~~~~~~~~~~~~~~~~~~

Single Field — index on one field, e.g. { name: 1 }
Compound — index on multiple fields, e.g. { name: 1, age: -1 }, order matters for query optimization
Text — enables text search across string content
Geospatial — supports location-based queries (2d/2dsphere), e.g. finding nearby points

13. Aggregation Framework
~~~~~~~~~~~~~~~~~~~~~~~~~

A framework for processing and transforming data through a sequence of stages, forming a "pipeline"
Each stage takes input documents, processes them, and passes output to the next stage
Used for: filtering, grouping, reshaping, computing aggregated values (sums, averages), similar to SQL's GROUP BY/JOIN but more flexible

14. Common aggregation stages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

$match — filters documents (like a WHERE clause)
$group — groups documents by a field and computes aggregates (sum, avg, count)
$project — reshapes documents, includes/excludes/renames fields
$sort — sorts documents by specified field(s)
$limit — restricts the number of documents passed to the next stage
js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$customer", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } },
  { $limit: 5 }
]);

15. $lookup stage
~~~~~~~~~~~~~~~~~

Performs a left outer join between two collections within an aggregation pipeline
Combines documents from a "foreign" collection based on a matching field
js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerDetails"
    }
  }
]);
Used to fetch related data across collections without manual application-level joining

16. Replication (Replica Sets)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A group of MongoDB servers maintaining the same data set — one primary node (handles writes) and multiple secondary nodes (replicate data from primary)
Provides High Availability: if the primary fails, an automatic election promotes a secondary to primary, minimizing downtime
Also improves read scalability (secondaries can serve read queries) and data redundancy/backup

17. Sharding
~~~~~~~~~~~~

A method of horizontal scaling — distributes data across multiple servers (shards) based on a shard key
Each shard holds a subset of the total data, allowing the database to handle much larger datasets and higher throughput than a single server could
A mongos router directs queries to the appropriate shard(s)

18. Horizontal vs. Vertical Scaling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Horizontal scaling (sharding) — adding more servers/machines to distribute load, near-limitless scalability, more complex to set up
Vertical scaling — adding more resources (CPU, RAM) to a single existing server, simpler but has a hard physical/cost ceiling
MongoDB is designed to scale horizontally via sharding, which suits very large or fast-growing datasets

19. Transactions & ACID in MongoDB
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

MongoDB supports multi-document ACID transactions (since v4.0 for replica sets, v4.2 for sharded clusters)
Ensures Atomicity, Consistency, Isolation, Durability across multiple operations/documents, similar to relational databases
Prior to this, MongoDB only guaranteed atomicity at the single-document level; transactions extend that guarantee across multiple documents/collections

20. Mongoose & benefits of an ODM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mongoose is an ODM (Object Data Modeling) library that provides a structured, schema-based way to interact with MongoDB in Node.js
Benefits: schema validation, built-in type casting, middleware (hooks), query building helpers, relationship population (.populate()), and cleaner, more maintainable code compared to using the raw MongoDB driver

21. Mongoose Schema vs. Model
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Schema — defines the structure of documents: field names, types, validation rules, defaults
js
  const userSchema = new mongoose.Schema({ name: String, age: Number });
Model — a compiled version of the schema, providing the actual interface to interact with the MongoDB collection (create, query, update documents)
js
  const User = mongoose.model('User', userSchema);
Schema defines the shape; Model is the usable, queryable constructor built from that shape

22. Mongoose Middleware (Pre/Post hooks)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Functions that run at specific points during document/query lifecycle operations
pre hooks — run before an operation (e.g. hashing a password before saving)
js
  userSchema.pre('save', function(next) { /* hash password */ next(); });
post hooks — run after an operation completes (e.g. logging after a document is saved)
Used for: validation, logging, cascading operations, data transformation

23. Populating references with .populate()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Replaces a referenced ObjectId field with the actual referenced document's data, similar to a SQL join
js
const orderSchema = new mongoose.Schema({
  customer: { type: mongoose.Schema.Types.ObjectId, ref: 'Customer' }
});

Order.find().populate('customer');
Makes it easy to fetch related data across collections without manually querying each reference

24. Capped Collections
~~~~~~~~~~~~~~~~~~~~~~

Fixed-size collections that maintain insertion order and automatically overwrite the oldest documents once the size limit is reached
Behave like a circular buffer — no manual deletion needed
Use cases: logging, caching, storing real-time data like chat messages or monitoring data where only recent entries matter

25. Analyzing query performance with explain()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.explain() shows how MongoDB executes a query — which indexes were used, how many documents were scanned, execution time, etc.
js
db.users.find({ age: { $gt: 20 } }).explain("executionStats");
Helps identify slow queries, missing indexes, or full collection scans (COLLSCAN) that should be optimized with proper indexing