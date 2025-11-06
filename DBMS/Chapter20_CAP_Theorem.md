# Chapter 20: CAP Theorem 📐

## Introduction 🎯

**Basic and one of the most important concept in Distributed Databases.**

**Useful to know this to design efficient distributed system for your given business logic.**

### What is CAP Theorem?
**The CAP theorem states that a distributed system can only provide two of three properties simultaneously: consistency, availability, and partition tolerance. The theorem formalises the tradeoff between consistency and availability when there's a partition.**

---

## Breaking Down CAP 🔍

### Let's first breakdown CAP

### 1. Consistency (C) 🔄

**Definition**:
**In a consistent system, all nodes see the same data simultaneously. If we perform a read operation on a consistent system, it should return the value of the most recent write operation.**

**Key Characteristics**:
- **All users see the same data** at the same time
- **Read operations return most recent write** result
- **Data replicated across nodes** maintains synchronization
- **No node returns stale data** to users

**How It Works**:
```
Write Operation Flow:
User → Node A (Write) → Node B (Replicate) → Node C (Replicate)
                                ↓
                         All nodes synchronized
                                ↓
Read Operation:
User → Any Node → Returns same data (most recent write)
```

**Example**: Banking system - when you deposit money, all nodes must show the updated balance immediately.

### 2. Availability (A) ✅

**Definition**:
**When availability is present in a distributed system, it means that the system remains operational all of the time. Every request will get a response regardless of the individual state of the nodes.**

**Key Characteristics**:
- **System remains operational** all the time
- **Every request gets a response** (success or failure)
- **System works even if multiple nodes down**
- **No guarantee of most recent write** data

**How It Works**:
```
Request Flow (Available System):
User Request → Load Balancer → Available Node → Response
                    ↕
               If one node fails:
User Request → Load Balancer → Another Available Node → Response
```

**Example**: Social media - users can always post and read, even if some data is slightly outdated.

### 3. Partition Tolerance (P) 🌐

**Definition**:
**When a distributed system encounters a partition, it means there's a break in communication between nodes. If a system is partition-tolerant, the system does not fail, regardless of whether messages are dropped or delayed between nodes within the system.**

**Key Characteristics**:
- **System doesn't fail** during network partitions
- **Handles message drops or delays** between nodes
- **Records replicated across combinations** of nodes and networks
- **Continues operating** despite communication breakdowns

**Network Partition Example**:
```
Normal Communication:
Node A ←─────────────→ Node B ←─────────────→ Node C

Network Partition:
Node A ←─✕─┐       ┌─✕─→ Node B ←─✕─┐
            │       │                │
         Partition      Partition
            │       │                │
Node A      └───────┼────────────────┘
                 Network Partition
Node B      └───────→ Node C (Still works)
```

**Why Partition Tolerance is Essential**:
- **Network failures are inevitable** in distributed systems
- **Cloud environments** experience network issues regularly
- **Geographic distribution** increases partition probability
- **System must continue** operating despite partitions

---

## The CAP Theorem Statement 📐

### Core Principle
**The CAP theorem states that a distributed system can only provide two of three properties simultaneously: consistency, availability, and partition tolerance.**

### The Triangle of Trade-offs
```
          Consistency (C)
              ▲
             / \
            /   \
           /     \
          /       \
         /         \
        /           \
       /             \
      /               \
     A ◄─────────────► P
 Availability     Partition Tolerance
```

### The Reality of Distributed Systems
**In any distributed system, partitions are bound to happen. This means you must choose between consistency and availability when a partition occurs.**

### The Trade-off Decision
```
When Network Partition Occurs:
                    ┌─────────────────┐
                    │ Network Partition│
                    └─────────────────┘
                            ↓
┌─────────────────────┐    ┌─────────────────────┐
│ Choose Consistency  │    │ Choose Availability │
│ (CP System)         │    │ (AP System)         │
│                     │    │                     │
│ • Stop responding   │    │ • Keep responding   │
│ • Wait for partition│    │ • Return stale data  │
│ • Maintain data     │    │ • Inconsistency     │
│   consistency       │    │   temporary         │
└─────────────────────┘    └─────────────────────┘
```

