# CS75 Lecture 9: Scalability Mind Map

## 1. **Vertical vs Horizontal Scaling**
   - **Vertical Scaling (Scale Up)**
     - *Definition*: Adding more resources (CPU, RAM) to a single server
     - *Benefits*:
       - Simpler architecture
       - No distributed systems complexity
     - *Limitations*:
       - Physical hardware limits
       - Single point of failure
       - Cost grows exponentially

   - **Horizontal Scaling (Scale Out)**
     - *Definition*: Adding more servers to distribute load
     - *Benefits*:
       - Near-limitless scaling
       - Fault tolerance
       - Cost-effective at scale
     - *Challenges*:
       - Complexity of distributed systems
       - Data consistency issues
       - Network overhead

## 2. **Load Balancing**
   - **Terminologies**:
     - *Round Robin*: Cyclic distribution of requests
     - *Sticky Sessions*: Maintaining user-server affinity
     - *Health Checks*: Monitoring server availability

   - **Implementation Approaches**:
     ```python
     # Simplified Round Robin Algorithm
     servers = ['server1', 'server2', 'server3']
     current = 0
     def get_server():
         nonlocal current
         server = servers[current]
         current = (current + 1) % len(servers)
         return server
     ```

   - *Tricky Aspects*:
     - Session persistence challenges
     - Uneven load distribution
     - DNS caching issues

## 3. **Database Scaling**
   - **Replication Models**:
     - *Master-Slave*:
       - Write to master, read from slaves
       - *Consistency Issue*: Replication lag
     - *Master-Master*:
       - Bidirectional replication
       - *Conflict Risk*: Write-write collisions

   - **Partitioning Strategies**:
     - *Vertical Partitioning*: By tables/columns
     - *Horizontal Partitioning (Sharding)*: By row ranges
       - *Key Challenge*: Cross-shard queries

## 4. **Caching Strategies**
   - **Cache Types**:
     - *Application Cache*: Memcached, Redis
     - *CDN Cache*: Edge location caching
     - *Browser Cache*: Local storage

   - *Eviction Policies*:
     - LRU (Least Recently Used)
     - LFU (Least Frequently Used)
     - FIFO (First In First Out)

   - *Tricky Statement*:
     "Cache invalidation is one of the two hard problems in CS" (along with naming things)

## 5. **Asynchronous Processing**
   - **Message Queues**:
     - *Components*:
       - Producers
       - Consumers
       - Brokers (RabbitMQ, Kafka)
     - *Benefits*:
       - Decoupling
       - Load leveling
       - Fault tolerance

   - *Challenge*:
     "Exactly-once delivery is impossible in distributed systems" (Two Generals Problem)

## 6. **CAP Theorem**
   - *Definition*: Distributed systems can only guarantee 2 of 3:
     - **C**onsistency
     - **A**vailability
     - **P**artition tolerance

   - *Real-world Implications*:
     - CA: Traditional SQL databases
     - AP: NoSQL (Cassandra)
     - CP: Distributed locks (Zookeeper)

## 7. **Content Delivery Networks**
   - *How They Work*:
     ```mermaid
     graph LR
     User -->|1. Request| Edge_POP
     Edge_POP -->|2. Cache Miss| Origin
     Origin -->|3. Response| Edge_POP
     Edge_POP -->|4. Cached Response| User
     ```

   - *Benefits*:
     - Reduced latency
     - Lower origin server load
     - DDoS protection

## 8. **Performance Metrics**
   - *Key Terms*:
     - Throughput: Requests/second
     - Latency: Time per request
     - Percentiles: P50, P95, P99

   - *Tricky Observation*:
     "Optimizing for average latency can hide worst-case scenarios"

## 9. **Limitations Discussed**
   - *Amdahl's Law*:
     "The speedup of a program using multiple processors is limited by the sequential fraction"
   - *Thundering Herd*:
     Cache expiration causing simultaneous requests
   - *Cost Complexity*:
     Hidden operational costs of distributed systems

## 10. **Comparative Analysis**
   | Technique          | Best For            | Worst For           |
   |--------------------|---------------------|---------------------|
   | Vertical Scaling   | Predictable loads   | Rapid growth        |
   | Horizontal Scaling | Variable loads      | Legacy systems      |
   | Database Sharding  | Large datasets      | Complex transactions|

## 11. **Key Takeaways**
   - "Scaling is iterative, not one-time"
   - "Design for failure - everything fails eventually"
   - "Measure before optimizing"
   - "Sometimes buying better hardware is the right solution"

## 12. **Lecture Resources**
   - [Original Slides](https://cs75.harvard.edu/slides)
   - Recommended Reading:
     - "Designing Data-Intensive Applications" by Kleppmann
     - Google's SRE Handbook