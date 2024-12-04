# Mind Map: System Design Primer - Database

## 1. Introduction to Databases
   - **Importance in system design**: Databases store, organize, and retrieve large amounts of data, making them foundational in application development.
   - **Use cases**: Data persistence, querying, handling high scalability needs.

---

## 2. Types of Databases

### **Relational Database Management System (RDBMS)**
   - **Structured Query Language (SQL)**:
     - Language for querying and managing relational databases.
     - Supports complex queries with JOINs, nested queries, and aggregations.
     - Schema-based with predefined structures (tables, columns, rows).
   - **ACID Transactions**:
     - **Atomicity**: All operations in a transaction are completed or none are.
     - **Consistency**: Database remains valid after a transaction.
     - **Isolation**: Transactions execute independently.
     - **Durability**: Completed transactions persist even after failures.

#### **Scaling Techniques**:
   - **Master-Slave Replication**:
     - **Master** handles writes; **slaves** handle reads.
     - Improves read performance but may cause stale reads.
   - **Master-Master Replication**:
     - Multiple masters for read/write operations.
     - Handles high availability but needs conflict resolution.
   - **Federation**:
     - Databases split by function or data type.
     - Reduces load per database but increases complexity.
   - **Sharding**:
     - Data split across multiple databases (shards) based on keys.
     - Enables horizontal scaling but complicates queries.
   - **Denormalization**:
     - Reduces JOINs by duplicating data.
     - Improves performance but increases redundancy.
   - **SQL Tuning**:
     - Indexing, optimizing queries, and reducing computational overhead.

### **NoSQL Databases**
   - **Categories**:
     - **Key-Value Store (e.g., Redis)**:
       - Simple key-value pairs.
       - High-speed lookups; good for caching.
     - **Document Store (e.g., MongoDB)**:
       - Stores data in JSON-like structures.
       - Flexible schemas for dynamic data.
     - **Wide-Column Store (e.g., Cassandra)**:
       - Column-oriented storage.
       - Efficient for large-scale, distributed datasets.
     - **Graph Database (e.g., Neo4j)**:
       - Stores relationships between entities as graphs.
       - Excellent for network and recommendation systems.
   - **Characteristics**:
     - **BASE Properties**:
       - **Basically Available**: System remains partially operational during failures.
       - **Soft-state**: State may change without input.
       - **Eventual Consistency**: Data converges to consistency over time.
     - **Horizontal Scalability**:
       - Scales by adding more servers.
   - **Use Cases**:
     - Real-time analytics, content management, social networks.

---

## 3. SQL vs NoSQL
   - **SQL**:
     - **Pros**:
       - Strong consistency, ACID compliance.
       - Mature tooling and support.
     - **Cons**:
       - Difficult to scale horizontally.
   - **NoSQL**:
     - **Pros**:
       - High scalability, flexible schemas.
     - **Cons**:
       - BASE properties mean weaker consistency.
   - **Decision factors**:
     - Data structure, query patterns, scalability needs.

---

## 4. Database Scaling

### **Vertical Scaling**:
   - Enhancing a single machine's capacity (e.g., more CPU, RAM).
   - Limited by hardware constraints.

### **Horizontal Scaling**:
   - Adding more servers to distribute data or traffic.
   - **Techniques**:
     - **Sharding**:
       - Data divided into independent parts.
     - **Partitioning**:
       - Logical data division for optimized storage and query performance.
   - **Trade-offs**:
     - Balancing complexity with performance gains.

---

## 5. Key Patterns in Databases

### **Consistency Patterns**:
   - **Weak Consistency**: Updated data may not immediately propagate.
   - **Eventual Consistency**: Data consistency achieved over time.
   - **Strong Consistency**: Immediate data consistency guaranteed.

### **Availability Patterns**:
   - **Failover**:
     - Automatic switching to backup servers during failures.
   - **Replication**:
     - Copies data across servers for fault tolerance.
   - **Availability Calculations**:
     - Measurement of system uptime (e.g., 99.99%).

---

## 6. Trade-offs and Theories

### **CAP Theorem**:
   - Only two of three can be guaranteed simultaneously:
     - **Consistency**: Data consistency across servers.
     - **Availability**: System remains operational during failures.
     - **Partition Tolerance**: System handles network partitioning.
   - Examples:
     - CP: RDBMS (e.g., SQL databases).
     - AP: NoSQL (e.g., DynamoDB).

### **ACID vs BASE**:
   - **ACID**:
     - Reliable transactions; suited for critical systems.
   - **BASE**:
     - Scalable, eventual consistency; suited for distributed systems.

---

## 8. References
   - [System Design Primer - Database Section](https://github.com/donnemartin/system-design-primer)