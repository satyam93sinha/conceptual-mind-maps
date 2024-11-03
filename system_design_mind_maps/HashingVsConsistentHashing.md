# Hashing vs Consistent Hashing Mind Map

## 1. Definition
- **Hashing**:  
  - Converts data to fixed-size values (hashes).
  - Primarily used for fast retrieval and indexing.

- **Consistent Hashing**:  
  - Specialized hashing for distributed systems.
  - Maps data to nodes (servers) to minimize data reallocation with node changes.

---

## 2. Application
- **Hashing**:
  - Used in static environments, hash tables, cryptographic functions.
  
- **Consistent Hashing**:
  - Ideal for distributed systems (e.g., caching, databases).
  - Balances data and minimizes disruption with dynamic node changes.

---

## 3. Data Redistribution
- **Hashing**:
  - High data redistribution with any changes in node count.
  
- **Consistent Hashing**:
  - Minimal data movement—only affects nearby data on the hash ring.

---

## 4. Hash Ring Structure (Consistent Hashing Only)
- **Hashing**:
  - No ring structure; applies hash function directly to data.
  
- **Consistent Hashing**:
  - Uses a circular hash space (hash ring), enabling even data distribution and fault tolerance.

---

## 5. Load Balancing and Virtual Nodes
- **Hashing**:
  - No inherent load-balancing; may lead to uneven distribution.
  
- **Consistent Hashing**:
  - Utilizes **Virtual Nodes (VNodes)** to ensure balanced data distribution across nodes.

---

## 6. Examples and Use Cases
- **Hashing**:
  - Applications in **Hash Tables**, cryptographic functions (SHA-256, MD5).
  
- **Consistent Hashing**:
  - Distributed caching (Redis, Memcached), CDNs, distributed databases (DynamoDB, Cassandra).

---

## 7. Advantages and Limitations
- **Hashing**:
  - **Pros**: Simple, fast, efficient.
  - **Cons**: Lacks support for dynamic systems with variable nodes.
  
- **Consistent Hashing**:
  - **Pros**: Scalable, dynamic environment support, minimal data reallocation.
  - **Cons**: More complex implementation, requires VNodes and suitable hash functions.