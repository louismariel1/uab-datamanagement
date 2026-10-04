# Exam-Style Questions and Model Answers

 ## Part 1 — Core Concepts

 ### Question 1 — Data-Intensive Applications

 **Question:**\
 What is a data-intensive application? Explain the main characteristics that make an application data-intensive.

 **Model Answer:**\
 A data-intensive application is an application whose main challenges are related to the **amount, complexity, and rate of change of data** rather than purely computational complexity.

 The main characteristics include:

 - **Large data volume:** The system may store millions or billions of records.
- **Data complexity:** Data may come in different formats and may require different processing techniques.
- **High rate of change:** Data may be continuously created, updated, or deleted.
- **Large workload:** Many users or processes may access the data simultaneously.

 Because of these challenges, data-intensive systems usually use **multiple specialized components** rather than relying on a single tool.

---

 ### Question 2 — Building Blocks

 **Question:**\
 Explain why data-intensive applications commonly use multiple specialized building blocks instead of one system for everything.

 **Model Answer:**\
 Different tasks have different requirements. A single system is usually not optimized for all of them.

 For example:

 - A **database** provides persistent storage.
- A **cache** provides fast access to frequently used data.
- A **search index** provides efficient searching.
- **Stream processing** handles continuously arriving events.
- **Batch processing** handles large volumes of historical data.

 Combining specialized components allows the overall architecture to provide better **performance, scalability, reliability, and maintainability**.

---

 ## Part 2 — Performance

 ### Question 3 — Performance Metrics

 **Question:**\
 Explain the difference between **throughput, latency, and response time**.

 **Model Answer:**

 - **Throughput** is the amount of work a system can complete during a given period, such as requests per second.
- **Latency** is the time required for an operation or communication to complete.
- **Response time** is the total time experienced by the user when waiting for a request to complete.

 For example, a system might process **10,000 requests per second** (throughput), while each individual request takes **200 ms** (latency/response time).

---

 ### Question 4 — Performance Scenario

 **Scenario:**\
 An online shopping application normally responds to requests in 0.5 seconds. After the number of concurrent users increases significantly, the response time increases to 5 seconds.

 **Questions:**

 1. Which performance metric has deteriorated?
2. What could be causing the problem?
3. What scaling approach could help?

 **Model Answer:**

 1. **Response time/latency** has deteriorated.
2. The increased number of concurrent users is creating a higher workload than the current infrastructure can efficiently handle.
3. **Horizontal scalability** could help by distributing requests across multiple servers. A load balancer could distribute incoming requests among these servers.

---

 # Part 3 — Reliability and Faults

 ### Question 5 — Reliability

 **Question:**\
 Define reliability in the context of data-intensive systems. Does reliability mean that a system will never experience a fault?

 **Model Answer:**\
 Reliability is the ability of a system to **continue operating correctly despite faults**.

 Reliability does **not** mean that faults never occur. Hardware, software, and human faults can still happen. A reliable system is designed to tolerate or recover from those faults.

 For example, if one database server fails but a replica takes over without users losing access, the system has demonstrated reliability.

---

 ### Question 6 — Fault Classification

 **Scenario:**\
 A company experiences the following problems:

 1. A hard disk physically fails.
2. A programmer introduces a bug during a software update.
3. An administrator accidentally deletes customer records.

 **Question:**\
 Classify each fault and suggest one technique to mitigate it.

 **Model Answer:**

 | Problem | Fault type | Possible mitigation |
| --- | --- | --- |
| Hard disk failure | Hardware fault | RAID, replication, redundant hardware |
| Programming bug | Software fault | Testing, monitoring, rollback |
| Accidental deletion | Human fault | Access controls, backups, permissions, approval mechanisms |

---

 ### Question 7 — Reliability Scenario

 **Scenario:**\
 An application stores important customer data on one database server. The company wants the application to remain available if that server fails.

 **Question:**\
 What reliability technique would you recommend? Explain why.

 **Model Answer:**\
 I would recommend **database replication**.

 Multiple copies of the database can be maintained on different servers. If theing system normally receives 1,000 requests per minute. When a popular concert goes primary database server fails, another replica can take over or continue serving requests.

 This reduces the **single point of failure** and improves availability.

