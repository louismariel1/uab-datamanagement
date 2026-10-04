# Comprehensive Exam-Style Questions — Data-Intensive Systems

 Below is an expanded exam set based on the material from **chunks 1–3**, covering data-intensive applications, performance, reliability, scalability, elasticity, maintainability, system building blocks, faults, and load-coping strategies.

 ## Part A — Multiple-Choice Questions

 ### 1\. What is a data-intensive application?

 A. An application that mainly requires a powerful CPU\
 B. An application whose main challenges involve large amounts of data, data complexity, or rapid data changes\
 C. An application that only runs in the cloud\
 D. An application that does not use databases

 **Answer: B**

 **Explanation:** Data-intensive applications are primarily challenged by the **volume, complexity, and rate of change of data**, rather than simply computational complexity.

---

 ### 2\. Which of the following is NOT one of the major characteristics of data-intensive systems?

 A. Large data volume\
 B. Data complexity\
 C. High rate of data change\
 D. Always requiring a single powerful server

 **Answer: D**

 **Explanation:** Data-intensive systems commonly combine multiple specialized components rather than depending on one powerful machine.

---

 ### 3\. Why do modern data-intensive systems use multiple specialized components?

 A. To make systems unnecessarily complicated\
 B. Because one component cannot efficiently solve every data-related problem\
 C. To eliminate the need for databases\
 D. Because cloud computing requires it

 **Answer: B**

 **Explanation:** Different components are optimized for different tasks, such as storage, searching, caching, stream processing, and batch processing.

---

 ### 4\. Which component is primarily used to store structured customer and product information?

 A. Cache\
 B. Search index\
 C. Database\
 D. Load balancer

 **Answer: C**

 **Explanation:** A database provides persistent storage and management of structured application data.

---

 ### 5\. What is the main purpose of a cache?

 A. Permanently store all application data\
 B. Improve response time by keeping frequently accessed data closer to the application\
 C. Replace every database\
 D. Process historical data in batches

 **Answer: B**

 **Explanation:** Caching reduces the need to repeatedly access slower storage or databases, improving performance.

---

 ### 6\. What is the primary purpose of a search index?

 A. To provide fast searching over large datasets\
 B. To back up a database\
 C. To automatically increase CPU capacity\
 D. To replace application servers

 **Answer: A**

 **Explanation:** Search indexes are specialized for efficiently finding information based on queries, such as keyword or name searches.

---

 ### 7\. Which component is particularly appropriate for processing continuous streams of events?

 A. Stream processing system\
 B. Batch processor\
 C. Backup disk\
 D. Static cache

 **Answer: A**

 **Explanation:** Stream processing handles data continuously as events arrive.

---

 ### 8\. Batch processing is most appropriate for:

 A. Processing one user request immediately\
 B. Processing large amounts of historical data\
 C. Reducing network latency instantly\
 D. Handling hardware failures

 **Answer: B**

 **Explanation:** Batch processing is designed to process large datasets, often periodically rather than immediately.

---

 ## Part B — Performance

 ### 9\. Which metric measures how much work a system completes in a given period?

 A. Latency\
 B. Throughput\
 C. Availability\
 D. Maintainability

 **Answer: B**

 **Explanation:** **Throughput** measures the amount of work processed per unit of time, such as requests per second.

---

 ### 10\. What does latency measure?

 A. The amount of storage available\
 B. The number of servers\
 C. The time required to complete an operation or request\
 D. The number of users registered

 **Answer: C**

 **Explanation:** Latency is the time taken for an operation or request to complete.

---

 ### 11\. A website's response time increases from 0.5 seconds to 5 seconds. Which performance metric has clearly deteriorated?

 A. Availability\
 B. Response time\
 C. Storage capacity\
 D. Maintainability

 **Answer: B**

 **Explanation:** Response time has increased significantly, meaning users must wait longer for results.

---

 ### 12\. A server processes 10,000 requests per second. What does this number represent?

 A. Latency\
 B. Throughput\
 C. Availability\
 D. Elasticity

 **Answer: B**

 **Explanation:** Requests processed per second are a measure of throughput.

