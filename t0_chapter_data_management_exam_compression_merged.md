# Data Management — Complete Exam Compression

**Course:** Data Management (2026–2027)\
**Professor:** Eduardo Cabrera\
**Topics:** Introduction to Data Systems, Data Models, Reliability, Scalability, Performance, and Maintainability

---

# PART 1 — INTRODUCTION TO DATA SYSTEMS

## 1\. Why Data Management?

Modern applications and AI systems depend on data that can be stored, accessed, processed, and scaled efficiently.

### Four fundamental challenges

- **Storage:** Where and how should data be stored?
- **Access:** How can data be retrieved efficiently using databases, indexes, SQL, or NoSQL?
- **Processing:** How should data be processed: batch or streaming?
- **Scaling:** How can the system handle growing data volume, traffic, and complexity?

**Key principle:** AI is only as good as the data that can be stored, accessed, processed, and scaled.

### Modern data-management domains

- **Infrastructure and storage:** SQL/NoSQL databases, data lakes, vector databases.
- **Data engineering:** ETL/ELT pipelines, streaming, feature stores.
- **Data preparation:** Cleaning, handling missing values, scaling features, transforming data.
- **Data quality and MLOps:** Dataset versioning, labeling, monitoring, detecting data drift.
- **Governance and privacy:** Regulatory compliance, bias reduction, privacy-preserving methods.

## 2\. Data-Intensive Applications

**Definition:** Applications whose main challenges involve storing, processing, and retrieving large amounts of data.

### Data-intensive vs. compute-intensive

- **Data-intensive:** Limited by data volume, complexity, access, storage, movement, or rate of change.
- **Compute-intensive:** Limited mainly by CPU/GPU computation.

Three important dimensions:

1. **Amount:** How much data exists?
2. **Complexity:** How varied or structurally complex is the data?
3. **Speed of change:** How quickly is data generated or updated?

**Exam tip:** Data-intensive systems are not necessarily limited by CPU power; data access, storage, movement, and processing may be the real bottlenecks.

## 3\. Common Building Blocks

| Component | Purpose | Example |
| --- | --- | --- |
| **Database** | Persistent data storage | PostgreSQL |
| **Cache** | Temporarily stores frequently used results to speed up access | Redis |
| **Search index** | Enables fast keyword-based retrieval | Inverted index |
| **Stream processing** | Handles continuous events asynchronously | Kafka ecosystem, Spark Structured Streaming |
| **Batch processing** | Processes accumulated datasets in groups | Apache Spark |

Important distinctions:

- Database = persistent storage.
- Cache = faster reuse of data or results.
- Search index = efficient keyword search.
- Stream processing = continuous event processing.
- Batch processing = processing accumulated data in groups.

**Why combine components?** No single technology optimally satisfies every requirement. Specialized components cooperate to balance performance, reliability, scalability, and maintainability.

## 4\. Data-System Quality Attributes

A successful data system balances three essential qualities:

| Attribute | Definition | Main question |
| --- | --- | --- |
| **Reliability** | Continues operating correctly despite faults | Does it work correctly? |
| **Scalability** | Handles increasing workload and data volume | Can it handle growth? |
| **Maintainability** | Can be understood, operated, repaired, and modified efficiently | Can it be managed and changed easily? |

---

# PART 2 — DATA MODELS AND ABSTRACTION

## 5\. What Is a Data Model?

A **data model** is an abstract representation that organizes data and describes how information is represented and related.

Data models help with:

- Software design and development.
- Organizing and representing information.
- Communicating between business and technical teams.
- Clarifying requirements.
- Influencing performance and system architecture.

### Layered abstraction

- Each layer provides a simplified model to the layer above.
- It hides unnecessary implementation details.
- Different teams can work on different layers independently.
- Each layer describes how information is represented by the next lower layer.

**Key principle:** Abstraction reduces complexity by hiding implementation details.

## 6\. Three Levels of Data Modeling

Database design typically progresses through three levels.

### A. Conceptual Model

**Purpose:** Describe _what_ information the system needs.

- High-level representation of business concepts and relationships.
- Used by stakeholders, business analysts, and data architects.
- Defines requirements and scope.
- Avoids implementation-specific details.

Main elements:

- **Entity:** A real-world object or concept, such as Customer, Student, or Car.
- **Relationship:** An association between entities, such as Customer places Order.

### B. Logical Model

**Purpose:** Describe _how data is structured_, independently of a specific DBMS.

