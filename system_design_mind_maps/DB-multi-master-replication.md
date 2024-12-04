Mind Map: Multi-Master Replication

1. Introduction

	•	Multi-master replication allows multiple database servers (masters) to handle both read and write operations, improving availability and scalability in distributed systems.

2. Key Concepts

	•	Replication: Synchronizing data across multiple database instances.
	•	Concurrency: Managing simultaneous data updates across all masters.
	•	Conflict Resolution: Handling data conflicts that arise during simultaneous writes.

3. Sub-Topics

a. How Multi-Master Replication Works

	•	Data changes are propagated asynchronously or synchronously between nodes.
	•	Ensures eventual consistency or strong consistency depending on configuration.
	•	Uses strategies like write-ahead logs (WAL) or triggers to replicate changes.

b. Conflict Handling

	•	Strategies:
	•	Last-write-wins.
	•	Custom application logic.
	•	Conflict-free replicated data types (CRDTs).
	•	Why It’s Needed:
	•	Concurrent updates can lead to conflicting data states.

c. Architectures

	•	Synchronous Replication:
	•	Ensures all nodes are updated before acknowledging a transaction.
	•	Example: Postgres-XC, Postgres-XL.
	•	Asynchronous Replication:
	•	Updates propagate in the background, reducing latency but risking stale reads.
	•	Example: SymmetricDS.

4. Advantages

	•	Improved availability: Multiple nodes ensure fault tolerance.
	•	Load balancing: Distributes read/write operations.
	•	Scalability: Can handle high traffic loads in distributed environments.

5. Disadvantages and Pitfalls

	•	Conflict resolution complexity.
	•	Increased latency in synchronous replication.
	•	Overhead in managing data consistency.
	•	Not suitable for all workloads (e.g., those requiring strict ACID compliance).

6. Examples of Multi-Master Implementation

a. PostgreSQL

	•	Supports bidirectional replication via extensions like BDR.
	•	Synchronous multi-master via forks like Postgres-XC and Postgres-XL.
	•	Built-in WAL-based asynchronous replication ￼ ￼.

b. SymmetricDS

	•	Open-source multi-master replication tool.
	•	Supports diverse databases (MySQL, PostgreSQL, Oracle, MongoDB, etc.).
	•	Offers horizontal and vertical filtering of synchronized data ￼.

7. Implementation Strategies

	•	Use replication tools suited to application needs (e.g., Bucardo, Slony-I for PostgreSQL).
	•	Leverage built-in replication features or third-party solutions for conflict resolution.
	•	Optimize for workload characteristics (e.g., high-read or high-write).

8. When to Use

	•	Distributed systems with geographically separated users.
	•	High availability setups requiring redundancy.
	•	Systems where downtime for updates is unacceptable.

9. References

	•	Multi-Master Replication on Wikipedia
	•	PostgreSQL Replication
	•	SymmetricDS Overview