---

 ### 13\. Which statement best describes response time?

 A. It only measures CPU usage\
 B. It represents the time experienced by the user for a request to complete\
 C. It measures how many servers are available\
 D. It measures database size

 **Answer: B**

 **Explanation:** Response time reflects the overall time experienced by the user and can include network latency, processing time, and other delays.

---

 ## Part C — Reliability and Faults

 ### 14\. What does reliability mean in a data-intensive system?

 A. The system always uses the newest hardware\
 B. The system continues to work correctly despite faults\
 C. The system uses only one server\
 D. The system never changes

 **Answer: B**

 **Explanation:** Reliability is about continuing correct operation despite failures or faults.

---

 ### 15\. A hard drive physically stops working. What type of fault is this?

 A. Human fault\
 B. Software fault\
 C. Hardware fault\
 D. Network fault

 **Answer: C**

 **Explanation:** A physical component failing is a **hardware fault**.

---

 ### 16\. A programmer introduces a bug that stores customer information in the wrong format. What type of fault is this?

 A. Hardware fault\
 B. Software fault\
 C. Human physical fault\
 D. Environmental fault

 **Answer: B**

 **Explanation:** A programming error is a software fault.

---

 ### 17\. An administrator accidentally deletes production data. Which category best describes this?

 A. Hardware fault\
 B. Software fault\
 C. Human fault\
 D. Scaling fault

 **Answer: C**

 **Explanation:** The failure results from human action or error.

---

 ### 18\. Which technique would best protect against a single hard-drive failure?

 A. Caching\
 B. Hardware redundancy or RAID\
 C. Increasing application latency\
 D. Removing backups

 **Answer: B**

 **Explanation:** RAID and replication provide redundancy so that the system can continue operating if one storage device fails.

---

 ### 19\. Which technique can help mitigate software faults introduced by a new deployment?

 A. Testing and rollback\
 B. Removing redundancy\
 C. Increasing database size\
 D. Reducing monitoring

 **Answer: A**

 **Explanation:** Testing can detect bugs before deployment, while rollback allows the system to return to a previous working version.

---

 ### 20\. Which approach can help prevent accidental destructive operations by administrators?

 A. Security controls and access restrictions\
 B. Increasing CPU speed\
 C. Removing authentication\
 D. Using a larger monitor

 **Answer: A**

 **Explanation:** Security mechanisms, permissions, approvals, and controlled access reduce the likelihood and impact of human errors.

---

 ## Part D — Scalability

 ### 21\. What is scalability?

 A. The ability of a system to handle increasing workload\
 B. The ability to prevent every software bug\
 C. The ability to permanently store data\
 D. The ability to reduce all network traffic

 **Answer: A**

 **Explanation:** Scalability concerns how well a system can handle growth in users, traffic, data, or workload.

---

 ### 22\. What is vertical scalability?

 A. Adding more servers\
 B. Increasing the capacity of an existing server\
 C. Removing servers\
 D. Moving data to a cache

 **Answer: B**

 **Explanation:** Vertical scaling, or **scaling up**, means increasing resources such as CPU, RAM, or storage on an existing machine.

---

 ### 23\. What is horizontal scalability?

 A. Making one server more powerful\
 B. Adding more machines or instances to distribute workload\
 C. Reducing database capacity\
 D. Increasing latency

 **Answer: B**

 **Explanation:** Horizontal scaling, or **scaling out**, distributes workload across multiple machines.

---

 ### 24. A service grows from 10,000 to 100,000 concurrent users. Which approach is generally more suitable?

 A. Horizontal scalability\
 B. No scaling\
 C. Only increasing disk capacity\
 D. Reducing the number of servers

 **Answer: A**

 **Explanation:** Adding multiple servers allows the workload to be distributed and provides greater capacity.

---

 ### 25\. What is diagonal scalability?

 A. Using only vertical scaling\
 B. Using only horizontal scaling\
 C. Combining vertical and horizontal scaling\
 D. Eliminating scaling

 **Answer: C**

 **Explanation:** Diagonal scaling combines increasing the capacity of existing machines with adding additional machines.

---

 ## Part E — Elasticity

 ### 26. What is elasticity?

 A. The ability to dynamically add or remove resources according to workload\
 B. The ability to permanently increase server size\
 C. The ability to prevent hardware failures\
 D. The ability to compress data

 **Answer: A**

 **Explanation:** Elasticity allows resources to adapt dynamically to current demand.

