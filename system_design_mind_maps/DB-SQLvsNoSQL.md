# Differences Between Relational and Non-Relational Databases (Mind Map)

## 1. Data Model
- **Relational (SQL):** Uses tables with rows and columns, resembling spreadsheets. Each row represents a record, and each column represents an attribute.
- **Non-Relational (NoSQL):** Uses models such as:
  - **Key-Value:** Data stored as key-value pairs (e.g., Redis).
  - **Document:** Stores entire objects/documents in formats like JSON (e.g., MongoDB).
  - **Column-Family:** Data stored in columns grouped by families (e.g., Cassandra).
  - **Graph:** Stores nodes and relationships (e.g., Neo4j).

---

## 2. Schema
- **Relational:** Fixed schema, requiring well-defined tables and data types. Changing schema needs migration.
- **Non-Relational:** Flexible or schema-less; can store data without strict structure.

---

## 3. Data Integrity
- **Relational:** Follows ACID principles ensuring:
  - **Atomicity:** Transactions are all-or-nothing.
  - **Consistency:** Data remains in a valid state after transactions.
  - **Isolation:** Transactions occur independently.
  - **Durability:** Data persists after completion.
- **Non-Relational:** Often follows BASE model:
  - **Basically Available, Soft state, Eventual consistency.**

---

## 4. Query Language
- **Relational:** Uses SQL for querying, which includes complex joins and aggregations.
- **Non-Relational:** No standard query language; uses APIs or proprietary languages (e.g., MongoDB’s query syntax).

---

## 5. Examples
- **Relational:** MySQL, PostgreSQL, Oracle, MS SQL Server.
- **Non-Relational:** MongoDB, Cassandra, Redis, Couchbase, Neo4j.

---

## 6. Scalability
- **Relational:** Vertically scalable – adds resources to a single machine (CPU, RAM).
- **Non-Relational:** Horizontally scalable – adds multiple machines to distribute data.

---

## 7. Use Cases
- **Relational:** Suitable for applications like:
  - Banking transactions.
  - Inventory management.
  - Enterprise systems (ERP, CRM).
- **Non-Relational:** Best for:
  - Social networks.
  - IoT applications.
  - Product catalogs.

---

## 8. Performance
- **Relational:** Optimized for complex queries and joins.
- **Non-Relational:** Optimized for quick lookups and distributed data handling.

---

## 9. Data Storage
- **Relational:** Uses tables (rows and columns) to store data.
- **Non-Relational:** Stores data as:
  - JSON, XML (Document).
  - Key-value pairs.
  - Graph models.
  - Column-oriented storage.

---

## 10. Relationships
- **Relational:** Uses foreign keys and joins to connect data across tables.
- **Non-Relational:** Embeds related data within documents or uses graph structures to model relationships.

---

## 11. Transaction Management
- **Relational:** Strong transactional consistency, suitable for systems where accuracy is crucial (e.g., banks).
- **Non-Relational:** Uses eventual consistency, which prioritizes availability over immediate consistency.

---

## 12. Handling of Big Data
- **Relational:** Less efficient with large-scale, distributed data.
- **Non-Relational:** Designed to handle Big Data with distributed architectures.

---

## 13. Complexity
- **Relational:** Requires normalization to reduce redundancy, which increases design complexity.
- **Non-Relational:** Easier to use with denormalized data models.

---

## 14. Development Flexibility
- **Relational:** Changing schema involves data migration.
- **Non-Relational:** Schema can evolve easily, making it suitable for agile projects.

---

## 15. Indexing and Search
- **Relational:** Uses B-trees or bitmap indexing for fast searches.
- **Non-Relational:** Provides specialized indexes (e.g., TTL index in MongoDB, secondary indexes in Cassandra).

---

## 16. Backup and Recovery
- **Relational:** Mature tools for backup and disaster recovery.
- **Non-Relational:** Relies on replication-based recovery or eventual backup solutions.

---

## 17. Security
- **Relational:** Offers encryption, role-based access control, and compliance with security standards.
- **Non-Relational:** Security models vary across databases; not as standardized as relational systems.

---

## 18. Handling Data Types
- **Relational:** Best for structured data (e.g., integers, strings, dates).
- **Non-Relational:** Can handle complex and unstructured data (e.g., JSON, multimedia, blobs).

---

## 19. Data Consistency
- **Relational:** Ensures strong consistency across operations.
- **Non-Relational:** Uses eventual consistency, focusing on availability and partition tolerance.

---

## 20. Example Scenarios
- **Relational:** Banking systems, inventory management, hospital databases.
- **Non-Relational:** E-commerce websites, social media, real-time analytics systems.