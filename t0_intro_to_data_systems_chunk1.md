# T1. Introduction to Data Systems — Exam Compression

**Course:** Data Management (2026–27)\
**Professor:** Eduardo Cabrera

## 1\. Why Data Management?

Modern applications and AI systems depend on data that can be stored, accessed, processed, and scaled efficiently.

### Four fundamental data challenges

- **Storage:** Where should data live? Locally, in the cloud, or across distributed systems?
- **Access:** How can data be retrieved efficiently? SQL, NoSQL, indexes, and query optimization.
- **Processing:** How should data be processed? Batch processing or streaming.
- **Scaling:** How can the system handle increasing data volume, traffic, and complexity? Cloud infrastructure, parallelism, and distributed systems.

**AI is only as good as the data that can be stored, accessed, processed, and scaled.**

### Modern data-management domains

- **Infrastructure and storage:** SQL databases, NoSQL databases, data lakes, vector databases.
- **Data engineering:** ETL/ELT pipelines, streaming (Kafka/Spark), feature stores.
- **Data preparation:** Cleaning, missing-value handling, feature scaling, and transforming text, images, and audio into machine-readable representations.
- **Data quality and MLOps:** Dataset versioning (DVC), labeling, monitoring, and detecting data drift.
- **Governance, privacy, and ethics:** Regulatory compliance (e.g., GDPR/HIPAA), bias reduction, and privacy-preserving methods such as federated learning.

## 2\. Learning Objectives

Be able to:

1. Define **data-intensive applications**.
2. Identify the **core components of a data system**.
3. Explain **reliability, scalability, and maintainability**.
4. Compare different **scaling approaches**.
5. Understand key **performance metrics**.

## 3\. Data-Intensive Applications

**Definition:** Software applications that prioritize storing, processing, and retrieving large amounts of data.

### Data-intensive vs. compute-intensive

- **Data-intensive:** Performance is often constrained by data volume, data complexity, data access, or the speed at which data changes.
- **Compute-intensive:** Performance is primarily constrained by CPU/GPU computation.

Data-intensive applications must handle:

- **Amount:** How much data exists?
- **Complexity:** How varied or structurally complex is the data?
- **Speed of change:** How quickly is new data generated or existing data updated?

**Key distinction:** CPU power is rarely the only or primary bottleneck in a data-intensive system; data storage, movement, access, and processing are major challenges.

## 4\. Common Building Blocks of Data-Intensive Applications

A data-intensive application combines specialized components that provide different functionalities.

| Component | Purpose | Example |
| --- | --- | --- |
| **Database** | Stores data for later access and processing | PostgreSQL |
| **Cache** | Stores results of expensive operations for faster reuse | Redis |
| **Search index** | Enables efficient keyword-based search | Inverted index |
| **Stream processing** | Processes messages or events asynchronously and continuously | Apache Kafka ecosystem |
| **Batch processing** | Processes large accumulated datasets, often in a distributed manner | Apache Spark |

**Important distinctions:**

- A **database** is the persistent storage component.
- A **cache** reduces repeated computation or expensive data retrieval.
- A **search index** accelerates retrieval based on search terms.
- **Stream processing** handles ongoing events with low processing delay.
- **Batch processing** handles accumulated data in groups.

Kafka is primarily an event-streaming platform; stream-processing computations may be performed by Kafka Streams, Spark Structured Streaming, or similar technologies.

## 5\. What Is a Data System?

**Definition:** A data system integrates specialized components and application logic to store, process, retrieve, and manage data efficiently.

Key principles:

- Modern applications generate and process large volumes of data.
- No single technology satisfies every data requirement optimally.
- Multiple components cooperate to form a complete solution.
- System design involves trade-offs among performance, reliability, scalability, and maintainability.

## 6\. Three Key Factors of a Data System

### 6.1 Reliability

**Definition:** The ability of a system to continue working correctly even when faults occur.

A reliable system should perform its intended functions correctly, including under adverse conditions.

### 6.2 Scalability

**Definition:** The ability of a system to handle growth in data volume, traffic, and system complexity.

Scalability concerns how well a system adapts when its workload increases.

### 6.3 Maintainability

**Definition:** The ease with which a system can be operated, understood, modified, repaired, and improved over time.

Maintainability depends on clear design, manageable complexity, and ease of diagnosing problems.

**Exam distinction:**