---

 ## Part 4 — Scalability

 ### Question 8 — Vertical vs. Horizontal Scaling

 **Question:**\
 Explain the difference between vertical and horizontal scalability. Give an example of each.

 **Model Answer:**

 **Vertical scalability** means increasing the capacity of an existing machine.

 Example:

 > Increasing a server from 8 GB RAM to 64 GB RAM and upgrading its CPU.

 **Horizontal scalability** means adding more machines and distributing the workload among them.

 Example:

 > Increasing an application from 2 web servers to 10 web servers behind a load balancer.

---

 ### Question 9 — Scaling Scenario

 **Scenario:**\
 A music-streaming service grows from 10,000 concurrent users to 100,000 concurrent users. The existing server is becoming overloaded.

 **Questions:**

 1. Would vertical or horizontal scaling be more appropriate?
2. Why?
3. What additional technique could be useful if traffic changes significantly throughout the day?

 **Model Answer:**

 1. **Horizontal scaling** would generally be more appropriate.
2. Multiple servers can share the workload, allowing the application to handle many more concurrent users.
3. **Elasticity** would be useful if demand changes throughout the day. The system could add resources during peak periods and remove them during low-demand periods.

---

 ## Part 5 — Elasticity

 ### Question 10 — Elasticity

 **Question:**\
 What is elasticity, and how is it different from scalability?

 **Model Answer:**\
 **Scalability** is the ability of a system to handle increasing workload.

 **Elasticity** is the ability to **dynamically adjust resources according to the current workload**.

 For example, a system might normally use five servers but automatically increase to twenty servers during a traffic spike and return to five afterward.

 Elasticity is particularly useful in environments where workload is **highly unpredictable or variable**.

---

 ### Question 11 — Cloud Scenario

 **Scenario:**\
 An online ticketing system normally receives 1,000 requests per minute. When a popular concert goes on sale, traffic suddenly increases to 100,000 requests per minute. Several hours later, traffic returns to normal.

 **Question:**\
 What approach would you recommend and why?

 **Model Answer:**\
 I would recommend **elasticity combined with horizontal scaling**.

 During the ticket release, additional servers can be dynamically added to handle the huge workload. When demand decreases, those resources can be removed.

 This avoids permanently paying for a large infrastructure that is only needed during short periods of high demand.

---

 # Part 6 — Maintainability

 ### Question 12 — Maintainability

 **Question:**\
 Define maintainability and explain the three principles associated with it.

 **Model Answer:**\
 Maintainability is the ability to **modify, correct, update, and improve a system after its initial deployment**.

 The three principles are:

 1. **Operability:** The system should be easy to operate efficiently and effectively in production.
2. **Simplicity:** The system should avoid unnecessary complexity so that engineers can understand it easily.
3. **Evolvability:** Engineers should be able to modify the system as requirements and use cases change.

---

 ### Question 13 — Maintainability Scenario

 **Scenario:**\
 A company has a large application that was developed ten years ago. Only two engineers understand its architecture. Every small change requires modifying several interconnected components, and 10,000 orders per hour. Two years later, it needs to process 200,000 orders per hour. At the same time, user records. Searching for a user by name has become very slow. However, CPU usage remains relatively very large number of records, and searching them efficiently is becoming difficult. The fact that CPU usage remains-based searches. Database indexing, partitioning, or horizontal scaling could also help depending traffic across servers, optimizing data transfer, caching frequently requested information, and dynamically allocating additional continue operating correctly despite faults. Techniques such as replication, redundancy, testing, monitoring, and backups help operated, understood, modified, and improved after deployment. three. A system may be highly scalable but difficult to maintain, or highly maintainable but unable to handle increased traffic it practical to dynamically provision and release computing new developers need months to understand the system.

 **Question:**\
 Which maintainability principles are being violated? Explain.

 **Model Answer:**\
 The main problem is **simplicity** and **evolvability**.

 - The system has excessive complexity, violating the principle of **simplicity**.