- Defines entities, attributes, and relationships.
- Specifies data structure in greater detail.
- Supports business requirements and portability.
- Helps determine the database schema.

Example:

`Customer(CustomerID, Name, Email)`

`CustomerID` can be the primary key.

### C. Physical Model

**Purpose:** Describe _how data is actually implemented and stored_ in a specific database system.

Includes:

- Tables, columns, and indexes.
- Database-specific data types.
- Storage structures.
- Constraints and implementation details.
- Performance-related choices.

Example: Define `Name` as `VARCHAR(100)` and create an index on `CustomerID`.

### Comparison

| Feature | Conceptual | Logical | Physical |
| --- | --- | --- | --- |
| Main question | What data is needed? | How is data structured? | How is it implemented? |
| Detail | High-level | Detailed structure | Implementation-specific |
| Attributes | Usually not detailed | Explicitly defined | Defined with data types |
| DBMS-specific | No | Generally no | Yes |
| Audience | Business stakeholders | Analysts and architects | Developers/DBAs |

**Memory aid: Conceptual = WHAT; Logical = STRUCTURE; Physical = IMPLEMENTATION.**

## 7\. Entities, Attributes, and Keys

- **Entity:** Real-world object or concept represented in the data.
- **Attribute:** Property describing an entity.
- **Relationship:** Association between entities.
- **Primary key (PK):** Uniquely identifies each row.
- **Foreign key (FK):** References a key in another table or a candidate key in the same table.

Example:

`Customer(CustomerID PK, Name)`

`Orders(OrderID PK, CustomerID FK)`

Here, `CustomerID` in `Orders` references the customer who placed the order.

## 8\. Relationship Cardinalities

### A. One-to-One (1:1)

One instance of A is associated with at most one instance of B, and vice versa.

**Example:** One person has one passport, assuming the business rules enforce this restriction.

### B. One-to-Many (1:M)

One instance of A can be associated with many instances of B, while each B belongs to at most one A.

**Example:** One customer can place many orders; each order belongs to one customer.

Relational implementation: Put the foreign key on the **many side**.

```
Customer(CustomerID PK, Name)
Orders(OrderID PK, CustomerID FK)
```

### C. Many-to-Many (M:N)

Many instances of A can relate to many instances of B.

**Example:** One student can enroll in many courses, and each course can have many students.

Relational implementation: Use a **junction table** containing foreign keys to both entities.

```
Student(StudentID PK, Name)
Course(CourseID PK, Title)

Enrollment(
    StudentID PK, FK,
    CourseID PK, FK
)
```

The composite primary key `(StudentID, CourseID)` prevents duplicate enrollment pairs in this simplified design.

For orders and products, an `OrderItem` junction table can also store `Quantity`.

### Essential cardinality rules

- **1:1:** One-to-one association.
- **1:M:** FK normally goes on the many side.
- **M:N:** Use a junction table in a relational database.

---

# PART 3 — RELIABILITY

## 9\. Reliability

**Definition:** The ability of a system to continue providing correct service despite faults.

A reliable system should perform its intended functions correctly, including under adverse conditions.

## 10\. Faults, Errors, and Failures

These three concepts must be distinguished.

| Concept | Definition | Example |
| --- | --- | --- |
| **Fault** | Defect or abnormal component condition that may cause an error | Defective disk or software bug |
| **Error** | Incorrect internal state caused by a fault | Corrupted data in memory |
| **Failure** | Externally observable service deviation from its specification | Incorrect response or unavailable service |

**Typical causal chain:**

`Fault → Error → Failure`

A fault may cause an error, which may lead to a failure. However, detection, containment, and recovery can prevent the error from becoming an externally visible failure.

## 11\. Types of Faults and Mitigation

| Fault type | Example | Mitigation |
| --- | --- | --- |
| **Hardware fault** | Disk or server failure | Redundancy, RAID, replication, failover |
| **Software fault** | Program bug or incorrect data format | Testing, monitoring, self-checking, rollback |
| **Human fault** | Operator accidentally deletes data | Access controls, confirmation steps, isolated environments, backups |

### Example exam problem

A company experiences:

1. A hard drive stops working.
2. A code update saves data in the wrong format.
3. An operator accidentally runs a destructive production script.

**Answers:**

1. Hard drive failure → **Hardware fault**; use redundancy or replication.
2. Bug in code → **Software fault**; use testing, monitoring, and rollback.
3. Incorrect operator action → **Human fault**; use least-privilege access, safeguards, and backups.

