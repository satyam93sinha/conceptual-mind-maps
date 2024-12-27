# Shard (Database Architecture) - Mind Map

## 1. Introduction to Sharding
- **Definition**: Data partitioning technique to distribute data across multiple nodes (shards) in a system.
- **Architectural Approaches**:
  - **Shared-Disk Architecture**:
    - Example: Oracle RAC, IBM DB2 pureScale.
    - Centralized data repository (e.g., NAS or SAN).
    - Pros: Consistent data.
    - Cons: Limited scalability.
  - **Shared-Nothing Architecture**:
    - Distributed servers with private memory and disks.
    - Examples: BigTable, MongoDB, DynamoDB.
    - Pros: High scalability in throughput and data volume.
    - Cons: Complexity in managing data distribution and consistency.

---

## 2. Key Concepts
- **Shard**: Individual database in the architecture.
- **Sharding Key**:
  - Defines data division across shards.
  - Examples: User IDs, geographic regions.
- **Shard Management**: 
  - Includes dynamic allocation and rebalancing.
- **Replication**:
  - Used alongside sharding for fault tolerance.
- **Distributed Queries**:
  - Handles cross-shard data requests.

---

## 3. Types of Sharding
- **Horizontal Sharding**:
  - Rows are distributed among shards.
  - Example: Split users by region.
- **Vertical Sharding**:
  - Columns are distributed across shards.
  - Example: User profiles vs. transactions.

---

## 4. Advantages
- **Performance**: Less load per shard.
- **Scalability**: Supports horizontal scaling.
- **Availability**: Shard failure is isolated.
- **Cost-Effectiveness**: Uses commodity hardware.

---

## 5. Disadvantages
- **Complexity**: Increased operational challenges.
- **Cross-Shard Queries**: Higher latency and complexity.
- **Rebalancing Overhead**: Expensive and time-consuming.
- **Key Selection Challenges**: Risk of hotspotting.

---

## 6. Implementation Strategies/Sharding Techniques
### 6.1 Range Sharding
- **Definition**: Partitions data into ordered and contiguous value ranges.
- **Advantages**:
  - Enables efficient scans.
  - Ideal for sequential data access patterns.
- **Disadvantages**:
  - Requires a coordinator (master) to manage shard assignments.
  - Hotspots can occur due to uneven data distribution.
- **Solution**:
  - Automatically detect and split overburdened shards.
- **Implementation**:
  - Supported by:
    - Wide-Column Stores: BigTable, HBase, Hypertable.
    - Document Stores: MongoDB, RethinkDB, Espresso, DocumentDB.
### 6.2 Hash Sharding
- **Definition**: Assigns data items to shards using a hash value derived from the primary key.
- **Advantages**:
  - Even distribution of data across shards.
  - No need for a central coordinator.
- **Disadvantages**:
  - Scans are infeasible due to lack of order.
  - Rehashing is required when servers are added or removed.
- **Solution**:
  - Use **Consistent Hashing**:
    - Records assigned to logical partitions instead of servers.
    - Reduces data reassignment during topology changes.
  - Examples:
    - Key-Value Stores: DynamoDB, Riak, Cassandra.
    - Wide-Column Stores: Cassandra, Azure Tables.
### 6.3 Entity-Group Sharding
- **Definition**: Groups related data into partitions (entity groups) for co-located transactions.
- **Advantages**:
  - Supports single-partition transactions efficiently.
- **Implementation**:
  - **Explicit Grouping**: Application-defined (e.g., G-Store, Megastore).
  - **Derived Grouping**: Based on access patterns (e.g., Relational Cloud, Cloud SQL Server).
- **Challenges**:
  - Multi-group transactions require complex protocols.?
  - **Solution**:?
    - Transfer data ownership between groups.
    - Use multi-node transaction managers as a fallback.

---

## 7. Database Systems Utilizing Sharding
- **7.1 MySQL Cluster**:
  - NDB Cluster engine with three-node architecture.
- **7.2 MongoDB**:
  - Built-in sharding with range or hash-based sharding keys.
- **7.3 Cassandra**:
  - Consistent hashing for automatic sharding.
- **7.4 PostgreSQL**:
  - Extensions like Citus for sharding support.HOW?
- **7.5 Wide-Column Stores**
  - Examples: BigTable, HBase, Hypertable.
  - Use: Range Sharding for structured data queries.
- **7.6 Document Stores**
  - Examples: MongoDB, RethinkDB, Espresso.
  - Use: Range Sharding for JSON-like document data.
- **7.7 Key-Value Stores**
  - Examples: DynamoDB, Riak, Cassandra.
  - Use: Hash Sharding with Consistent Hashing for high write scalability.
- **7.8 Relational Cloud Systems**
  - Examples: Cloud SQL Server, Relational Cloud.
  - Use: Entity-Group Sharding for transactional operations.

---

## 8. Use Cases
- **High-Traffic Applications**:
  - E-commerce, social media platforms.
- **Geographic Distribution**:
  - Localized data for faster access.

---

## 9. Sharding Best Practices
- **Shard Key Selection**:
  - Minimize cross-shard queries.
- **Rebalancing Plan**:
  - Automate load rebalancing.
- **Monitoring and Maintenance**:
  - Regular shard performance checks.

---

## 10. Consistent Hashing
- **Definition**: Assigns records to logical partitions distributed across shard servers.
- **Advantages**:
  - Only a fraction of data is reassigned during topology changes.
  - Ensures elasticity in systems with dynamic resource allocation.
- **Examples**:
  - Dynamo, Riak, Cassandra.
- **Implementation**:
  - Hash function determines logical partitions.
  - Partitions are reassigned to servers as they are added/removed.

---

## 11. Challenges and Solutions in Sharding
### 11.1 Hotspots
- **Problem**: Uneven distribution of data leads to overburdened shards.
- **Solution**:
  - Use adaptive algorithms to split or redistribute shards dynamically.

### 11.2 Scalability Trade-Offs
- **Problem**: Increased complexity in scaling and managing distributed systems.
- **Solution**:
  - Automate shard assignment and balancing processes.

### 11.3 Consistency and Transactions
- **Problem**: Maintaining strong consistency across shards can impair performance.
- **Solution**:
  - Adopt eventual consistency models where suitable.
  - Use entity-group sharding for transactional consistency.

### 11.4 Latency in Range Sharding
- **Problem**: Coordination overhead due to central shard manager.
- **Solution**:
  - Decentralize shard assignment with intelligent client-side logic.

---


## 12. References for Further Learning
- [IMPORTANT] (https://medium.baqend.com/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.wskogqenq)
- [CAP Theorem in Sharding](https://en.wikipedia.org/wiki/CAP_theorem)
- [Dynamo Paper (Consistent Hashing)](https://www.allthingsdistributed.com/2007/10/amazons_dynamo.html)
- [Google Megastore](https://research.google/pubs/pub36971/)