- Making changes is difficult, indicating poor **evolvability**.
- If operating and maintaining the system in production is also difficult, **operability** is affected.

 The company should simplify the architecture, document important components, reduce unnecessary dependencies, and make components easier to modify independently.

---

 # Part 7 — Architecture Scenarios

 ## Question 14 — E-Commerce Architecture

 **Scenario:**\
 You are designing an e-commerce system that must:

 - Process thousands of orders every hour.
- Store customer and product information.
- Provide fast product searches.
- Handle frequently accessed product information quickly.
- Process some events continuously.
- Analyze historical order data.

 **Question:**\
 Which building blocks would you use? Explain the purpose of each.

 **Model Answer:**

 - **Database:** Store customer, product, and order information.
- **Search index:** Provide fast product searches.
- **Cache:** Store frequently accessed information for faster responses.
- **Stream processing:** Process continuously arriving events, such as orders or user activity.
- **Batch processing:** Analyze large volumes of historical orders.

 These components are specialized for different workloads and together form a more effective data-intensive architecture.

---

 ## Question 15 — Complete E-Commerce Scenario

 **Scenario:**\
 An e-commerce company initially processes 10,000 orders per hour. Two years later, it needs to process 200,000 orders per hour. At the same time, customers complain that product searches have become slow.

 **Questions:**

 1. What scalability strategy would you recommend for processing the increased number of orders?
2. What could improve product-search performance?
3. If traffic varies significantly between day and night, what additional concept should be used?

 **Model Answer:**

 1. **Horizontal scalability** should be used to distribute the increased workload across multiple servers.
2. A **dedicated search index** should be introduced or optimized for product searches.
3. **Elasticity** should be used so resources can dynamically increase during peak periods and decrease during low-demand periods.

---

 # Part 8 — Data Management vs. Compute

 ## Question 16

 **Scenario:**\
 A social network contains 50 million user records. Searching for a user by name has become very slow. However, CPU usage remains relatively low.

 **Question:**\
 Is the main problem likely to be a compute limitation or a data-management problem? Explain.

 **Model Answer:**\
 The problem is primarily a **data-management problem**.

 The database contains a very large number of records, and searching them efficiently is becoming difficult. The fact that CPU usage remains low suggests that CPU computation is not the main bottleneck.

 A **search index** could provide much faster name-based searches. Database indexing, partitioning, or horizontal scaling could also help depending on the architecture.

---

 ## Question 17 — Network Bottleneck

 **Scenario:**\
 A social network experiences increasing traffic. CPU utilization remains stable, but network latency increases significantly.

 **Question:**\
 What is the likely bottleneck?

 **Model Answer:**\
 The likely bottleneck is **network communication/data transmission**, rather than CPU computation.

 Possible solutions include improving network capacity, distributing traffic across servers, optimizing data transfer, caching frequently requested information, and dynamically allocating additional resources when traffic increases.

---

 # Part 9 — Integrated Exam Questions

 ## Question 18 — Full System Design

 **Scenario:**

 You are designing a large social-media platform. Users can:

 - Post text, images, and videos.
- Search for other users.
- View popular posts.
- Send messages.
- Upload content continuously.

 The system must remain available even when individual servers fail and must support rapid growth in users.

 ### Questions

 **a.** Identify at least four appropriate building blocks.

 **b.** Explain how you would provide reliability.

 **c.** Explain how you would handle user growth.

 **d.** Explain how you would handle unpredictable traffic.

 **e.** Explain how maintainability should be addressed.

 ### Model Answer

 **a. Building blocks:**

 - **Database** — persistent storage of user and application data.
- **Search index** — fast user and content searches.
- **Cache** — fast access to frequently requested content.
- **Stream processing** — processing continuous events such as uploads and user activity.
- **Batch processing** — analyzing historical data.

 **b. Reliability:**

 Use **replication and redundancy**. Important data should have multiple copies, and critical services should have redundant instances.

 **c. User growth:**

 Use **horizontal scalability**. Add additional servers and distribute requests across them.

 **d. Unpredictable traffic:**

 Use **elasticity** so resources can automatically increase and decrease based on workload.

 **e. Maintainability:**

 The architecture should emphasize:

 - **Operability** — easy to run and monitor.