## 12\. Reliability Techniques

- **Redundancy:** Use backup components or machines to preserve service.
- **Replication:** Maintain copies of data or services on different nodes.
- **Failover:** Switch to a backup when the primary component fails.
- **Recovery:** Restore a valid state after an error.
- **Testing and monitoring:** Detect bugs and abnormal behavior early.
- **Rollback:** Revert to a stable version after a faulty deployment.
- **Chaos testing:** Intentionally introduce failures to discover weaknesses.
- **Security:** Protect systems and data from unauthorized access and breaches.

**Exam principle:** Reliability involves preventing, detecting, tolerating, and recovering from failures.

## 13\. High Availability vs. Fault Tolerance

- **High availability (HA):** Minimizes service interruptions and keeps the service accessible; brief downtime may still occur.
- **Fault tolerance (FT):** Aims to continue correct operation despite specified faults, ideally without interruption.

**Exam trap:** High availability does not necessarily imply fault tolerance. Both improve continuity, but fault tolerance emphasizes uninterrupted correct operation despite faults.

---

# PART 4 — SCALABILITY AND LOAD MANAGEMENT

## 14\. Scalability

**Definition:** The ability of a system to handle increasing workload while maintaining acceptable performance and reliability by adding resources.

Workload may increase through more users, requests, data, or processing requirements.

### Load parameters

Load parameters describe the workload placed on a system, not the resources available.

| Component | Example load parameter |
| --- | --- |
| Web server | Requests per second, network traffic |
| Database | Read/write ratio |
| Web application | Concurrent users |
| Cache | Cache hit rate |

## 15\. System Performance Metrics

| Metric | Definition | Example |
| --- | --- | --- |
| **Throughput** | Number of tasks or requests completed per unit of time | Transactions/second |
| **Latency** | Delay associated with a request, often the time until response begins | Initial response delay |
| **Response time** | Total time from request to completed response | Total user waiting time |

**Formula:**

`Response time = Latency + Service time`

### Which metric matters most?

- **Batch processing:** Usually throughput — how much data can be processed per second.
- **Online systems:** Usually response time — how long the user waits for a request to finish.

**Exam trap:**

- Throughput = work completed over time.
- Latency = delay.
- Response time = total request duration.

### Two fundamental performance questions

1. **Fixed resources:** If workload increases but resources stay constant, how does performance change?
2. **Fixed performance:** If workload increases, how many additional resources are required to maintain performance?

## 16\. Static Scaling

**Definition:** Capacity is adjusted manually by an administrator, usually through planned changes that remain in place until another adjustment.

### Scaling strategies

| Strategy | Also called | How it works | Example |
| --- | --- | --- | --- |
| **Vertical scaling** | Scale up/down | Change the capacity of one machine | Add CPU or RAM |
| **Horizontal scaling** | Scale out/in | Add or remove machines | Add application servers |
| **Diagonal scaling** | Combination | Combine vertical and horizontal scaling | Upgrade servers, then add more |

### Vertical scaling

- Makes one machine more powerful.
- Usually simpler to manage initially.
- Limited by hardware capacity.
- A single-machine failure may remain a major risk without redundancy.

### Horizontal scaling

- Adds more machines or nodes.
- Distributes workload across multiple machines.
- Can provide greater aggregate capacity and flexibility.
- Introduces coordination, networking, replication, and consistency challenges.

### Diagonal scaling

- Combines vertical and horizontal scaling.
- Can provide flexible capacity growth.
- May handle resource-intensive workloads more effectively.
- Is more complex to manage.

**Memory aid:**

- Vertical = bigger machine.
- Horizontal = more machines.
- Diagonal = both.

## 17\. Dynamic Scaling and Elasticity

**Dynamic scaling:** Resources are automatically adjusted in real time as workload changes.

**Elasticity:** The ability to automatically provision or release resources in response to changing demand.

- **Scale up/out:** Add capacity when demand increases.
- **Scale down/in:** Release capacity when demand decreases.
- Closely associated with cloud computing.
- Particularly useful for unpredictable or highly variable workloads.

### Static vs. dynamic scaling

| Feature | Static scaling | Dynamic scaling |
| --- | --- | --- |
| Decision | Manual | Automatic |
| Timing | Planned changes | Real-time adjustment |
| Adaptation | Less responsive | Adapts to changing demand |
| Best suited for | Predictable workloads | Variable workloads |

