# Snowflake Elastic Data Warehouse: Summary

## Overview

Snowflake is a **cloud-native data warehouse** designed to address limitations of traditional systems in elasticity, semi-structured data handling, and operational complexity. It combines a **multi-cluster, shared-data architecture** with a pure SaaS model, offering ANSI SQL compliance, ACID transactions, and seamless scalability. **CP system: ACID compliant (transactional consistency)**.

---

## Key Characteristics

### 1. **Decoupled Storage & Compute**

- **Storage**: Uses cloud object storage (e.g., Amazon S3) for immutable, compressed table files in a hybrid columnar format (PAX).
- **Compute**: Ephemeral "Virtual Warehouses" (VWs) scale independently. Users pay only for compute used.

### 2. **Elastic Scalability**

- Compute (VWs) can be dynamically scaled up/down or paused. Storage scales automatically without data movement.

### 3. **Semi-Structured Data Support**

- **VARIANT type**: Stores JSON, Avro, etc., with automatic schema inference and columnar storage optimizations.
- **Optimistic conversion**: Converts semi-structured data to typed columns while preserving raw data.

### 4. **High Availability & Durability**

- Tolerates node, cluster, and AZ failures. S3 ensures 99.999999999% durability.
- **Online upgrades**: No downtime; multiple service versions run side-by-side.

### 5. **Security**

- **End-to-end encryption**: AES-256 with a hierarchical key model (root → account → table → file keys).
- **RBAC**: Fine-grained access control at the SQL level.

### 6. **Cost Efficiency**

- Pay-as-you-go pricing for storage and compute. Compute can be paused to reduce costs.

---

## Architecture Layers

### 1. **Data Storage (S3)**

- **Immutable files**: Tables are stored as compressed, columnar files in S3.
  - Potential limitation: file can only be overwritten
- **Metadata**: Stored in a transactional key-value store (part of Cloud Services).
- **(-) Higher access latency than local storage**: compute caches table data
### 2. **Virtual Warehouses (Compute)**

- **Ephemeral clusters**: EC2 instances execute queries. Each VW is isolated and auto-scales.
- **Caching**: Worker nodes cache frequently accessed data locally (LRU policy).
- **File stealing**: Dynamically redistributes files during skew to balance workloads.

### 3. **Cloud Services (Metadata & Management)**

- **Query optimization**: Cascades-style optimizer with pruning (min-max metadata, Bloom filters).
- **Concurrency control**: Snapshot Isolation (SI) via MVCC. Each write creates a new table version.
- **Metadata management**: Handles transactions, security, and versioning.

---

## Core Mechanisms

### 1. **Execution Engine**

- **Columnar & vectorized**: Processes data in batches for CPU cache efficiency.
- **Push-based**: Operators push results downstream (vs. pull-based Volcano model).

### 2. **Pruning**

- **Static**: Uses min-max values and Bloom filters to skip irrelevant files.
- **Dynamic**: Collects statistics during execution (e.g., hash join build-side keys) for runtime filtering.

### 3. **Time Travel & Cloning**

- **Time travel**: Access historical data via `AT`/`BEFORE` clauses (retains versions for up to 90 days).
- **Zero-copy cloning**: Creates instant metadata-based copies of tables/databases.

### 4. **Handling Semi-Structured Data**

- **Automatic schema inference**: Extracts common paths from JSON/XML into typed columns.
- **Flattening & nesting**: Converts hierarchical data to relational format via SQL lateral views.

---

## Key Features

### 1. **Pure SaaS Experience**

- No tuning knobs, indices, or physical design. Managed entirely via web UI or SQL.

### 2. **Continuous Availability**

- Fault tolerance across AZs. Failed queries auto-retry with standby nodes.

### 3. **Security Model**

- **Key rotation**: Automatically rekeys data without downtime.
- **AWS CloudHSM**: Root keys stored in hardware security modules.

### 4. **Performance**

- Near-relational speed for semi-structured data (10% overhead in TPC-H benchmarks).

---

## Advantages Over Traditional Systems

| **Aspect**      | **Traditional DW**       | **Snowflake**                    |
| --------------- | ------------------------ | -------------------------------- |
| **Scalability** | Fixed compute/storage    | Independent elastic scaling      |
| **Data Types**  | Relational only          | Semi-structured (JSON/XML) + SQL |
| **Maintenance** | Manual tuning, vacuuming | Fully automated                  |
| **Cost Model**  | Upfront licensing        | Pay-as-you-go/pay-what-you-use   |
| **Upgrades**    | Downtime required        | Zero downtime                    |

### **Functional Requirements for Snowflake Database**

1. **Data Storage & Query Execution** – Supports structured and semi-structured data storage with SQL-based querying.
2. **Data Sharing** – Enables secure, real-time data sharing without copying data.
3. **Automatic Scaling** – Dynamically scales compute resources based on workload.
4. **Multi-Cloud Support** – Operates across AWS, Azure, and Google Cloud.
5. **Time Travel & Fail-Safe** – Allows users to restore previous versions of data for recovery.
6. **Role-Based Access Control (RBAC)** – Provides granular user permissions.
7. **Zero-Copy Cloning** – Enables instant duplication of databases without additional storage.
8. **SaaS experience** - Resource allocation without user interaction.

### **Non-Functional Requirements for Snowflake Database**

9. **Performance & Latency** – Optimized query execution with adaptive caching.
10. **High Availability** – Built-in redundancy ensures minimal downtime.
11. **Security & Compliance** – Supports encryption, SOC 2, HIPAA, and GDPR compliance.
12. **Elasticity** – Seamlessly adjusts resources without downtime.
13. **Observability & Monitoring** – Provides query profiling, logging, and auditing.
14. **Cost Efficiency** – Uses per-second billing and workload-based scaling.
15. **Interoperability** – Supports integration with BI tools, ETL pipelines, and data lakes.