---

## CAP Theorem and NoSQL Databases 🗂️

### Why CAP Matters for NoSQL
**NoSQL databases are great for distributed networks. They allow for horizontal scaling, and they can quickly scale across multiple nodes. When deciding which NoSQL database to use, it's important to keep the CAP theorem in mind.**

### Database Classification by CAP Properties

## 1. CA Databases (Consistency + Availability) 📊

### Characteristics
**CA databases enable consistency and availability across all nodes. Unfortunately, CA databases can't deliver fault tolerance.**

### Why CA is Not Practical
**In any distributed system, partitions are bound to happen, which means this type of database isn't a very practical choice.**

### Implementation Notes
```
CA Database Architecture:
┌─────────────┐ ┌─────────────┐
│   Node A    │ │   Node B    │
│ (Consistent │ │ (Consistent │
│ + Available)│ │ + Available)│
└─────────────┘ └─────────────┘
        ↕ Communication
        ↕ No Partition Tolerance
        ↕ Single Point of Failure
```

### Examples
- **Relational databases**: MySQL, PostgreSQL
- **Single-node systems** or tightly coupled clusters
- **Traditional RDBMS** with replication

### Use Cases
- **Small-scale systems** where network partitions are rare
- **Single data center** deployments
- **Applications requiring** strong consistency and high availability

## 2. CP Databases (Consistency + Partition Tolerance) 🎯

### Characteristics
**CP databases enable consistency and partition tolerance, but not availability. When a partition occurs, the system has to turn off inconsistent nodes until the partition can be fixed.**

### How CP Systems Work
```
CP System During Partition:
┌─────────────┐ ┌─✕─ Partition ─✕─┐ ┌─────────────┐
│   Node A    │                   │   Node B    │
│ (Primary)   │                   │ (Secondary) │
│ Available   │                   │ Unavailable │
└─────────────┘                   └─────────────┘
        ↕                            ↕
   Responds to                   Rejects
   requests                    requests until
                                partition resolved
```

### MongoDB Example (CP Database)
**MongoDB is an example of a CP database. It's a NoSQL database management system (DBMS) that uses documents for data storage.**

**MongoDB Architecture**:
```
MongoDB Replica Set:
┌─────────────────────────────────────────────────────────────┐
│                      Primary Node                           │
│                 (Receives all writes)                       │
│ ┌─────────────────────────────────────────────────────┐   │
│ │                 Users Collection                    │   │
│ │ {"_id": 1, "name": "John", "balance": 1000}        │   │
│ │ {"_id": 2, "name": "Jane", "balance": 2000}        │   │
│ └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                                ↓
                           Replication
                                ↓
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   Secondary 1   │ │   Secondary 2   │ │   Secondary 3   │
│   (Replica)     │ │   (Replica)     │ │   (Replica)     │
│   Available     │ │   Available     │ │   Available     │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

**During Partition**:
- **Primary node** continues serving requests
- **Secondary nodes** may become unavailable
- **System maintains consistency** at cost of availability

### When to Choose CP
**In banking system Availability is not as important as consistency, so we can opt it (MongoDB).**

**Ideal Use Cases**:
- **Financial applications** (banking, trading)
- **Inventory management** systems
- **Order processing** where accuracy is critical
- **Applications where** data consistency is non-negotiable

## 3. AP Databases (Availability + Partition Tolerance) 🚀

### Characteristics
**AP databases enable availability and partition tolerance, but not consistency. In the event of a partition, all nodes are available, but they're not all updated.**

### How AP Systems Work
```
AP System During Partition:
┌─────────────┐ ┌─✕─ Partition ─✕─┐ ┌─────────────┐
│   Node A    │                   │   Node B    │
│   Available │                   │   Available │
│ (Version 1) │                   │ (Version 2) │
└─────────────┘                   └─────────────┘
        ↕                            ↕
   Responds with                   Responds with
   Version 1                      Version 2
        ↕                            ↕
   Both available                 Both available
   but inconsistent                but inconsistent
