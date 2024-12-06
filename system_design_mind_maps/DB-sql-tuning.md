# Mind Map: SQL Tuning

## 1. Introduction
- **Purpose**: Optimize SQL queries and database design to improve performance.
- **Approach**:
  - **Benchmarking**:
    - Simulate high-load conditions (e.g., using `ab`).
  - **Profiling**:
    - Track slow queries using tools like MySQL's slow query log.

---

## 2. Schema Optimization: Tightening the Schema
### a. **Optimizing Data Types**
- **CHAR**:
  - Fixed-length fields, suitable for uniform data.
  - Fast random access.
- **VARCHAR**:
  - Variable-length fields for non-uniform data.
  - More storage-efficient but slower random access.
  - 255: maximum length of string that can be stored
  - why 255? 1-byte length indicator can represent numbers from 0-255, thus length of the string.
  
- **TEXT**:
  - Ideal for large text blocks (e.g., blog posts).
  - Stored as pointers; supports boolean searches.

- **Numerical Data Types**:
  - Use `INT` for large numbers (up to \(2^{32}\) or 4 billion).
  - Use `DECIMAL` for precise values (e.g., currency) to avoid floating-point errors.

- **Other Tips**:
  - Avoid storing large BLOBs; store file paths instead.
  - Use `VARCHAR(255)` to optimize storage.
  - Set `NOT NULL` constraints for columns where applicable to improve search performance.
  - The fastest thing for the server to do is to move the row off that page into another, and to replace the row's entry with a forward pointer. Unfortunately, this requires an extra lookup when a query is performed: one to find the natural location of the row, and one to find its current location. So, the short answer to your question is yes, making those fields non-nullable will help search performance. This is especially true if it often happens that the null fields in records you search on are updated to non-null. - SQL Server.
  - Use required data type, validate input value to avoid data truncation errors like VARCHAR(50) or CHAR(50) truncates input string to length 50, corrupting data > length 50.

---

## 3. Query Optimization
### a. **Indices**
- **Purpose**:
  - Speeds up queries (`SELECT`, `GROUP BY`, `JOIN`).
- **Structure**:
  - Self-balancing B-trees for efficient search and insertion.
- **Trade-offs**:
  - Increased memory usage.
  - Slower write operations due to index updates.

### b. **Partitioning and Sharding**
- Partition hot spots into separate tables.
- Keep frequently accessed data in memory.

### c. **Avoiding Expensive Joins**
- Use denormalization to reduce complex joins.
- Balance performance with storage efficiency.

---

## 4. Query Cache Optimization
- **Query Cache**:
  - Temporarily stores query results to speed up repeated queries.
- **Drawbacks**:
  - Risk of stale data.
  - Performance issues with certain workloads.

---

## 5. Advanced Topics
### a. **Floating-Point Representation Errors**
- **Why They Occur**:
  - Binary encoding approximates values, leading to precision loss.
- **Solution**:
  - Use `DECIMAL` or fixed-point types for precise calculations.

### b. **CHAR vs VARCHAR vs TEXT**:
- **Internal Implementation**:
  - **CHAR**: Fixed-size allocation, no end marker.
  - **VARCHAR**: Length-prefix encoding.
  - **TEXT**: Pointer-based external storage.

---

## 6. Summary of Tips
- **Schema Design**:
  - Optimize data types and apply `NOT NULL` constraints.
- **Indexing**:
  - Index columns involved in frequent queries.
- **Performance Analysis**:
  - Regularly profile queries and refine indexes.

---

