# Systems Design for Interviews – Mind Map

## 1. Networks and Protocols
  - **Networks**: Connections
  - **Protocols**: rules that govern something, official procedure.
- **IP (Internet Protocol)**
  - Numeric label(Protocol) assigned to devices over Internet for communication.
  - Messages sent in **packets**
    - **Definition of Packets**
      - small chunk of information (2^16 bytes)
    - **Components**
      - headers (source/destination IP addresses)
      - data (message/information)
  - **IPv4** vs. **IPv6** (to handle IP exhaustion).
  - **Demerits (Corrupted Data)**
    - Loss or dropped packets
    - Unordered packet delivery

- **TCP (Transmission Control Protocol)**
  - Built on top of IP to ensure reliable, ordered packet delivery.
  - Uses **handshakes** to establish and close connections.
  - **Message Components**
    - IP Header: From/To addresses
    - TCP Header: Ordering of packets, no. of packets etc
    - Data: Information

- **HTTP/HTTPS (Hypertext Transfer Protocol)**
  - Client requests, Server responds
  - Protocol for web communication using a **request-response model**.
  - Supports **verbs** like GET, POST, DELETE, PATCH/PUT, TRACE, OPTIONS, HEAD

---

## 2. Storage, Latency, and Throughput
- **Storage Types**
  - **Memory (RAM):** Fast but volatile(data lost in Power OFF), costly.
  - **Disk Storage:** Persistent but slower (used for databases).

- **Latency**
  - Time taken (duration) for an operation (e.g., request-response cycle, searching in dict than array).
  - Lower latency means faster response time.
  - Affected by Client-Server distance

- **Throughput**
  - Amount of data processed in a given time (e.g., Mbps or requests/second). Max capacity of a system.
  - Higher throughput means better scalability improving performance.
  - System is limited by its lowest throughput (bottleneck)

---

## 3. System Availability
- **High Availability (HA)**
  - Achieved by reducing **single points of failure** and adding redundancy.
  - Fault-tolerant system: robust to handle failures in the networks, databases, servers etc.

- **SLAs (Service Level Agreements)**
  - Commitments to uptime (e.g., 99.99% availability).
  - 99.9% uptime means 8.77 hours of DOWNTIME per year
  - 99.99% uptime means 52.6 mins DOWNTIME per year
  - 99.999% uptime means approx 5 mins DOWNTIME per year

- **Designing HA Systems**
  - Use **replication** and **load balancing** to ensure resilience.
  - Avoid Single Point of Failures, introduce redundancy.

---

## 4. Caching and CDNs
- **Caching**  
  - Stores frequently used data for fast access (reduces latency).
  - Reduces Database overload
  - Reduces Network calls to Database
  - Fast access compared to Disk/Database
  - Use cache miss and eviction strategies according to use-case

- **Content Delivery Networks (CDNs)**
  - Distribute content across global servers to reduce load and latency.
  - Acts as mini-servers to quickly serve requests from a closer geographical location.

---

## 5. Proxies
- **Forward Proxy**
  - Acts on behalf of a client (e.g., VPNs).
  - Hides client's IP, geographical location.

- **Reverse Proxy**
  - Works on behalf of a server, often handling **load balancing** and **security**.
  - Hides backend servers from public access, (internal servers) can have private IPs and no encryption etc for optimisation and free flow.

---

## 6. Load Balancing
- **Algorithms**  
  - Round-robin, Least connections, Load Based, Mixed Bag, Path or Service based, IP-hash, Weighted Round-robin, Server statuses, Alphabetic partition.
- **Use Case:** Distributes traffic across multiple servers to prevent overload.
- Mainly introduced to regulate n/w calls or communications like client-load balancer-backend, backend-load balancer-database, backend-load balancer-caches etc.
- Can act as rate limiter, reverse proxy server too.
- Ensure Load Balancers redundancy, fail-over, fail-back to better scalability and reduce fault-tolerance.

## 7. Hashing
- **Hashing:** Puts universe of keys to fixed size bags.
- Collision Handling: Chaining, Open Addressing: Linear, Quadratic and Double Hashing/Probing.
- **Cons:**
  - server fails, traffic still gets routed to it.
  - High data redistribution with any change in node count, wastes previous caches.
  - No ring structure; applies hashes directly to data.

## 8. Consistent Hashing
- Ring structure; server is hashed and placed at multiple locations for uniform distribution/routing of requests.
- Servers at different locations in a ring can be called nodes
- A server/node fails, requests are redirected to the next node in loop.
- To achieve optimum uniformity in load distribution, hashed requests can be routed to hashed-server/node
- Introduced to backend-servers and caches.
- Addition or removal of servers has least impact on the system.

## 9. Databases
- **Relational**
  - Strictly enforced relationships like rows and columns.
  - Schema: Classic relational database or formalized entity structure.
  - Data being inserted should conform to the predefined schema.
  - SQL: Structured Query Language, a language designed to interact with structured (relational) database.
  - Databases itself manages these queries(executes them) and returns matching results.
  - Follows ACID property
    - **Atomicity:** One operation fails, entire transaction fails. All or nothing.
    - **Consistency:** Every read operation receives the most recent write operation result. Full-synchronisation.
    - **Isolation:** you can "concurrently" (at the same time) run multiple transactions on a database, but the database will end up with a state that looks as though each operation had been run serially ( in a sequence, like a queue of operations).
    - **Durability:** data stored in DB is persistent (ROM or disk), not in-memory (RAM).
- **Non-relational**
  - Less rigid, more flexible structure like key-value pairs.
  - Follows BASE
    - **Basically Available:** system guarantess availability.
    - **Soft State:** DB/table state may change over time, even without input.
    - **Eventual Consistency:** states that the system will become consistent over a (very short) period of time unless other inputs are received.
  - At core, DB holds data in a hash-table-like structure (thus, extremely fast, simple and easy to use).
  - Perfect for use-cases like caching, environment variables, configuration files and session state etc.
  - Memcached (in-memory), DynamoDB (persistent)



---

This mind map captures the core concepts, protocols, and strategies mentioned in the article, making it easier to recall key topics relevant to system design interviews. For more details, explore the full article [here](https://www.freecodecamp.org/news/systems-design-for-interviews/).