```

### Cassandra Example (AP Database)
**Apache Cassandra is an example of an AP database. It's a NoSQL database with no primary node, meaning that all of the nodes remain available.**

**Cassandra Architecture**:
```
Cassandra Ring Architecture:
    Node 1 ←→ Node 2 ←→ Node 3 ←→ Node 4 ←→ Node 1
      ↓        ↓        ↓        ↓        ↓
  Shard A  Shard B  Shard C  Shard D  Shard A

All nodes are equal (no primary node):
• Any node can accept writes
• Any node can serve reads
• Data replicated across ring
• Eventual consistency maintained
```

**Eventual Consistency in AP Systems**:
**Cassandra allows for eventual consistency because users can re-sync their data right after a partition is resolved.**

**Eventual Consistency Process**:
```
During Partition:
Node A: User Balance = $1000 (Original)
Node B: User Balance = $1200 (After deposit)

After Partition Resolution:
Sync Process:
Node A ↔ Node B → Resolve conflict → Final balance = $1200
```

### When to Choose AP
**For apps like Facebook, we value availability more than consistency, we'd opt for AP Databases like Cassandra or Amazon DynamoDB.**

**Ideal Use Cases**:
- **Social media platforms** (Facebook, Twitter)
- **Content delivery networks**
- **IoT applications** with high data volume
- **Real-time analytics** systems
- **Applications where** temporary inconsistency is acceptable

---

## RDBMS and CAP Theorem 🏛️

### Traditional RDBMS Position
**RDBMS databases are often at the CA side of the triangle. This is only the case in a single node setup.**

### Single Node RDBMS (CA)
```
Single Database Server:
┌─────────────────────────────────────────┐
│        Relational Database              │
│  ┌─────────┬─────────┬─────────────┐   │
│  │   Table │   Table │   Table     │   │
│  │   Users │ Orders  │ Products    │   │
│  └─────────┴─────────┴─────────────┘   │
│                                         │
│  • Consistent (ACID)                    │
│  • Available (Single point)             │
│  • Not Partition Tolerant               │
└─────────────────────────────────────────┘
```

### Master-Slave Setup Challenges
**Even with master (write) - slave (read) setup, the system is not CA.**

### Split-Brain Scenario
**If it is termed "CA" for some reason, and cannot recover from network partitions, then a split-brain scenario may happen, a new master is elected for the partition, and chaos ensues, possibly breaking the consistency of the system.**

**Split-Brain Example**:
```
Normal Operation:
Master ←──────→ Slaves
(Writes)       (Reads)

Network Partition:
Master (Partition A) ←─✕─→ Slaves (Partition B)
                                    ↓
                            New Master Elected
                                    ↓
                            Two Masters accepting writes
                                    ↓
                            Data Inconsistency! ❌