**Important distinction:** Scaling describes changing capacity; elasticity emphasizes automatic adaptation to changing workload.

## 18\. Parallel and Distributed Processing

- **Parallel processing:** Executes parts of a workload concurrently.
- **Distributed processing:** Coordinates work across multiple machines.

Benefits may include greater capacity and throughput.

Trade-offs include coordination overhead, network communication, and limits imposed by tasks that must run sequentially.

## 19\. Batch vs. Stream Processing

| Dimension | Batch processing | Stream processing |
| --- | --- | --- |
| Input | Accumulated data | Continuous events |
| Execution | Processes groups of data | Processes events continuously or in short intervals |
| Main goal | High throughput | Low-latency processing |
| Example | Historical transaction analysis | Fraud detection as transactions arrive |

**Memory aid: Batch = accumulated data; Stream = ongoing events.**

---

# PART 5 — MAINTAINABILITY

## 20\. Maintainability

**Definition:** The ability to modify, correct, update, operate, and improve a system after its initial deployment.

A maintainable system should be understandable, easy to diagnose, and adaptable to future requirements.

### Three design principles

1. **Operability:** Enable efficient and effective operation in the production environment.
2. **Simplicity:** Reduce unnecessary complexity so engineers can understand the system.
3. **Evolvability:** Make future changes easier as requirements and use cases change.

**Memory aid: Maintainability = Operability + Simplicity + Evolvability.**

---

# PART 6 — INTEGRATED EXAM PROBLEMS

## Problem 1 — Fault Classification (1 point)

A company stores millions of customer records. During an update:

- A hard drive fails.
- A software bug saves some data in the wrong format.
- An operator accidentally runs a script that deletes data.

**Questions:** Classify each fault and suggest a reliability technique.

**Model answer:**

- Hard drive failure → **Hardware fault**; use redundancy, RAID, replication, or failover.
- Incorrect data format → **Software fault**; use automated testing, monitoring, and rollback.
- Accidental deletion → **Human fault**; use access controls, safeguards, backups, and recovery procedures.

## Problem 2 — Growing User Traffic (1 point)

A music streaming application grows from 10,000 to 100,000 concurrent users and becomes slower.

**Q1. Which scaling approach is appropriate?**

**Answer:** Horizontal scaling — add application servers and distribute traffic through load balancing. Vertical scaling can help initially but is limited by the capacity of one machine.

**Q2. Traffic varies considerably between peak and off-peak hours. What additional concept is useful?**

**Answer:** Elasticity — automatically add resources during peaks and release them when demand decreases.

## Problem 3 — E-Commerce Architecture (2 points)

An e-commerce system must:

- Process thousands of orders per hour.
- Store customer and product information.
- Support fast keyword-based product searches.
- Continue operating if a server fails.

### Q1. Recommend the building blocks.

- **Database:** Persistent customer, product, and order storage.
- **Search index:** Fast keyword-based product searches.
- **Stream processing:** Asynchronous handling of order events.
- **Cache:** Faster access to frequently requested data.
- **Batch processing:** Large-scale analysis of historical orders.

### Q2. How can database availability be improved?

Use **replication**, such as a database cluster with suitable failover mechanisms, so another replica can serve requests if the primary fails.

### Q3. How should the system scale from 10,000 to 200,000 orders per hour?

**Answer:** Horizontal scaling is a strong default recommendation because it distributes workload across multiple machines.

- Elasticity is useful if demand fluctuates significantly.
- Vertical scaling may help initially but has hardware limits.
- Diagonal scaling combines vertical and horizontal approaches but adds complexity.

## Problem 4 — Social Network Performance (2 points)

A social network experiences:

1. Response time increasing from 0.5 seconds to 5 seconds as concurrent users increase.
2. Search by name becoming slower with 50 million database records.
3. Increasing network latency despite stable CPU usage.

### Q1. Identify the affected metrics.

- Slow responses → **Response time** increases.
- Slow database search → **Query latency** increases and query throughput may decrease.
- Increasing network delay → **Latency** increases.

### Q2. Is the problem mainly compute capacity or data management?

The symptoms suggest a **data-management or networking bottleneck**, rather than a simple CPU bottleneck, because CPU usage remains stable. Further measurements are needed to confirm the exact cause.

### Q3. Recommend solutions.