- Reliability = correct operation despite problems.
- Scalability = ability to handle growth.
- Maintainability = ease of changing and operating the system.

## 7\. Reliability: Faults, Errors, and Failures

Three concepts must be distinguished.

| Concept | Definition | Example |
| --- | --- | --- |
| **Fault** | A defect or abnormal component condition that can cause an error | A defective disk or software bug |
| **Error** | An incorrect internal state caused by a fault | Corrupted data in memory |
| **Failure** | The system's externally observable service deviates from its specification | A request returns incorrect data or the service becomes unavailable |

**Typical causal chain:**

`Fault → Error → Failure`

A fault may produce an error, and an error may lead to a failure. The chain is not inevitable: a system can detect, contain, or recover from errors before they become externally visible failures.

### Fault-tolerant systems

A **fault-tolerant system** is designed to continue providing correct service despite certain component faults or failures.

Common techniques include:

- **Redundancy:** Multiple components or copies provide alternatives when one fails.
- **Replication:** Maintain copies of data or services on different nodes.
- **Failover:** Switch to a backup component when the primary fails.
- **Recovery:** Restore a valid state after an error.

The objective is to prevent individual component problems from causing system-level failure.

### High availability vs. fault tolerance

- **High availability (HA):** Minimizes service interruptions and keeps a service accessible; brief downtime may still occur.
- **Fault tolerance (FT):** Aims to continue correct operation despite specified faults, ideally without service interruption.

**Exam trap:** High availability does not necessarily imply fault tolerance. Both improve service continuity, but fault tolerance places stronger emphasis on continuing operation despite faults.

## 8\. Scaling Approaches

The course introduction identifies scaling as a core data-system concern.

- **Vertical scaling (scale up):** Increase the resources of one machine, such as CPU, RAM, or storage.
- **Horizontal scaling (scale out):** Add more machines or nodes and distribute the workload.
- **Parallel processing:** Execute parts of a workload concurrently.
- **Distributed processing:** Coordinate work across multiple machines, often to improve capacity and throughput.

General trade-offs:

- Vertical scaling is simpler to manage initially but has hardware and capacity limits.
- Horizontal scaling can provide greater aggregate capacity but introduces network communication, coordination, replication, and consistency challenges.
- Parallelism can improve throughput but introduces coordination overhead and may be limited by sequential tasks.

## 9\. Batch vs. Streaming

| Dimension | Batch processing | Stream processing |
| --- | --- | --- |
| Input | Accumulated data | Continuous events |
| Execution | Processes data in groups | Processes events continuously or in short intervals |
| Typical goal | Large-scale throughput | Low-latency processing |
| Example | Periodic analysis of historical transactions | Detecting suspicious transactions as they arrive |

**Remember:** Batch is suited to accumulated datasets; streaming is suited to continuously arriving events.

## 10\. Quick Exam Revision

Memorize these definitions and distinctions:

1. **Data-intensive application:** An application whose challenges center on handling large, complex, or rapidly changing datasets.
2. **Data system:** An integrated collection of components that store, process, retrieve, and manage data.
3. **Database vs. cache:** Persistent data storage vs. temporary storage for faster reuse.
4. **Stream vs. batch:** Continuous event processing vs. processing accumulated data.
5. **Reliability:** Correct operation despite faults.
6. **Scalability:** Handling increased workload.
7. **Maintainability:** Ease of operating, repairing, and modifying a system.
8. **Fault vs. error vs. failure:** Defect or abnormal condition vs. incorrect internal state vs. externally observable deviation from specification.
9. **Fault tolerance vs. high availability:** Continued correct operation despite specified faults vs. minimizing service downtime.
10. **Vertical vs. horizontal scaling:** Increase resources on one machine vs. add more machines.

### Likely Exam Questions

- Define a data-intensive application and distinguish it from a compute-intensive application.
- Explain why no single technology can satisfy all data-management requirements.
- Describe the roles of databases, caches, search indexes, stream processing, and batch processing.
- Define reliability, scalability, and maintainability, giving one example of each.
- Explain the relationship between a fault, an error, and a failure.
- Compare fault-tolerant and high-availability systems.
- Compare vertical and horizontal scaling.
- Distinguish batch processing from stream processing.

**Core takeaway:** Good data-system design combines specialized components and balances reliability, scalability, and maintainability according to the application's workload and requirements.

Suggested GitHub filename: `t0_introduction_to_data_systems_chunk1.md`
