# Consistent Hashing Mind Map

## 1. Introduction to Consistent Hashing
- **Definition**: A hashing technique for distributing data across nodes (servers) in a way that minimizes the impact of adding or removing nodes.
- **Goal**: Balance the load across distributed systems with minimal data reallocation.

---

## 2. Key Concepts in Consistent Hashing

### 2.1 Hash Function
- **Purpose**: Maps data keys to a numeric space (0 to maximum value).
- **Consistent Hashing Hash Function**:
  - Ensures that adding or removing nodes requires only minimal data movement.
  - Example: MD5, SHA-1 are commonly used in distributed systems.

### 2.2 Hash Ring (Circular Hash Space)
- **Structure**: The hash space is visualized as a circle (or ring), where both data and nodes are assigned hash values.
- **Data Mapping**: Each key is mapped to the first node with a hash greater than or equal to the key's hash.
- **Advantage**: Adding or removing nodes only affects nearby data points on the ring, reducing disruption.

### 2.3 Data Distribution
- **Data Movement**: When nodes are added or removed, only keys within the affected range are redistributed.
- **Load Balancing**: Nodes handle a subset of keys, reducing hotspots and balancing load across the system.

---

## 3. Virtual Nodes (VNodes)

- **Definition**: Multiple “virtual” instances of each physical node on the hash ring to improve balance.
- **Purpose**: Better distribution of data by creating multiple points for each server on the ring.
- **Implementation**:
  - Each physical server manages several virtual nodes, improving resilience and reducing skew.
  - Example: A server might be assigned 10 or more virtual nodes, each evenly spread around the ring.

---

## 4. Advantages of Consistent Hashing

- **Minimal Data Movement**: Only affected keys are remapped when nodes are added/removed.
- **Scalability**: Scales easily as new nodes can be added without redistributing the entire data.
- **Fault Tolerance**: If a node fails, only the keys mapped to that node need to be reassigned.
- **Improved Load Balancing**: Virtual nodes enhance even distribution of data and prevent hotspots.

---

## 5. Disadvantages of Consistent Hashing

- **Complexity in Implementation**: Requires sophisticated hashing and balancing logic, especially with VNodes.
- **Potential Load Imbalance Without VNodes**: Without virtual nodes, some nodes may end up with significantly more data.
- **Dependency on Hash Function**: Choice of hash function affects efficiency and load distribution.

---

## 6. Applications of Consistent Hashing

- **Distributed Caching**: Used in caching systems like Redis and Memcached to distribute cached data.
- **Content Delivery Networks (CDNs)**: Balances content requests across multiple servers, improving access speed.
- **Distributed Databases**: In databases like Cassandra and DynamoDB to distribute partitions across nodes.

---

## 7. Conclusion
- **Consistent Hashing**: Essential for scalable, distributed systems where nodes may join or leave dynamically.
- **Future-Ready**: Ensures systems can handle scaling demands with minimal data movement and load balancing.