---

 ### 27\. A website has very high traffic during the day but very little traffic at night. Which concept is particularly useful?

 A. Elasticity\
 B. Static scaling\
 C. Hardware failure\
 D. Maintainability

 **Answer: A**

 **Explanation:** Elasticity allows resources to increase during peak periods and decrease during low-demand periods.

---

 ### 28\. Which statement correctly distinguishes scalability from elasticity?

 A. They are exactly the same\
 B. Scalability concerns handling growth; elasticity concerns dynamically adapting resources to changing load\
 C. Elasticity only applies to databases\
 D. Scalability only applies to hardware failures

 **Answer: B**

 **Explanation:** A system can be scalable without dynamically adjusting resources. Elasticity specifically emphasizes **automatic or dynamic adaptation**.

---

 ## Part F — Maintainability

 ### 29\. What is maintainability?

 A. The ability to increase CPU speed\
 B. The ability to modify, correct, update, and improve a system after deployment\
 C. The ability to process data faster\
 D. The ability to prevent all failures

 **Answer: B**

 **Explanation:** Maintainability describes how easily a system can be changed and improved over time.

---

 ### 30\. Which three principles are associated with maintainability?

 A. Operability, simplicity, evolvability\
 B. Scalability, latency, throughput\
 C. Hardware, software, network\
 D. CPU, RAM, storage

 **Answer: A**

 **Explanation:** The three principles are **operability, simplicity, and evolvability**.

---

 ### 31\. What does operability mean?

 A. Making the system easy to operate efficiently and effectively\
 B. Making the system impossible to modify\
 C. Increasing database size\
 D. Adding more users

 **Answer: A**

 **Explanation:** Operability focuses on running and managing the system effectively in its production environment.

---

 ### 32\. What is the purpose of simplicity?

 A. To make systems more complicated\
 B. To make the system easier for engineers to understand and maintain\
 C. To eliminate databases\
 D. To increase latency

 **Answer: B**

 **Explanation:** Simplicity reduces unnecessary complexity, making the system easier to understand and maintain.

---

 ### 33\. What does evolvability mean?

 A. The system cannot change\
 B. Engineers can easily modify the system as requirements change\
 C. The system automatically deletes old data\
 D. The system requires more hardware

 **Answer: B**

 **Explanation:** Evolvability allows a system to adapt to future requirements and unexpected use cases.

---

 # Part G — Scenario-Based Exam Questions

 ### 34. E-commerce System

 An e-commerce company processes thousands of orders every hour. It stores customer and product information and requires fast product searches.

 **Which components would you recommend?**

 A. Database, search index, cache, stream processing, batch processing\
 B. Only one database\
 C. Only a cache\
 D. Only a search engine

 **Answer: A**

 **Explanation:** Different components solve different problems:

 - **Database:** persistent customer/product/order data.
- **Search index:** fast product searches.
- **Cache:** faster access to frequently requested information.
- **Stream processing:** real-time order/event processing.
- **Batch processing:** large-scale historical analysis.

---

 ### 35\. Database Failure

 An e-commerce application has two database servers. One database server fails, but users should continue using the application.

 Which technique should be used?

 A. Replication\
 B. Vertical scaling only\
 C. Batch processing\
 D. Caching only

 **Answer: A**

 **Explanation:** Database replication provides redundant copies so another server can continue serving the application.

---

 ### 36\. Massive User Growth

 A social network increases from 10,000 to 1,000,000 users. A single server can no longer handle requests.

 What is the most appropriate general approach?

 A. Horizontal scaling\
 B. Decreasing RAM\
 C. Removing servers\
 D. Disabling the database

 **Answer: A**

 **Explanation:** Horizontal scaling distributes workload among multiple servers.

---

 ### 37\. Highly Variable Traffic

 A video service receives extremely high traffic during major events but low traffic overnight.

 Which approach is most appropriate?

 A. Elasticity\
 B. Permanent vertical scaling only\
 C. No scaling\
 D. Removing capacity during peak periods

 **Answer: A**

 **Explanation:** Elasticity allows the system to dynamically increase capacity during peaks and reduce it afterward.

