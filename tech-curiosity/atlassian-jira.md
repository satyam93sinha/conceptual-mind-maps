# Mind Map: Scaling Low Latency, Multi-Region Services on AWS

## 1. Introduction
   - **Context**: Atlassian's migration to AWS in 2016.
   - **Objective**: Develop a globally distributed context service with low latency and high resilience.

## 2. Architectural Challenges
   - **State Management**:
     - Building stateless services requires handling state efficiently.
     - Offloading state to databases with partition keys per tenant.
   - **Jira's Requirements**:
     - Minimize blast radius and noisy neighbor issues.
     - Enable horizontal scaling with globally distributed tenant groups (shards).
     - Maintain existing DB-per-tenant model during AWS migration.

## 3. Centralized Context Service
   - **Purpose**: Maintain a global mapping of tenants to their respective shards/DBs.
   - **Functionality**:
     - Provide necessary context for network ingress and Jira services.
     - Ensure low-latency access to context information.

## 4. Optimizations Implemented
   - **Data Partitioning**:
     - Partition data to improve access times and reduce latency.
   - **Caching Strategies**:
     - Implement caching to minimize repeated data retrieval.
   - **Load Balancing**:
     - Distribute requests evenly across servers to prevent overload.
   - **Asynchronous Processing**:
     - Handle tasks asynchronously to improve response times.

## 5. Problems Faced and Solutions
   - **High Latency Issues**:
     - **Problem**: Increased latency due to distant data centers.
     - **Solution**: Deploy services closer to users to reduce latency.
   - **Data Consistency Challenges**:
     - **Problem**: Maintaining consistency across distributed databases.
     - **Solution**: Implement robust synchronization mechanisms.
   - **Scalability Concerns**:
     - **Problem**: Handling increased load without performance degradation.
     - **Solution**: Utilize auto-scaling features of AWS.

## 6. Suggestions and Best Practices
   - **Monitoring and Metrics**:
     - Continuously monitor performance and set up alerts.
   - **Regular Testing**:
     - Conduct load and stress testing to identify potential bottlenecks.
   - **Incremental Deployment**:
     - Deploy changes gradually to monitor impact and rollback if necessary.