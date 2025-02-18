# **Amazon Dynamo Summary**

Amazon **Dynamo** is a highly available, distributed **key-value** store, **non-relational** (relatively **small objects**) designed for low-latency and scalable applications. It provides **eventual consistency** while ensuring **high availability** and **fault tolerance** through decentralized techniques. **PA/EL system**. To make it **strong consistency**: drop sloppy quorum, **W+R>N**, node failure -> system failure.

## **Key Characteristics**

- **Decentralized & Peer-to-Peer:** Uses a **ring-based architecture** with no central coordinator.
- **Eventual Consistency:** Ensures **high availability** by allowing temporary inconsistencies that eventually resolve.
- **Partitioning & Replication:** Uses **consistent hashing** to distribute and replicate data across nodes.
- **Highly Available Writes:** Uses **quorum-based techniques** and **hinted handoff** to allow writes even during node failures. **(always writable data store)**
- **Tunable Consistency:** Clients can configure **R (reads), W (writes), and N (replicas)** to balance consistency vs. availability.

## **Main Mechanisms in Dynamo**

### **1. Consistent Hashing (Partitioning & Replication)**

- Dynamo **distributes data across nodes** using **consistent hashing**, reducing data movement when nodes join or leave.
- Each data item is **replicated across N nodes** for fault tolerance.

### **2. Hinted Handoff (Handling Temporary Failures)**

- If a **replica node is temporarily unavailable**, writes are **stored on another node** with a “hint.”
- When the failed node recovers, the substitute **forwards the stored data**, ensuring durability without immediate consistency loss.

### **3. Quorum-Based Reads & Writes**

- Uses **R/W/N** parameters for flexible consistency tuning:
  - **N:** Number of replicas per data item.
  - **W:** Number of replicas that must acknowledge a write.
  - **R:** Number of replicas that must respond to a read.
- Ensures **availability** even if some nodes are unreachable.

### **4. Vector Clocks (Conflict Resolution)**

- Each update is **versioned** using **vector clocks** to track causality.
- If conflicting versions exist, Dynamo **returns multiple versions to the client** for resolution.

### **5. Anti-Entropy (Merkle Trees for Data Repair)**

- Uses **Merkle trees** to efficiently detect and synchronize inconsistencies across replicas.
- Helps resolve **divergent replicas** without full data scanning.

### **6. Sloppy Quorum (Handling Network Partitions)**

- If a write cannot meet **W quorum** due to partitioned nodes, Dynamo writes to **the next available nodes** (fallback replicas).
- Prevents system unavailability even in extreme network failures.

## **Why Dynamo? (Benefits)**

✅ **Highly Available** – Designed for **always-on services** (e.g., Amazon shopping cart).  
✅ **Scalable** – Handles dynamic **node additions/removals** seamlessly.  
✅ **Fault Tolerant** – Survives **node failures, network partitions**, and **high request loads**.  
✅ **Customizable Consistency** – Clients can tune **R, W, N** for their use case.

## **Conclusion**

Dynamo is a pioneering **NoSQL key-value store** that inspired modern distributed databases (e.g., **Cassandra, Riak**). It prioritizes **availability and partition tolerance** over strict consistency, making it ideal for **large-scale, fault-tolerant applications**.

### **Functional Requirements for Amazon Dynamo**

1. **Distributed Key-Value Store** – Uses a key-value model for fast lookups.
2. **State storage**
3. **Persistent data storage**
4. **Eventual Consistency & Tunable Consistency** – Allows trade-offs between consistency and availability.
5. **Failure Detection & Recovery** – Uses gossip-based membership protocols.
6. **Data Replication** – Supports synchronous and asynchronous replication.
7. **Partitioning & Load Balancing** – Uses virtual nodes for even data distribution.
8. **Versioning & Conflict Resolution** – Handles concurrent updates using vector clocks.

### **Non-Functional Requirements for Amazon Dynamo**

9. **High Availability** – Designed to avoid single points of failure, running across multiple nodes.
10. **Scalability** – Scales horizontally by adding nodes dynamically.
11. **Fault Tolerance** – Uses multiple replicas to recover from node failures.
12. **Low Latency** – Optimized for fast reads/writes with predictable performance.
13. **Durability** – Ensures data persistence across failures.
14. **Decentralized Architecture** – No single coordinator, reducing bottlenecks.
15. **Observability** – Monitors system state using distributed logging and health checks.
