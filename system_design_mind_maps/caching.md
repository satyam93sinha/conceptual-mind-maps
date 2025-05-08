# Caching in Distributed Systems Mind Map

## 1. Introduction to Caching  
- **Caching**: Temporary storage of frequently accessed data to reduce latency and load.
- **Goal**: Improve application performance and scalability.

---

## 2. Cache Architecture  
- **Read-Through Cache**  
  - Data fetched from cache; if not present (cache miss), Cache fetches data from database, updates cache then returns to Application.
  - Application may see Cache as the primary source of Data.  
  - **Use Case**: Simplifies access logic.

- **Write-Through Cache**  
  - Data written to both cache and database simultaneously. 
  - Most of the written data may never be used, exploiting space. 
  - **Use Case**: Ensures data consistency.

- **Write-Behind Cache**  
  - Writes are queued and processed asynchronously.  
  - **Use Case**: Better performance but higher risk of data loss.

- **Cache-Aside (Lazy Loading)**  
  - Application directly interacts with cache; data is loaded only on a cache miss.
  - **Use Case**: Used when the cost of stale data is low.

---

## 3. Cache Eviction Policies  
- **1. Least Recently Used (LRU)**: Removes least accessed items.
- **2. Least Frequently Used (LFU)**: Removes items with the lowest access frequency.
- **3. First-In-First-Out (FIFO)**: Evicts the oldest items.

---

## 4. Challenges in Distributed Caching  
- **Cache Consistency**: Ensuring cache and database stay in sync.
- **Cache Invalidation**: Removing stale data from cache.
- **Data Partitioning**: Distributing cached data across multiple nodes.
- **Cache Warm-up**: Preloading popular data to prevent cold starts.

---

## 5. Cache Consistency Models  
- **Strong Consistency**: Immediate sync between cache and database but with latency overhead.
- **Eventual Consistency**: Cache syncs over time, better for distributed systems.
  
---

## 6. Tools for Distributed Caching  
- **Redis**: In-memory key-value store with persistence support.
- **Memcached**: Lightweight, high-performance caching solution.

---

## 7. Conclusion  
- **Caching Strategy**: Choose based on system needs—balancing performance, consistency, and complexity.
- **Distributed Caching**: Essential for handling scalability challenges in large-scale systems.