---

 ### 38\. Search Problem

 A social network database contains 50 million users. Searching for a user by name has become very slow.

 What is the most appropriate solution?

 A. Add a dedicated search index\
 B. Delete half of the users\
 C. Increase network latency\
 D. Remove database indexes

 **Answer: A**

 **Explanation:** A dedicated search index is designed to efficiently locate records in very large datasets.

---

 ### 39\. Network Bottleneck

 A system's CPU usage remains low, but network latency increases significantly as traffic increases.

 What is the most likely bottleneck?

 A. CPU computation\
 B. Network/data transmission\
 C. Software compilation\
 D. User authentication

 **Answer: B**

 **Explanation:** Stable CPU usage combined with increasing network latency suggests that the bottleneck is related to communication or data transfer rather than computation.

---

 ### 40\. Compute vs. Data Management

 A system has low CPU usage but increasingly slow database queries as the dataset grows.

 What is the most likely problem?

 A. Compute limitation\
 B. Data-management limitation\
 C. Lack of RAM only\
 D. Human fault

 **Answer: B**

 **Explanation:** The evidence suggests that data access, indexing, storage, or database processing is becoming the bottleneck.

---

 # Part H — Extended Exam Questions

 ## Question 41 — Reliability

 A company stores millions of customer records. During a system update:

 - A hard drive fails.
- A programmer introduces a data-formatting bug.
- An administrator accidentally deletes production data.

 ### Required:

 1. Classify each fault.
2. Give one reliability technique for each fault.

 ### Model Answer

 1. **Hard-drive failure → Hardware fault**
   - Technique: **Hardware redundancy**, such as RAID or replicated storage.
2. **Programming bug → Software fault**
   - Technique: **Testing, monitoring, and rollback capability**.
3. **Accidental deletion → Human fault**
   - Technique: **Access controls, security mechanisms, backups, and controlled administrative operations**.

---

 # Question 42 — Scalability and Elasticity

 A music streaming application grows from 10,000 to 100,000 concurrent users. The system becomes increasingly slow. Traffic is also highly variable, with very high demand during evenings and very low demand overnight.

 ### Required:

 1. Which scalability approach should be used?
2. Which additional concept is particularly useful?
3. Explain why.

 ### Model Answer

 1. **Horizontal scalability** should be used because multiple servers can distribute requests across the system.
2. **Elasticity** is particularly useful because traffic varies significantly.
3. During peak periods, additional resources can be added. During low-demand periods, unnecessary resources can be removed. This improves resource efficiency while maintaining performance.

---

 # Question 43 — E-Commerce Architecture

 An e-commerce application must:

 - Process thousands of orders per hour.
- Store customer and product information.
- Provide fast product search.
- Continue operating if a server fails.

 ### Required:

 Identify suitable building blocks and explain their purpose.

 ### Model Answer

 - **Database:** stores customer, product, and order information.
- **Search index:** provides fast product searches.
- **Cache:** improves response time for frequently accessed information.
- **Stream processing:** handles order/event processing in real time.
- **Batch processing:** processes large volumes of historical data.
- **Replication:** provides redundancy and helps maintain availability when a server fails.

---

 # Question 44 — Scaling an E-Commerce System

 An e-commerce application currently processes **10,000 orders/hour**. It is expected to process **200,000 orders/hour** in the future.

 ### Required:

 Which scaling strategy would you recommend: vertical, horizontal, diagonal, or elasticity?

 ### Model Answer

 **Horizontal scalability** is the primary recommendation.

 The workload can be distributed across multiple servers, allowing the application to handle substantially more requests and orders.

 However:

 - **Elasticity** would be especially useful if order volume varies significantly.
- **Diagonal scaling** could combine vertical and horizontal scaling.
- **Vertical scaling alone** may eventually reach hardware limits.

---

 # Question 45 — Performance Analysis

 A social network experiences three problems:

 1. Response time increases from **0.5 seconds to 5 seconds** as concurrent users increase.
2. User searches become slow when the database reaches **50 million records**.
3. CPU usage remains stable while network latency increases.

 ### Required:

 For each problem:

 1. Identify the affected performance metric.
2. Identify the likely bottleneck.
3. Recommend a solution.

 ### Model Answer

 | Problem | Metric | Bottleneck | Solution |