```

---

## CAP Trade-offs in Practice ⚖️

### Decision Matrix

| **Application Type** | **CAP Choice** | **Reasoning** | **Examples** |
|---------------------|----------------|---------------|--------------|
| **Banking Systems** | CP | Consistency critical for financial data | MongoDB, PostgreSQL |
| **Social Media** | AP | Availability paramount, temporary inconsistency acceptable | Cassandra, DynamoDB |
| **E-commerce** | CP/AP | Consistency for inventory, availability for browsing | MongoDB (CP), Cassandra (AP) |
| **Analytics** | AP | High availability for data ingestion | Cassandra, BigTable |
| **IoT Data** | AP | High volume, availability over consistency | Cassandra, InfluxDB |

### Practical Considerations

#### **When to Choose Consistency over Availability**
- **Financial transactions** where accuracy is non-negotiable
- **Inventory management** where overselling is unacceptable
- **User authentication** where security is critical
- **Regulatory compliance** requirements

#### **When to Choose Availability over Consistency**
- **Social media feeds** where recent data is nice but not critical
- **Logging systems** where data loss is unacceptable but slight delays are okay
- **CDN content delivery** where availability is paramount
- **Real-time analytics** where continuous data flow is essential

#### **The Reality of Partition Tolerance**
**Partition tolerance is not optional in distributed systems - it's a requirement. The real choice is between C and A when P occurs.**

---

## CAP Theorem Visualization 📊

### System State Diagrams

#### **CP System Behavior**
```
Normal State:        Partition State:
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Node A    │      │   Node A    │      │   Node A    │
│ Available   │ ←──→ │ Available   │ ←─✕─ │ Unavailable │
│ Consistent  │      │ Consistent  │      │ Consistent  │
└─────────────┘      └─────────────┘      └─────────────┘
       ↑                    ↑                    ↑
   All nodes             Some                 Node fails
   consistent            nodes                rather than
                         fail                return stale
                                              data
```

#### **AP System Behavior**
```
Normal State:        Partition State:         Recovery State:
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Node A    │      │   Node A    │      │   Node A    │
│ Available   │ ←──→ │ Available   │ ←─✕─ │ Available   │
│ Consistent  │      │ Version 1   │      │ Syncing     │
└─────────────┘      └─────────────┘      └─────────────┐
       ↑                    ↑                    ↑
   All nodes             All nodes             Nodes sync
   consistent            available             to achieve
                                              eventual
                                              consistency
