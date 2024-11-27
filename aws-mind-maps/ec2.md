# AWS EC2 Mind Map

## 1. Introduction
- **What is EC2?**
  - Amazon Elastic Compute Cloud (EC2) is a web service providing scalable computing capacity in the cloud.
  - Enables users to run virtual servers (instances) without owning physical hardware.

## 2. Key Concepts
- **Instances**
  - Virtual machines running in the AWS cloud.
- **Regions**
  - Geographical areas where EC2 resources are hosted.
- **Availability Zones (AZs)**
  - Isolated locations within regions offering fault tolerance.
- **AMI (Amazon Machine Image)**
  - Pre-configured templates for launching instances.
  - Includes OS, application server, and application software.
- **Elastic Block Store (EBS)**
  - Persistent storage for EC2 instances.

## 3. Instance Types
- **General Purpose**
  - Balanced compute, memory, and networking.
  - Examples: `t3`, `m6i`.
  - Use Case: Web servers, app servers, small databases.

- **Compute Optimized**
  - High performance for compute-intensive tasks.
  - Examples: `c6g`, `c7g`.
  - Use Case: High-performance computing (HPC), gaming servers.

- **Memory Optimized**
  - For workloads needing high memory.
  - Examples: `r6i`, `x2idn`.
  - Use Case: In-memory databases, real-time analytics.

- **Storage Optimized**
  - High throughput for workloads needing intensive I/O.
  - Examples: `i4i`, `d3en`.
  - Use Case: NoSQL databases, large data warehousing.

- **Accelerated Computing**
  - Uses GPUs or FPGAs for high-performance tasks.
  - Examples: `p4de`, `g5`.
  - Use Case: Machine learning, deep learning, video rendering.

## 4. Internal Working
- **Hypervisors**
  - AWS uses Xen and Nitro hypervisors to virtualize hardware.
- **Elastic IP**
  - Static IPv4 address to allow instances to communicate with the internet.
- **Networking**
  - Instances are launched in a **VPC** (Virtual Private Cloud).
  - Can have public or private IPs.
- **Scaling**
  - Supports vertical scaling (changing instance type) and horizontal scaling (adding/removing instances).

## 5. Configuration
- **Launch Process**
  1. Select an AMI.
  2. Choose an instance type.
  3. Configure instance details (VPC, subnet, storage, tags).
  4. Add storage (EBS volumes).
  5. Configure security groups (firewall rules).
  6. Launch instance.

- **Security**
  - Use **Security Groups** for inbound/outbound traffic control.
  - Use **IAM Roles** to assign permissions to instances.
  - Key pairs for SSH access.

- **Auto Scaling**
  - Automatically adds or removes instances based on demand.
  - Requires an Auto Scaling Group (ASG) and Launch Template.

## 6. Pricing Model
- **On-Demand**
  - Pay by the second or hour.
  - Best for short-term, unpredictable workloads.

- **Reserved Instances**
  - Commit to 1 or 3 years for lower pricing.
  - Best for steady, predictable workloads.

- **Spot Instances**
  - Bid for unused capacity at reduced prices.
  - Best for fault-tolerant, flexible workloads.

- **Savings Plans**
  - Flexible pricing based on usage commitment over time.
  - Lower costs compared to On-Demand.

- **Dedicated Hosts**
  - Physical servers fully dedicated to you.
  - Useful for regulatory or licensing requirements.

## 7. When and How to Use EC2
- **When to Use**
  - Hosting websites and web applications.
  - Running custom software or legacy systems.
  - Running workloads requiring fine-grained control over compute.

- **How to Use**
  1. Choose the right instance type for your workload.
  2. Configure networking and storage.
  3. Scale instances manually or automatically.
  4. Use tags to organize instances for billing and management.
  5. Monitor performance using **CloudWatch**.

## 8. Additional Features
- **Elastic Load Balancing (ELB)**
  - Distributes traffic across EC2 instances.
- **Monitoring**
  - Use Amazon CloudWatch for CPU, memory, and disk usage metrics.
- **Backups**
  - Automate EBS snapshots for instance backups.
- **Elastic Beanstalk**
  - Simplifies application deployment by managing EC2 instances automatically.

## 9. Best Practices
- Use Auto Scaling for high availability.
- Apply IAM roles for secure access.
- Monitor costs and use Savings Plans or Reserved Instances for optimization.
- Place critical applications in multiple AZs for fault tolerance.
- Regularly patch and update instances for security.

---

This markdown structure provides a comprehensive overview of AWS EC2, explaining its key features, use cases, and configurations. Let me know if you need detailed examples or further elaboration on specific topics!