- **Simplicity** — avoid unnecessary complexity.
- **Evolvability** — make future modifications easier.

---

 # Part 10 — Challenging Scenario Questions

 ## Question 19 — Choosing the Correct Scaling Strategy

 **Scenario:**

 A company has a database server with 16 CPU cores and 64 GB RAM. The database is becoming overloaded.

 The company is considering four options:

 - Upgrade to 64 CPU cores and 256 GB RAM.
- Add four additional database servers.
- Upgrade the existing server and add additional servers.
- Automatically add/remove servers depending on demand.

 ### Question

 Identify which option represents:

 1. Vertical scaling
2. Horizontal scaling
3. Diagonal scaling
4. Elasticity

 ### Model Answer

 1. **Vertical scaling:** Upgrade to 64 CPU cores and 256 GB RAM.
2. **Horizontal scaling:** Add four additional database servers.
3. **Diagonal scaling:** Upgrade the existing server **and** add additional servers.
4. **Elasticity:** Automatically add/remove servers according to workload.

---

 ## Question 20 — Reliability Under Multiple Faults

 **Scenario:**

 An online banking system has the following failures:

 - A storage disk fails.
- A software update introduces incorrect transaction calculations.
- An administrator accidentally executes a destructive command.
- The primary database server becomes unavailable.

 ### Question

 For each problem, identify the fault and recommend a mitigation technique.

 ### Model Answer

 | Failure | Fault | Mitigation |
| --- | --- | --- |
| Disk failure | Hardware fault | RAID/redundancy |
| Incorrect software update | Software fault | Testing + monitoring + rollback |
| Destructive command | Human fault | Access controls + approval \+ backups |
| Database unavailable | Hardware/infrastructure fault | Replication/failover |

The general principle is to design the system so that **individual faults do not necessarily cause complete system failure**.

---

 # Part 11 — Long-Answer Exam Questions

 ## Question 21

 **Question:**\
 "Reliability, scalability, and maintainability are different properties, but they are all important in modern data-intensive systems."

 Discuss this statement.

 ### Model Answer

 These three properties address different aspects of system quality.

 **Reliability** concerns the ability of a system to continue operating correctly despite faults. Techniques such as replication, redundancy, testing, monitoring, and backups help improve reliability.

 **Scalability** concerns the ability to handle increasing workloads. Vertical scaling increases the capacity of existing machines, while horizontal scaling adds additional machines.

 **Maintainability** concerns how easily a system can be operated, understood, modified, and improved after deployment. It includes **operability, simplicity, and evolvability**.

 A successful data-intensive system must balance all three. A system may be highly scalable but difficult to maintain, or highly maintainable but unable to handle increased traffic. Good architecture considers these properties together.

---

 ## Question 22

 **Question:**\
 Explain why elasticity is particularly important in cloud computing environments.

 ### Model Answer

 Cloud environments make it practical to dynamically provision and release computing resources.

 If workload is unpredictable, permanently provisioning enough resources for the maximum workload would be expensive and inefficient.

 With elasticity:

 - Resources can be added during high demand.
- Resources can be removed during low demand.
- The system can maintain performance during traffic peaks.
- Resource utilization can be improved.
- Costs can be reduced because unnecessary resources do not need to run continuously.

 Therefore, elasticity is particularly valuable for workloads that are **variable or unpredictable**.

---

 # Part 12 — Comprehensive Case Study

 ## Question 23 — 10-Mark Scenario

 **Scenario:**

 A company operates a global video-streaming platform.

 Initially, the platform serves 50,000 concurrent users. After becoming popular, it reaches 2 million concurrent users.

 The company experiences the following problems:

 - Video requests become slower during peak hours.