| --- | --- | --- | --- |
| Increasing users | Response time/latency | Server capacity | Horizontal scaling + load balancing |
| 50M records | Query latency/throughput | Data management/search | Search index + database scaling |
| Network latency | Latency | Network/data transmission | Elastic or horizontally scalable network resources |

---

 # Part I — Higher-Difficulty Questions

 ## Question 46

 A company argues:

 > "Our application is scalable because we bought a much more powerful server."

 Is this statement necessarily correct?

 **Answer:** Not necessarily.

 **Explanation:** Increasing the capacity of one server is **vertical scalability**. It can improve capacity, but scalability is broader than simply buying a more powerful machine. For large systems, horizontal scaling may provide better growth, redundancy, and flexibility.

---

 ## Question 47

 A system can handle 1 million users, but during periods of low traffic it continues running 100 servers.

 Is the system scalable?

 **Answer:** It may be scalable, but it is **not necessarily elastic**.

 **Explanation:** Scalability means the system can handle increasing workload. Elasticity means it can **dynamically adjust resources according to workload**. If it never reduces resources during low demand, it is not taking full advantage of elasticity.

---

 ## Question 48

 Why can a system be reliable even though faults occur?

 **Answer:** Because **reliability does not mean that faults never happen**. It means the system can continue operating correctly despite faults.

 For example, if one database server fails and a replica takes over, a fault occurred, but the service remains available.

---

 ## Question 49

 Why is maintainability important for long-lived systems?

 **Answer:** Requirements, technologies, users, and business needs change over time. A maintainable system makes it easier for engineers to correct bugs, introduce features, update components, and adapt to new requirements.

---

 ## Question 50

 A company wants its system to handle unpredictable demand efficiently.

 Which combination is most appropriate?

 A. Reliability only\
 B. Horizontal scalability \+ elasticity\
 C. Maintainability only\
 D. Batch processing only

 **Answer: B**

 **Explanation:** Horizontal scalability provides additional capacity, while elasticity allows that capacity to be dynamically adjusted as demand changes.

---

 # Part J — Quick Exam Revision

 Memorize these distinctions:

 | Concept | Key idea |
| --- | --- |
| **Throughput** | How much work is completed per unit of time |
| **Latency** | How long an operation takes |
| **Response time** | Time experienced by the user for a request |
| **Reliability** | Continue working correctly despite faults |
| **Hardware fault** | Physical component failure |
| **Software fault** | Bug/error in software |
| **Human fault** | Mistake caused by a person |
| **Scalability** | Ability to handle increasing workload |
| **Vertical scaling** | Make one machine more powerful |
| **Horizontal scaling** | Add more machines |
| **Diagonal scaling** | Combine vertical + horizontal scaling |
| **Elasticity** | Dynamically add/remove resources based on demand |
| **Maintainability** | Ease of modifying and improving a system |
| **Operability** | Ease of operating the system |
| **Simplicity** | Reduce unnecessary complexity |
| **Evolvability** | Ease of adapting to future requirements |
| **Database** | Persistent data storage |
| **Cache** | Faster access to frequently used data |
| **Search index** | Fast searching |
| **Stream processing** | Continuous/real-time event processing |
| **Batch processing** | Large-scale processing of accumulated data |
| **Replication** | Maintain copies for redundancy/availability |

## Most Important Exam Patterns

 If you see:

 - **"One server becomes insufficient"** → think **horizontal scaling**.
- **"Make the existing server more powerful"** → **vertical scaling**.
- **"Combine both"** → **diagonal scaling**.
- **"Traffic changes unpredictably"** → **elasticity**.
- **"Server/database fails but service must continue"** → **replication/redundancy**.
- **"Programmer introduces a bug"** → **software fault**.
- **"Administrator accidentally deletes data"** → **human fault**.
- **"Hard disk stops working"** → **hardware fault**.
- **"Search becomes slow with millions of records"** → **search index/data-management solution**.
- **"Requests per second"** → **throughput**.
- **"Time taken by request"** → **latency/response time**.
- **"Easy to modify after deployment"** → **maintainability**.
- **"Easy for engineers to understand"** → **simplicity**.
- **"Easy to change for future requirements"** → **evolvability**.
- **"Efficiently operate in production"** → **operability**.
