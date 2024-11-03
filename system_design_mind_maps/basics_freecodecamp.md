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
  - Data retrieval from cache is faster than from the disk/database.
  - Backend's computationally intensive and time consuming work could be reduced through caching.
  - Cache data should be in sync with the Database
  - Reduce and Handle Cache misses
  - Data Eviction like LRU, LFU, LIFO, FIFO

- **Content Delivery Networks (CDNs)**
  - Distribute content across global servers to reduce load and latency.

---

## 5. Proxies
- **Forward Proxy**
  - Acts on behalf of a client (e.g., VPNs).

- **Reverse Proxy**
  - Works on behalf of a server, often handling **load balancing** and **security**.
  - Hides backend servers from public access, can have private IPs and no encryption etc for optimisation and free flow.

---

## 6. Load Balancing
- **Algorithms**  
  - Round-robin, Least connections, Load Based, Mixed Bag, Path or Service based, IP-hash, Weighted Round-robin, Server statuses, Alphabetic partition.
- **Use Case:** Distributes traffic across multiple servers to prevent overload.
- Can be introduced at multpile points like Backend Server, Databases, Caches.
- Can limit number of requests per minute, per user etc to avoid DOS/DDOS attacks.
- Ensure Load Balancers redundancy, fail-over, fail-back to better scalability and reduce fault-tolerance.



---

This mind map captures the core concepts, protocols, and strategies mentioned in the article, making it easier to recall key topics relevant to system design interviews. For more details, explore the full article [here](https://www.freecodecamp.org/news/systems-design-for-interviews/).