- A database server failure temporarily makes user information unavailable.
- Searching for videos becomes slow as the catalog grows.
- The system uses many servers at night even though traffic is very low.
- Developers find it increasingly difficult to modify the application.

 ### Questions

 **1.** What scalability strategy should be used to handle the growth from 50,000 to 2 million users?

 **2.** What reliability technique should address the database-server failure?

 **3.** What could improve video-search performance?

 **4.** What concept could reduce unnecessary infrastructure usage at night?

 **5.** Which maintainability principles should the developers focus on?

 ### Model Answer

 **1\. Scalability:**\
 Use **horizontal scalability**. Multiple servers can distribute the workload and support millions of concurrent users.

 **2\. Reliability:**\
 Use **database replication and failover**. If one database server fails, another replica can continue serving requests.

 **3\. Search performance:**\
 Use a **dedicated search index** optimized for video searches. This avoids inefficiently searching the entire database.

 **4\. Resource efficiency:**\
 Use **elasticity**. The system can add resources during peak hours and remove them when demand decreases.

 **5\. Maintainability:**\
 Focus on:

 - **Operability** — make production operation easier.
- **Simplicity** — reduce unnecessary architectural complexity.
- **Evolvability** — make future changes easier.

---

 # Part 13 — Very Short Exam Questions

 These are useful for rapid revision.

 ### Question 24

 What is throughput?

 **Answer:** The amount of work a system completes per unit of time.

 ### Question 25

 What is latency?

 **Answer:** The time required for an operation or request to complete.

 ### Question 26

 What is horizontal scaling?

 **Answer:** Adding more machines/instances to distribute workload.

 ### Question 27

 What is vertical scaling?

 **Answer:** Increasing the resources of an existing machine.

 ### Question 28

 What is elasticity?

 **Answer:** Dynamically increasing or decreasing resources according to workload.

 ### Question 29

 What is replication?

 **Answer:** Maintaining multiple copies of data or services to improve availability and fault tolerance.

 ### Question 30

 What is maintainability?

 **Answer:** The ease with which a system can be modified, corrected, updated, and improved after deployment.

 ### Question 31

 Name the three maintainability principles.

 **Answer:** **Operability, simplicity, and evolvability.**

 ### Question 32

 What type of fault is a disk failure?

 **Answer:** Hardware fault.

 ### Question 33

 What type of fault is a programming bug?

 **Answer:** Software fault.

 ### Question 34

 What type of fault is an administrator accidentally deleting data?

 **Answer:** Human fault.

 ### Question 35

 Which component is optimized for fast searching?

 **Answer:** Search index.

 ### Question 36

 Which component stores persistent application data?

 **Answer:** Database.

 ### Question 37

 Which component provides fast access to frequently requested data?

 **Answer:** Cache.

 ### Question 38

 Which processing model handles continuously arriving events?

 **Answer:** Stream processing.

 ### Question 39

 Which processing model is suitable for large volumes of historical data?

 **Answer:** Batch processing.

---

 # Final Exam Strategy

 For scenario questions, identify the **keyword/problem first**, then select the concept:

 | If the scenario says... | Think... |
| --- | --- |
| "More users" | **Scalability** |
| "One server is overloaded" | **Horizontal scaling** |
| "Make the existing server more powerful" | **Vertical scaling** |
| "Combine both approaches" | **Diagonal scaling** |
| "Traffic changes throughout the day" | **Elasticity** |
| "Server fails" | **Reliability + redundancy/replication** |
| "Disk fails" | **Hardware fault** |
| "Programmer makes a bug" | **Software fault** |
| "Administrator makes a mistake" | **Human fault** |
| "Millions of records, slow search" | **Data management + search index** |
| "CPU is fine, network is slow" | **Network/data-transfer bottleneck** |
| "Requests per second" | **Throughput** |
| "Time to complete request" | **Latency/response time** |
| "Easy to change later" | **Maintainability/evolvability** |
| "Hard for engineers to understand" | **Simplicity** |
| "Hard to run in production" | **Operability** |

**The most important exam distinction to remember is:**

 > **Scalability = can handle more load.**\
>  **Elasticity = can dynamically adjust capacity as load changes.**\
>  **Reliability = continues working despite faults.**\
>  **Maintainability = remains easy to operate, understand, and change.**
