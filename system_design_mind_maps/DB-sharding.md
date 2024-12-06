# Shard (Database Architecture) - Mind Map

## 1. Introduction to Sharding
- **Definition**: Divides data across multiple databases (shards) for scalability.
- **Purpose**:
  - Enhances scalability.
  - Reduces load on individual databases.

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

## 6. Implementation Strategies
- **Hash-Based Sharding**:
  - Uses hash functions for data distribution.
- **Range-Based Sharding**:
  - Divides data into continuous ranges.
  - Example: Users 1-1000 in Shard 1, 1001-2000 in Shard 2.
- **Directory-Based Sharding**:
  - Uses a mapping service for dynamic key-to-shard assignment.

---

## 7. Database Systems Utilizing Sharding
- **MySQL Cluster**:
  - NDB Cluster engine with three-node architecture.
- **MongoDB**:
  - Built-in sharding with range or hash-based sharding keys.
- **Cassandra**:
  - Consistent hashing for automatic sharding.
- **PostgreSQL**:
  - Extensions like Citus for sharding support.

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

For more information, refer to [Wikipedia](https://en.wikipedia.org/wiki/Shard_(database_architecture)).