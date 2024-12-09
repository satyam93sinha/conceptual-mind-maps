# Database Replication - Mind Map

## 1. Introduction to Replication
- **Definition**: Storing the same data on multiple machines to ensure availability and durability.
- **Purpose**:
  - Ensures high availability in distributed systems.
  - Maintains data integrity during failures.
  - Balances the trade-offs between **Consistency, Latency, and Availability**.

---

## 2. Replication and CAP Theorem
- **Conventional RDBMSs**:
  - Often **CA systems**.
  - Depend on high-end hardware for integrity and availability.
  - Single points of failure: System becomes unavailable if a machine fails.
- **NoSQL Systems**:
  - Designed for massive data volumes using cost-efficient **low-end hardware**.
  - Handle frequent failures in distributed systems.

### Example of Failures in Google Clusters:
- Thousands of hard drive failures yearly.
- 1,000 single-machine failures.
- 20 rack failures.
- Several network partitions.
- https://aphyr.com/posts/288-the-network-is-reliable 
- https://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf 
- https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton_html/ 

---

## 3. Replication Strategies (Two-Tier Classification) 
### **1st Tier: When to Propagate Updates**
- **Eager Replication** (Synchronous):
  - Changes propagated to all replicas before a commit.
  - **Advantages**:
    - Strong consistency.
  - **Disadvantages**:
    - High write latency.
    - Impaired availability.
- **Lazy Replication** (Asynchronous):
  - Updates occur locally and propagate asynchronously.
  - **Advantages**:
    - Lower write latency.
    - Higher availability.
  - **Disadvantages**:
    - Possibility of stale data.

### **2nd Tier: Where to Accept Updates**
- **Master-Slave (Primary Copy)**:
  - Writes occur only at the master.
  - **Advantages**:
    - Simplified concurrency control.
  - **Disadvantages**:
    - Unavailability on master failure.
- **Multi-Master (Update Anywhere)**:
  - Any replica can accept writes.
  - **Advantages**:
    - Better availability.
  - **Disadvantages**:
    - Complex conflict resolution.
    - Requires mechanisms like:
      - **Versioning**, **Vector Clocks**, **Gossip Protocols**.
      - **Read Repair**, **CRDTs** (Convergent or Commutative Data Types).

---

## 4. Combined Strategies
- **Eager Master-Slave**:
  - Strong consistency, e.g., traditional distributed RDBMSs.
- **Eager Update-Anywhere**:
  - Heavy communication overhead.
  - Risks: Distributed deadlocks.
  - Example: Google Megastore.
- **Lazy Master-Slave**:
  - Common in CP systems like HBase, MongoDB.
- **Lazy Update-Anywhere**:
  - Common in AP systems like Dynamo, Cassandra.
  - Clients can decide:
    - Minimal latency (read from any replica).
    - Strong consistency (read majority of replicas).

---

## 5. Geo-Replication
- **Definition**: Distributing replicas across geographically distant locations.
- **Advantages**:
  - Protection against total data loss.
  - Improved read latency for distributed clients.
- **Eager Geo-Replication**:
  - Examples: Megastore, Spanner, MDCC, Mencius.
  - **Characteristics**:
    - Strong consistency.
    - Higher write latency (100ms-600ms).
- **Lazy Geo-Replication**:
  - Examples: Dynamo, PNUTS, Walter, COPS, Cassandra, BigTable.
  - **Characteristics**:
    - Higher availability during network partitions.
    - Potential loss of recent updates.

---

## 6. Replica Placement Considerations
- **Close Proximity**:
  - **Advantages**: Low latency.
  - **Disadvantages**:
    - Availability issues during rack failures.
    - Risk of simultaneous data loss in disasters.
- **Distributed Placement**:
  - Protects against localized failures.
  - Improves availability and durability.

---