## 7. Sources for Further Reading
- [10 Tips for Optimizing MySQL Queries](http://aiddroid.com/10-tips-optimizing-mysql-queries-dont-suck/)
- [Impact of NULL Values on Search Performance](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)
- [MySQL Query Cache Tuning](https://dev.mysql.com/doc/refman/5.7/en/query-cache.html)

---


Mind Map: SQL Tuning

1. Introduction

	•	Definition: SQL tuning is the process of optimizing SQL queries and database schema to improve performance.
	•	Tools for Benchmarking:
	•	Benchmarking: Simulates high-load scenarios using tools like ab.
	•	Profiling: Uses tools like MySQL slow query log to identify bottlenecks.

2. Schema Optimization: Tightening the Schema

a. Optimizing Data Types

	•	CHAR vs VARCHAR vs TEXT:
	•	CHAR:
	•	Fixed-length fields.
	•	Fast random access; suitable for uniform-length data.
	•	VARCHAR:
	•	Variable-length fields; more storage-efficient for non-uniform data.
	•	Slower random access as the end of the string must be located.
	•	TEXT:
	•	Suitable for large text blocks (e.g., blog posts).
	•	Stores pointers on disk for text locations, supporting boolean searches.
	•	Reasons for Use:
	•	CHAR: Improves lookup speeds in fixed-length fields.
	•	VARCHAR: Saves storage for variable-length data.
	•	TEXT: Enables efficient handling of large text fields with search capabilities.
	•	Numerical Types:
	•	INT: Suitable for numbers up to ￼ (4 billion).
	•	DECIMAL: Best for currencies to avoid floating-point precision issues.
	•	Other Best Practices:
	•	Use NOT NULL constraints where possible to:
	•	Improve search performance.
	•	Allow optimizations by the database engine.
	•	Avoid storing large BLOBs in the database; instead, store references (e.g., file paths).

3. Query Optimization

a. Using Indices

	•	What are Indices:
	•	Structures (e.g., self-balancing B-trees) that keep data sorted.
	•	Enable faster queries for SELECT, GROUP BY, ORDER BY, and JOIN.
	•	Benefits:
	•	Reduces query time by keeping frequently accessed data in memory.
	•	Trade-offs:
	•	Slower write performance as indices must be updated.
	•	Consumes additional memory space.
	•	Best Practices:
	•	Disable indices when loading bulk data, then rebuild them afterward.

b. Partitioning Tables

	•	Definition: Splits a large table into smaller ones to reduce “hot spots.”
	•	Benefits:
	•	Keeps frequently accessed data in memory.
	•	Improves query performance on partitioned data.

c. Avoiding Expensive Joins

	•	Denormalization:
	•	Reducing join operations by storing redundant data in a single table.
	•	Improves performance at the cost of storage efficiency.

4. Query Cache Optimization

	•	Query Cache:
	•	Temporarily stores query results to improve performance.
	•	Can be a double-edged sword:
	•	Advantages: Reduces query processing time.
	•	Disadvantages: Can lead to stale data or performance bottlenecks.

5. Advanced Topics

a. Floating Point Representation Errors

	•	Why It Occurs:
	•	Floating-point numbers are stored as approximations due to binary encoding.
	•	Leads to precision loss during arithmetic operations.
	•	How to Resolve:
	•	Use DECIMAL or fixed-point types for calculations requiring high precision (e.g., monetary values).

b. CHAR vs VARCHAR vs TEXT:

	•	Internal Implementation:
	•	CHAR: Fixed-size allocation; no end marker needed.
	•	VARCHAR: Uses a length prefix to indicate size; more efficient for variable data but slower for reads.
	•	TEXT: Data stored out-of-row with pointers to the actual content.

6. Summary of Tips

	•	Schema Design:
	•	Choose the appropriate data type for the use case.
	•	Apply NOT NULL constraints wherever possible.
	•	Indexing:
	•	Focus on columns used in frequent queries.
	•	Optimize index use during data load operations.
	•	Performance Analysis:
	•	Profile queries regularly to identify slow operations.
	•	Use query caching judiciously for high-read workloads.

7. Additional Reading

	•	10 Tips for Optimizing MySQL Queries
	•	MySQL Slow Query Log
	•	VARCHAR vs CHAR Usage
	•	Impact of NULL Values on Search Performance
	•	MySQL Query Cache Tuning
