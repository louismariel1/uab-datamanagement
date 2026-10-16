# Reliability, Scalability & System Performance — Exam Compression (Chunk 2)

## 1\. Reliability

**Definition:** A reliable system continues to provide service correctly despite failures.

### Types of faults and prevention

| Fault type | Example | Mitigation |
| --- | --- | --- |
| Hardware fault | Disk failure | Redundancy and fault tolerance |
| Software fault | Bugs | Testing, monitoring, self-checking |
| Human fault | Operational mistakes | Isolated development and testing environments |

### Common reliability techniques

- **Redundancy:** Use backup components or machines to maintain availability when failures occur.
- **Chaos testing:** Intentionally introduce failures to discover weaknesses. Example: Netflix's Chaos Monkey randomly terminates production instances.
- **Testing and monitoring:** Automate tests and monitor system health to detect and fix bugs early.
- **Rollback capability:** Quickly revert to a stable version to minimize downtime.
- **Security:** Protect against unauthorized access and data breaches; integrate security from the beginning.

**Exam tip:** Reliability requires preventing, detecting, tolerating, and recovering from failures.

---

## 2\. Scalability

**Definition:** A system's ability to handle increasing workload while maintaining performance and reliability by adding resources.

### Load parameters

Load parameters measure the workload placed on a system.

| System/component | Example load parameter |
| --- | --- |
| Web server | Requests per second, network traffic |
| Database | Ratio of reads to writes |
| Web application | Number of concurrent users |
| In-memory database/cache | Cache hit rate |

**Key distinction:** Load parameters describe the workload, not the system's resources.

---

## 3\. System Performance

**Definition:** How efficiently a computer system or application completes tasks.

### Two fundamental performance questions

1. **Fixed resources:** If workload increases while resources remain unchanged, how does performance change?
2. **Fixed performance:** If workload increases, how many additional resources are needed to maintain the same performance?

### Performance metrics

| Metric | Definition | Example/use |
| --- | --- | --- |
| **Throughput** | Number of transactions or tasks completed per unit of time | Records processed per second |
| **Latency** | Delay before a system starts responding to a request | Time until the first response |
| **Response time** | Total time required to complete a request | Service time + latency |

**Formula to remember:**

Response time = Service time + Latency

### Which metric matters most?

- **Batch processing:** Usually throughput — how many records can be processed per second.
- **Online systems:** Usually response time — how long the user waits for a request to finish.

**Exam trap:** Throughput measures work completed over time; latency measures delay; response time measures the complete request duration.

---

## 4\. Coping with Loads: Static Scaling

**Static scaling:** Resources are adjusted manually by an administrator after analyzing system capacity. Changes are generally planned and relatively permanent.

### Scaling strategies

| Strategy | Also called | How it works | Example |
| --- | --- | --- | --- |
| **Vertical scaling** | Scale up/down | Increase or decrease the capacity of one machine | Add CPU or RAM |
| **Horizontal scaling** | Scale out/in | Add or remove machines from a pool | Add application servers |
| **Diagonal scaling** | Combination | Combine vertical and horizontal scaling | Upgrade servers, then add more servers |

### Vertical vs. horizontal scaling

- **Vertical:** Makes an individual machine more powerful.
- **Horizontal:** Increases or decreases the number of machines.
- **Diagonal:** Combines both approaches for greater flexibility.

**Diagonal scaling advantage:** Can handle traffic spikes and resource-intensive workloads more flexibly.

**Diagonal scaling disadvantage:** More complex to manage and may require additional tools and resources.

### Memory aid

- **Vertical = bigger machine**
- **Horizontal = more machines**
- **Diagonal = both**

---

## 5\. Coping with Loads: Dynamic Scaling

**Dynamic scaling:** Automatically adjusts resources in real time as workload changes.

### Elasticity

**Definition:** A system's ability to automatically provision or release resources in response to changing demand.

- **Scale up/out:** Add resources when workload increases.
- **Scale down/in:** Release resources when workload decreases.
- Closely associated with **cloud computing**.
- Particularly useful when workload is unpredictable.

### Static vs. dynamic scaling

| Feature | Static scaling | Dynamic scaling |
| --- | --- | --- |
| Decision | Manual | Automatic |
| Timing | Planned changes | Real-time adjustments |
| Workload adaptation | Less responsive | Adapts to changing demand |
| Best suited for | Predictable capacity needs | Variable or unpredictable workloads |

**Important distinction:** Scaling describes changing capacity; elasticity emphasizes automatic adaptation to workload changes.

---

## 6\. High-Yield Exam Questions

**Q1. What are the three types of faults?**

Hardware, software, and human faults.

**Q2. How can reliability be improved?**

Redundancy, chaos testing, automated testing, monitoring, rollback mechanisms, and security.

**Q3. Define scalability.**

The ability to handle increasing workload by adding resources while maintaining performance and reliability.

**Q4. Differentiate throughput, latency, and response time.**

- Throughput: completed tasks per unit of time.
- Latency: delay before the system starts responding.
- Response time: total time to complete a request.

**Q5. Which metric is usually most important for batch processing?**

Throughput.

**Q6. Which metric is usually most important for online systems?**

Response time.

**Q7. Differentiate vertical and horizontal scaling.**

Vertical scaling changes the capacity of one machine; horizontal scaling changes the number of machines.

**Q8. What is diagonal scaling?**

Combining vertical and horizontal scaling.

**Q9. What is elasticity?**

The automatic provisioning and releasing of resources in response to workload changes.

**Q10. Why is dynamic scaling useful in cloud computing?**

It automatically adjusts capacity to unpredictable demand, improving resource utilization while maintaining service performance.

---

## Final Revision Sheet

- **Reliability:** Correct service despite failures.
- **Faults:** Hardware, software, human.
- **Reliability techniques:** Redundancy, chaos testing, testing/monitoring, rollback, security.
- **Scalability:** Handle more load by adding resources.
- **Load parameter:** Measures workload.
- **Throughput:** Work completed per second.
- **Latency:** Delay before response begins.
- **Response time:** Service time + latency.
- **Batch processing:** Throughput.
- **Online systems:** Response time.
- **Static scaling:** Manual adjustment.
- **Vertical:** Bigger machine.
- **Horizontal:** More machines.
- **Diagonal:** Combination of vertical and horizontal.
- **Dynamic scaling:** Automatic, real-time resource adjustment.
- **Elasticity:** Automatically scale resources up or down with demand.