```

### Response Time Comparison

| **System Type** | **Normal Response** | **Partition Response** | **Recovery Time** |
|-----------------|---------------------|------------------------|-------------------|
| **CP System** | Fast | Error/Timeout (reject requests) | Immediate when partition resolves |
| **AP System** | Fast | Fast (may return stale data) | Gradual sync after partition resolves |

---

## Interview Questions 🎯

### Q1: Explain the CAP theorem and its significance in distributed systems
**Answer**:
**CAP theorem states that distributed systems can only provide two out of three properties: Consistency, Availability, and Partition Tolerance. Since network partitions are inevitable in distributed systems, the real trade-off is between consistency and availability when partitions occur. This theorem is crucial for designing distributed systems as it helps architects make informed decisions about which properties to prioritize based on business requirements.**

### Q2: What are the three properties in CAP theorem? Explain each with examples
**Answer**:
**Consistency**: All nodes see the same data simultaneously. Every read returns the most recent write. Example: Banking system where account balances must be accurate across all nodes.

**Availability**: Every request receives a response, regardless of node state. System remains operational even during failures. Example: Social media where users can always access the platform.

**Partition Tolerance**: System continues operating despite network partitions between nodes. Example: Global distributed systems where network failures between regions are common.

### Q3: Why is partition tolerance not optional in distributed systems?
**Answer**:
**Partition tolerance is not optional because network failures are inevitable in distributed systems. In cloud environments and geographically distributed systems, network partitions occur regularly due to hardware failures, network congestion, or communication breakdowns. A system that cannot handle partitions would fail completely, making it unsuitable for distributed deployment. Therefore, architects must choose between consistency and availability when partitions occur.**

### Q4: Differentiate between CP and AP databases with examples
**Answer**:
**CP Databases (Consistency + Partition Tolerance)**:
- **Prioritize consistency** over availability during partitions
- **Stop responding** to maintain data consistency
- **Example**: MongoDB - primary node handles writes, secondaries replicate. During partition, inconsistent nodes become unavailable
- **Use case**: Banking systems where data accuracy is critical

**AP Databases (Availability + Partition Tolerance)**:
- **Prioritize availability** over consistency during partitions
- **Continue responding** with potentially stale data
- **Example**: Cassandra - all nodes available, eventual consistency
- **Use case**: Social media where platform availability is more important than immediate consistency

### Q5: When would you choose a CP database over an AP database?
**Answer**:
**Choose CP databases when:**
- **Data consistency** is business-critical (financial transactions, inventory management)
- **Stale data** could cause serious problems (account balances, stock quantities)
- **Regulatory requirements** demand strong consistency
- **Application can tolerate** temporary unavailability during partitions
- **Examples**: Banking systems, e-commerce inventory, order processing systems

### Q6: Explain split-brain scenario in distributed systems
**Answer**:
**Split-brain occurs when network partitions cause multiple nodes to believe they are the primary/master, leading to data inconsistencies. In a master-slave setup, if communication breaks, the slave might elect itself as master while the original master is still active. Both accept writes, creating divergent data sets that are difficult to reconcile. This violates consistency and can cause data corruption. That's why proper partition handling strategies are essential in distributed systems.**

### Q7: Why are traditional RDBMS considered CA systems?
**Answer**:
**Traditional RDBMS are considered CA systems only in single-node deployments where they provide strong consistency through ACID properties and high availability through reliable single-server operation. However, in distributed deployments, they cannot maintain the CA properties when network partitions occur. Even with master-slave replication, they face split-brain scenarios and must sacrifice either consistency or availability, making them unsuitable for truly distributed systems without additional complexity.**

### Q8: How does eventual consistency work in AP systems?
**Answer**:
**In AP systems, eventual consistency means that while nodes may return different data during partitions, they will eventually converge to the same state once the partition resolves. The system accepts writes during partitions and propagates updates to all nodes when communication is restored. During this sync period, different nodes may return different versions of data, but given enough time and no new updates, all nodes will reach consistency. This trade-off allows systems to remain available during partitions while still achieving consistency eventually.**

---

## Quick Reference Table 📋

| **CAP Type** | **Properties** | **Behavior During Partition** | **Examples** | **Use Cases** |
|--------------|----------------|-------------------------------|--------------|---------------|
| **CA** | Consistency + Availability | System fails (not partition tolerant) | Single-node RDBMS | Small applications, single data center |
| **CP** | Consistency + Partition Tolerance | Rejects requests to maintain consistency | MongoDB, HBase | Banking, inventory, financial systems |
| **AP** | Availability + Partition Tolerance | Returns stale data to remain available | Cassandra, DynamoDB | Social media, IoT, analytics |

| **Database** | **CAP Classification** | **Primary Use** | **Consistency Model** |
|--------------|------------------------|-----------------|---------------------|
| **MySQL** | CA (single node) | General purpose | Strong consistency |
| **PostgreSQL** | CA (single node) | General purpose | Strong consistency |
| **MongoDB** | CP | Document storage | Strong consistency |
| **Cassandra** | AP | Wide-column storage | Eventual consistency |
| **DynamoDB** | AP | Key-value storage | Eventual consistency |
| **Redis** | AP | In-memory caching | Eventual consistency |

---

## Key Takeaways 💡

1. **CAP theorem**: Distributed systems can only provide 2 of 3 properties (C, A, P)
2. **Partition tolerance is mandatory** in distributed systems - not optional
3. **Real choice is between C and A** when partition occurs
4. **CP systems prioritize consistency** - may reject requests during partitions
5. **AP systems prioritize availability** - may return stale data during partitions
6. **CA systems only work** in single-node or non-partitioned environments
7. **Choose CAP type based on** business requirements, not technical preferences
8. **Eventual consistency** allows AP systems to achieve consistency over time
9. **Split-brain scenarios** occur when partition handling is poor
10. **No perfect solution** - always trade-offs based on application needs

**Remember**: Understanding CAP theorem is essential for designing distributed systems. The key is to choose the right trade-offs based on your specific business requirements! 🎯