# Storage Management - Detailed Mind Map

## 1. Introduction to Storage Management
- **Purpose**: Optimize database systems for the storage media (RAM, SSDs, HDDs) used to serve and persist data.
- **Key Concepts**:
  - **Storage Pyramid**:
    - Visualizes storage hierarchy (e.g., RAM → SSD → HDD).
    - Transparent caches (L1-L3 CPU caches, disk buffers) implicitly improve data locality.
  - **Spatial Dimension**: *Where to store data*.
  - **Temporal Dimension**: *When to store data*.
  - **Techniques**:
    - Update-in-Place: Immediate data modification.
    - Append-Only I/O: Sequential data logging.

---

## 2. Storage Media Characteristics
### 2.1 RAM
- **Advantages**:
  - High-speed random and sequential access.
- **Disadvantages**:
  - High cost.
  - Volatility (data lost on power failure).
- **Solutions**:
  - **Replication**: Protects against single-node failures (e.g., HStore, VoltDB).
  - **Logging**: Persistent storage backup (e.g., Redis, SAP HANA).

### 2.2 SSDs (NAND Flash Memory)
- **Characteristics**:
  - Asymmetric read/write speeds.
  - No in-place overwrite (requires block erasure).
  - Limited program/erase cycles.
- **Advantages**:
  - Fast random reads.
  - Efficient sequential writes.
- **Disadvantages**:
  - Slower random writes than sequential writes.
- **Optimized Use**:
  - Logging suits SSDs for higher throughput.
  - Examples: Oracle Exadata, Aerospike.

### 2.3 HDDs (Spinning Disks)
- **Characteristics**:
  - Both random reads and writes are 10–100 times slower than sequential access.
- **Optimized Use**:
  - Logging for sequential writes.

---

## 3. Techniques in Storage Management
### 3.1 Update-in-Place
- **Definition**: Direct modification of data in storage.
- **Applications**:
  - Ideal for in-memory databases (e.g., MongoDB).
  - Simplifies implementation.
- **Challenges**:
  - Poor performance on HDDs and SSDs for random writes.
- **Solutions**:
  - Use main memory as a cache.
  - Logging to ensure durability.

### 3.2 Append-Only Storage (Log-Structured)
- **Definition**: Sequential writes to a log structure.
- **Advantages**:
  - Maximizes write throughput.
- **Implementations**:
  - **Log-Structured Merge (LSM) Trees**:
    - Components:
      - In-memory cache.
      - Persistent log.
      - Immutable storage files.
    - Examples: BigTable, Cassandra, LevelDB, RocksDB.
  - **Variants**:
    - Sorted Array Merge Trees (SAMT).
    - Cache-Oblivious Look-ahead Arrays (COLA).
  - **Indexing Techniques**:
    - Copy-on-Write Structures: CouchDB’s B-trees.
    - Immutable Structures: BigTable-style indexing.
- **Challenges**:
  - Costly garbage collection (compaction).
  - Solutions: Optimize compaction algorithms.

---

## 4. Challenges in Storage Management
### 4.1 Buffer Management
- **Problem**:
  - Significant overhead (e.g., RDBMS: 34.6% execution time spent on caching).
- **Solutions**:
  - Simplify buffer pools in NoSQL systems.
  - Use OS virtual memory (e.g., MongoDB’s MMAP engine).

### 4.2 Durability in In-Memory Databases
- **Problem**:
  - Volatility of RAM.
- **Solutions**:
  - Replicate data across nodes.
  - Log writes to persistent storage.

### 4.3 Logging Overhead
- **Problem**:
  - High latency due to individual log writes.
- **Solution**:
  - Group Commit: Batch multiple log writes to improve throughput.

### 4.4 Random Access Performance
- **Problem**:
  - Slow random access on HDDs and SSDs.
- **Solutions**:
  - Prefer sequential access patterns.
  - Optimize database architecture for media characteristics.

---

## 5. Applications of Storage Techniques
### 5.1 In-Memory Databases
- Examples: Redis, SAP HANA.
- Techniques: Update-in-Place, Logging.

### 5.2 LSM-Based Systems
- Examples: Cassandra, RocksDB, BigTable.
- Techniques: Append-Only I/O, Log-Structured Merge Trees.

### 5.3 SSD-Optimized Databases
- Examples: Oracle Exadata, Aerospike.
- Techniques: Sequential logging, Asynchronous writes.

---

## 6. Architectural Insights
- **Stonebraker et al. Findings**:
  - Only 6.8% of execution time in RDBMSs is "useful work."
  - Overhead distribution:
    - Buffer Management: 34.6%.
    - Latching: 14.2%.
    - Locking: 16.3%.
    - Logging: 1.9%.
    - Hand-Coded Optimizations: 16.2%.

### 6.1 Implications for NoSQL
- Simpler architectures than RDBMSs.
- Focus on high throughput and scalability.
- Less emphasis on strict ACID compliance.