## 7. Techniques and Mechanisms
 - ### 7.1 Conflict Resolution in Multi-Master**:
  - #### 7.1.1 Versioning
    - **Definition**: Tracks changes over time by assigning versions to data.
    - **Usage**:
        - Helps identify conflicting changes.
        - Allows reconciliation based on timestamps or semantic rules.
    - **Tricky Concepts**:
        - Conflicts occur when concurrent versions exist.
        - Requires merging strategies for conflict resolution.
    - **Pitfalls**: Complexity increases with high write concurrency.
    - **Solution**: Employ deterministic merge functions to resolve conflicts.
  - #### 7.1.2 Vector Clocks.
    - **Definition**: A data structure used to capture causality between events in a distributed system.
    - **Working**:
        - Each replica maintains a vector of logical clocks.
        - Updates increment the local clock and propagate with the vector.
    - **Advantages**: Precise identification of conflicting writes.
    - **Disadvantages**: Metadata overhead grows with the number of replicas.
    - **Pitfalls**: Overhead in maintaining and synchronizing clocks.
    - **Solution**: Compress clock metadata using optimized algorithms.
  - #### 7.1.3 Gossip Protocols.
    - **Definition**: A peer-to-peer communication method to disseminate updates across replicas.
    - **Characteristics**:
        - Stochastic propagation of updates.
        - Ensures eventual consistency.
    - **Advantages**:
        - Scalable for large distributed systems.
        - Resistant to node failures.
    - **Pitfalls**: May propagate stale data temporarily.
    - **Solution**: Use anti-entropy techniques like Merkle trees for data consistency checks.

  - #### 7.1.4 Read Repair.
    - **Definition**: A background mechanism to correct inconsistencies during read operations.
    - **Working**:
        - Compare data across replicas during a read request.
        - Fix inconsistencies on-the-fly by synchronizing replicas.
    - **Advantages**: Ensures up-to-date data for read-heavy systems.
    - **Pitfalls**: Latency increase for read requests during repair.
    - **Solution**: Perform repairs in batches to minimize read latency.
  - ### 7.1.5 Conflict-Free Replicated Data Types (CRDTs)
    - **Definition**: Data structures designed for distributed systems that allow safe, automatic merging of updates.
    - **Types**:
        - **Convergent (state-based) CRDTs**: Merge states from replicas.
        - **Commutative (operation-based) CRDTs**: Apply operations in any order.
    - **Examples**:
        - Counters, Sets, Maps with conflict-free operations.
    - **Advantages**:
        - Avoids manual conflict resolution.
        - Guarantees strong eventual consistency.
    - **Pitfalls**:
        - Complex implementation for custom data types.
    - **Solution**: Use libraries and frameworks supporting CRDTs.
- ### 7.2 Caching Techniques:
  ### 2.1 Orestes Caching
    - **Definition**: A technique using web caching infrastructure to reduce latency.
    - **Mechanism**:
        - Cache data close to applications.
        - Use **cache coherence protocols** to maintain data validity.
    - **Advantages**:
        - Reduces load on primary replicas.
        - Improves read performance.
    - **Disadvantages**:
        - Inconsistencies due to stale cache data.
    - **Pitfalls**:
        - Cache invalidation complexity.
    - **Solution**: Employ strong cache consistency protocols like write-through or write-behind.

---

## 8. Pitfalls and Solutions
- **High Write Latency**:
  - Mitigation: Use lazy replication for write-intensive workloads.
- **Stale Data**:
  - Mitigation: Use consistency models like quorum reads or strong consistency when necessary.
- **Distributed Deadlocks**:
  - Mitigation: Implement deadlock detection and resolution mechanisms.
- **Failure of Master in Master-Slave**:
  - Mitigation: Automated failover mechanisms.
### 8.1 Complexity in Conflict Detection
  - **Problem**: Increased overhead with concurrent updates.
  - **Solution**:
  - Automate conflict detection using algorithms like Lamport timestamps or vector clocks.

  ### 8.2 Latency in Synchronization
    - **Problem**: High latency in achieving consistency.
    - **Solution**:
    - Use asynchronous replication combined with lazy conflict resolution mechanisms.

  ### 8.3 Metadata Overhead
    - **Problem**: High storage cost for conflict-tracking metadata.
    - **Solution**:
    - Employ compaction and efficient data structures for metadata storage.

---

## 9. Example Systems and Their Replication Models
- **Google Spanner**:
  - Eager replication with global consistency.
- **Cassandra**:
  - Lazy replication with AP characteristics.
- **DynamoDB**:
  - Lazy replication with vector clocks for consistency.
- **BigTable**:
  - Lazy replication with optimized read performance.

---

## 10. Further Reading and References
- [IMPORTANT] (https://medium.baqend.com/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.wskogqenq)
- [JeffDeanGoogle] (http://db.cs.berkeley.edu/cs286/papers/dangers-sigmod1996.pdf)
- [NetworkPartitionsCases] (https://aphyr.com/posts/288-the-network-is-reliable)
- [2-TierGrayEtEl] (http://db.cs.berkeley.edu/cs286/papers/dangers-sigmod1996.pdf)
- [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem)
- [Distributed Databases](https://en.wikipedia.org/wiki/Distributed_database)
- [Google Spanner](https://research.google/pubs/pub39966/)
- [Conflict-Free Replicated Data Types (CRDTs)](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type)
- [Vector Clocks](https://en.wikipedia.org/wiki/Vector_clock)
- [Gossip Protocols](https://en.wikipedia.org/wiki/Gossip_protocol)
- [Conflict-Free Replicated Data Types](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type)
- [Caching Techniques](https://en.wikipedia.org/wiki/Cache_coherence)