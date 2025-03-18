# CAP Theorem Mind Map

## 1. Introduction to CAP Theorem
- **Origin**  
  - Proposed by Eric Brewer in 2000 at PODC conference.
  - Proven by Seth Gilbert & Nancy Lynch in 2002.

- **Definition**  
  - Impossible to achieve **Consistency (C)**, **Availability (A)**, and **Partition Tolerance (P)** simultaneously in a distributed system.
  - Choose 2 out of 3 in real-world scenarios.

---

## 2. Key Concepts

### 2.1 Consistency (C)
- Ensures all nodes see the same data at the same time.
- Example: All reads reflect the latest write.

### 2.2 Availability (A)
- Every request (read/write) receives a response, even if it’s not the most up-to-date.

### 2.3 Partition Tolerance (P)
- System continues to operate despite message loss or failure in network communication.

---

## 3. Trade-offs in CAP

### 3.1 CA Systems
- Strong Consistency & Availability but no Partition Tolerance.
- Rare in distributed environments.

### 3.2 CP Systems
- Strong Consistency & Partition Tolerance but may compromise Availability.
- Example: HBase.

### 3.3 AP Systems
- Availability & Partition Tolerance but relaxed Consistency.
- Example: Cassandra, DynamoDB.

---

## 4. Real-World Implications

### 4.1 Scenarios - Consistency or Availability
- **Critical Condition:** During a partition, trade-offs must be made.
  - Maintain **Availability** with stale data (AP).
  - Maintain **Consistency** by denying requests (CP).

### 4.2 Design Decisions
- Engineers must prioritize based on system requirements:
  - Banking: Prefer CP.
  - Social Media Feeds: Prefer AP.

---

## 5. Misconceptions and Clarifications

### 5.1 Partition Tolerance is Mandatory
- Real-world distributed systems must handle partitions.

### 5.2 "CA" Misunderstanding
- Systems labeled CA often ignore potential partitions.

### 5.3 CAP is Not Binary
- Trade-offs depend on probability and severity of partition events.

---

## 6. Relationship to Other Theorems

### 6.1 FLP Theorem
- Consensus is impossible in fully asynchronous systems with even one faulty node.

### 6.2 ACID vs. CAP
- CAP focuses on distributed systems.
- ACID applies to transactional databases, prioritizing atomicity and isolation.

---

## 7. For a 5-Year-Old
- Imagine you have a magic mailbox (Consistency), and it always delivers letters fast (Availability), even if there's a storm that blocks the road (Partition Tolerance). CAP says you can only pick two of these features in a game.

---

## References
- [CAP FAQ on GitHub](https://github.com/henryr/cap-faq)
- [CAP and Distributed Systems](https://en.wikipedia.org/wiki/CAP_theorem)