- **Increasing concurrent users:** Horizontal scaling of application servers and load balancing.
- **Slow search over 50 million records:** Add or optimize a dedicated search index; optimize database queries and consider database scaling.
- **Variable network latency:** Investigate network congestion and data movement first. Use elasticity if resource demand varies with traffic, but note that elasticity alone does not necessarily fix a network bottleneck.

---

# PART 7 — HIGH-YIELD QUICK REVISION

## Definitions to memorize

1. **Data-intensive application:** An application centered on handling large, complex, or rapidly changing datasets.
2. **Data system:** Components and application logic integrated to store, process, retrieve, and manage data.
3. **Conceptual model:** Defines what information is needed.
4. **Logical model:** Defines data structure and attributes independently of a particular DBMS.
5. **Physical model:** Specifies implementation details for a particular database system.
6. **Entity:** A real-world object or concept.
7. **Attribute:** A property of an entity.
8. **Primary key:** Uniquely identifies a row.
9. **Foreign key:** References a key in another table or a candidate key in the same table.
10. **Reliability:** Correct operation despite faults.
11. **Scalability:** Ability to handle increased workload.
12. **Maintainability:** Ease of operating, repairing, understanding, and modifying a system.
13. **Fault:** A defect or abnormal condition that can cause an error.
14. **Error:** An incorrect internal state.
15. **Failure:** An externally observable deviation from the service specification.
16. **High availability:** Minimizes service interruptions.
17. **Fault tolerance:** Continues correct operation despite specified faults.
18. **Throughput:** Completed tasks per unit of time.
19. **Latency:** Request delay.
20. **Response time:** Total time to complete a request.
21. **Vertical scaling:** Increase resources on one machine.
22. **Horizontal scaling:** Add more machines.
23. **Diagonal scaling:** Combine vertical and horizontal scaling.
24. **Elasticity:** Automatically adjust resources to changing demand.
25. **Batch processing:** Process accumulated data in groups.
26. **Stream processing:** Process continuously arriving events.

## Essential formulas and rules

- **Response time = Latency + Service time**
- **1:M relationship:** Foreign key normally belongs on the many side.
- **M:N relationship:** Use a junction table in a relational database.
- **Fault → Error → Failure:** A typical causal chain, not an inevitable sequence.
- **Batch processing:** Usually prioritizes throughput.
- **Online applications:** Usually prioritize response time.
- **Variable workload:** Elasticity is especially useful.
- **Large, growing workload:** Horizontal scaling often offers more flexible capacity.

## Likely Exam Questions

1. Define a data-intensive application and distinguish it from a compute-intensive application.
2. Explain why modern data systems combine specialized components.
3. Compare databases, caches, search indexes, stream processing, and batch processing.
4. Explain the differences between conceptual, logical, and physical data models.
5. Distinguish entities, attributes, relationships, primary keys, and foreign keys.
6. Explain how 1:1, 1:M, and M:N relationships are implemented.
7. Define reliability, scalability, and maintainability.
8. Explain the relationship between faults, errors, and failures.
9. Compare high availability and fault tolerance.
10. Identify hardware, software, and human faults and suggest mitigations.
11. Differentiate throughput, latency, and response time.
12. Compare vertical, horizontal, and diagonal scaling.
13. Explain dynamic scaling and elasticity.
14. Compare batch and stream processing.
15. Explain operability, simplicity, and evolvability.
16. Design a data architecture for a growing e-commerce or social network application.

---

# FINAL MEMORY SHEET

- **Data models:** Conceptual = what; Logical = structure; Physical = implementation.
- **Relationships:** 1:1 = one-to-one; 1:M = FK on many side; M:N = junction table.
- **System qualities:** Reliability = correct operation; Scalability = growth; Maintainability = ease of change and operation.
- **Faults:** Hardware, software, human.
- **Causal chain:** Fault → Error → Failure.
- **Reliability tools:** Redundancy, replication, failover, recovery, testing, monitoring, rollback, security.
- **Performance:** Throughput = work/time; Latency = delay; Response time = total duration.
- **Scaling:** Vertical = bigger machine; Horizontal = more machines; Diagonal = both.
- **Elasticity:** Automatically add and release resources with demand.
- **Architecture:** Database for persistence, cache for speed, search index for keyword retrieval, streams for continuous events, batches for accumulated data.
- **Maintainability:** Operability + Simplicity + Evolvability.

**Core takeaway:** Good data-system design combines appropriate data models and specialized components while balancing reliability, scalability, performance, and maintainability according to workload and requirements.
