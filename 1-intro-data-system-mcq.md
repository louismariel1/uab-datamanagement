# Comprehensive MCQ Bank

 ## Data-Intensive Systems, Reliability, Scalability & Maintainability

 **Instructions:** Choose the **one best answer** for each question.\
 The answer and explanation are provided immediately after each question.

---

 # Section 1 — Data-Intensive Applications

 ### Question 1

 What is the main characteristic of a **data-intensive application**?

 A. It only requires a powerful CPU\
 B. Its main challenges involve the volume, complexity, and rate of change of data\
 C. It can only run on a single server\
 D. It does not require a database

 **Answer: B**

 **Explanation:** Data-intensive applications are primarily challenged by large amounts of data, complex data processing, and rapidly changing data. They often require multiple specialized components rather than one tool.

---

 ### Question 2

 Which of the following is most likely to be a data-intensive application?

 A. A simple calculator\
 B. A basic text editor\
 C. An e-commerce platform processing millions of orders\
 D. A static HTML page

 **Answer: C**

 **Explanation:** An e-commerce platform may need to manage millions of customers, products, orders, searches, and transactions, making data management a major architectural concern.

---

 ### Question 3

 Why do modern data-intensive systems typically use multiple specialized components?

 A. To make the architecture unnecessarily complicated\
 B. Because one tool is always incapable of storing any data\
 C. Because different workloads have different requirements\
 D. Because databases cannot process requests

 **Answer: C**

 **Explanation:** Different workloads require different technologies. For example, databases store data, caches speed up frequent requests, search indexes provide fast search, and stream processing handles continuous events.

---

 ### Question 4

 Which factor is **NOT** typically associated with data-intensive applications?

 A. Large data volume\
 B. Data complexity\
 C. High rate of data change\
 D. Complete absence of users

 **Answer: D**

 **Explanation:** Data-intensive applications commonly have many users and requests. Large data volume, complexity, and changing data are common characteristics.

---

 # Section 2 — Data-System Building Blocks

 ### Question 5

 What is the primary purpose of a database?

 A. To dynamically add servers\
 B. To store and retrieve application data\
 C. To reduce network bandwidth only\
 D. To replace all other system components

 **Answer: B**

 **Explanation:** Databases store information such as customers, products, orders, and transactions and provide mechanisms for retrieving and updating that information.

---

 ### Question 6

 An online store wants to reduce the time required to retrieve frequently requested product information. Which component would be most appropriate?

 A. Cache\
 B. Batch processor\
 C. Backup\
 D. RAID controller

 **Answer: A**

 **Explanation:** A cache stores frequently accessed data in a faster layer, reducing response time and decreasing the load on the main database.

---

 ### Question 7

 What is the primary purpose of a **search index**?

 A. To provide fast searching over large datasets\
 B. To automatically back up all servers\
 C. To increase RAM\
 D. To replace a cache in every situation

 **Answer: A**

 **Explanation:** Search indexes are specialized for efficiently finding information, especially when users search large collections using keywords.

---

 ### Question 8

 An e-commerce application must quickly find products matching the keyword "wireless headphones" among 100 million products. Which component is most appropriate?

 A. Search index\
 B. Batch processor\
 C. RAID\
 D. Load balancer only

 **Answer: A**

 **Explanation:** A dedicated search index is designed for fast keyword-based searching over large datasets.

---

 ### Question 9

 Which component is best suited for processing events continuously as they arrive?

 A. Batch processing\
 B. Stream processing\
 C. Backup storage\
 D. Static storage

 **Answer: B**

 **Explanation:** Stream processing handles continuously arriving data, such as orders, user activity, sensor events, or real-time transactions.

---

 ### Question 10

 Which is an example of **batch processing**?

 A. Processing a user click immediately\
 B. Processing live stock prices as they arrive\
 C. Analyzing ten years of historical sales data overnight\
 D. Returning a cached web page

 **Answer: C**

 **Explanation:** Batch processing handles large collections of stored data, usually periodically or at scheduled times.

---

 ### Question 11

 Which building block would be most appropriate for analyzing millions of historical orders?

 A. Cache\
 B. Batch processing\
 C. Search index only\
 D. Load balancer

 **Answer: B**

 **Explanation:** Batch processing is designed for processing large volumes of accumulated data, such as historical orders.

---

 ### Question 12

 Which combination correctly matches the component with its purpose?

 A. Cache → historical analytics\
 B. Search index → fast keyword searching\
 C. Batch processing → dynamic server scaling\
 D. Database → network routing

 **Answer: B**

 **Explanation:** Search indexes are specifically designed to support efficient searching.

---

 ### Question 13

 Which architecture best represents a modern data-intensive application?

 A. One tool performing every task\
 B. Multiple specialized components working together\
 C. Only a CPU and RAM\
 D. Only a search engine

 **Answer: B**

 **Explanation:** Data-intensive systems commonly combine databases, caches, search indexes, stream processing, batch processing, and other specialized components.

---

 # Section 3 — Performance Metrics

 ### Question 14

 What does **throughput** measure?

 A. The time required for one request\
 B. The amount of work completed per unit of time\
 C. The amount of memory in a server\
 D. The number of database replicas

 **Answer: B**

 **Explanation:** Throughput measures how much work a system can complete in a given period, such as orders per hour or requests per second.

---

 ### Question 15

 A system processes 20,000 orders every hour. This is an example of:

 A. Latency\
 B. Response time\
 C. Throughput\
 D. Elasticity

 **Answer: C**

 **Explanation:** Orders per hour measures the amount of work completed during a period, which is throughput.

---

 ### Question 16

 A database query takes 80 milliseconds to complete. Which metric is most directly being described?

 A. Throughput\
 B. Latency\
 C. Scalability\
 D. Maintainability

 **Answer: B**

 **Explanation:** Latency describes the time taken by an operation or request.

---

 ### Question 17

 A web page takes five seconds from the user's request until the page is fully returned. This primarily describes:

 A. Response time\
 B. Throughput\
 C. Replication\
 D. Elasticity

 **Answer: A**

 **Explanation:** Response time represents the total time experienced by the user while waiting for the system to respond.

---

 ### Question 18

 Which statement is correct?

 A. Throughput measures how long one request takes\
 B. Latency measures how much work is completed per hour\
 C. Throughput measures work completed, while latency measures time for an operation\
 D. Throughput and latency always mean exactly the same thing

 **Answer: C**

 **Explanation:** Throughput is about the amount of work completed per unit time, while latency concerns the duration of an operation.

---

 ### Question 19

 A system's response time increases from 0.5 seconds to 5 seconds. Which performance metric is directly affected?

 A. Response time\
 B. Replication\
 C. Maintainability\
 D. Storage capacity

 **Answer: A**

 **Explanation:** The scenario directly describes a change in response time.

---

 ### Question 20

 A system can process 50,000 requests per second but each individual request takes a long time to complete. Which statement is possible?

 A. High throughput and high latency can coexist\
 B. High throughput always means low latency\
 C. Latency cannot be measured in distributed systems\
 D. Throughput and latency are identical

 **Answer: A**

 **Explanation:** Throughput and latency are different dimensions. A system can process many requests overall while individual requests still take a long time.

---

 # Section 4 — Reliability

 ### Question 21

 What does **reliability** mean?

 A. The system always uses the newest hardware\
 B. The system continues performing correctly despite faults\
 C. The system always has maximum CPU usage\
 D. The system never changes

 **Answer: B**

 **Explanation:** Reliability is the ability of a system to continue functioning correctly despite component failures or other faults.

---

 ### Question 22

 A server's hard drive physically stops working. What type of fault is this?

 A. Human fault\
 B. Software fault\
 C. Hardware fault\
 D. Network policy fault

 **Answer: C**

 **Explanation:** A physical component failing is a hardware fault.

---

 ### Question 23

 A programmer introduces a bug that stores customer data in the wrong format. What type of fault is this?

 A. Hardware fault\
 B. Software fault\
 C. Human fault only\
 D. Network fault

 **Answer: B**

 **Explanation:** The immediate problem is a defect in the software, making it a software fault.

---

 ### Question 24

 An operator accidentally executes a production script that deletes customer records. What type of fault is this?

 A. Hardware fault\
 B. Human fault\
 C. Hardware redundancy fault\
 D. Database fault only

 **Answer: B**

 **Explanation:** The fault results from an incorrect human action.

---

 ### Question 25

 Which technique is most appropriate for mitigating a hard-drive failure?

 A. RAID or hardware redundancy\
 B. Search indexing\
 C. Increasing query latency\
 D. Removing all backups

 **Answer: A**

 **Explanation:** RAID and other forms of hardware redundancy allow the system to continue operating when an individual storage component fails.

---

 ### Question 26

 Which technique can help mitigate software faults?

 A. Testing and monitoring\
 B. Removing all monitoring\
 C. Using fewer tests\
 D. Disabling rollback

 **Answer: A**

 **Explanation:** Testing can detect software defects before deployment, while monitoring can detect problems after deployment. Rollback can also help recover from faulty releases.

---

 ### Question 27

 Which technique can help reduce the impact of human mistakes in production?

 A. Removing access controls\
 B. Security and access controls\
 C. Giving every employee administrator privileges\
 D. Disabling auditing

 **Answer: B**

 **Explanation:** Access controls, permissions, approval processes, and auditing can reduce the likelihood and impact of human errors.

---

 # Section 5 — Redundancy, Replication and Availability

 ### Question 28

 What is the purpose of redundancy?

 A. To create additional components that can take over when one fails\
 B. To eliminate all data\
 C. To make every system slower\
 D. To prevent scalability

 **Answer: A**

 **Explanation:** Redundancy provides alternative resources so that failure of one component does not necessarily bring down the system.

---

 ### Question 29

 A database has three replicas. If one server fails, another replica can continue serving requests. This is an example of:

 A. Replication\
 B. Vertical scaling only\
 C. Batch processing\
 D. Caching only

 **Answer: A**

 **Explanation:** Replication maintains multiple copies of data or services to improve availability and fault tolerance.

---

 ### Question 30

 A company wants its application to remain available when one database server fails. Which technique is most appropriate?

 A. Database replication\
 B. Removing databases\
 C. Increasing latency\
 D. Using only one server

 **Answer: A**

 **Explanation:** Replication allows another database instance to take over or continue serving requests when one fails.

---

 ### Question 31

 Which statement best distinguishes replication from backup?

 A. Replication helps availability; backups primarily help recovery from data loss\
 B. Backups always provide real-time availability\
 C. Replication is only used for hardware\
 D. They are exactly the same

 **Answer: A**

 **Explanation:** Replicas can keep services running when a server fails. Backups provide a recoverable copy of data, especially useful after accidental deletion or corruption.

---

 ### Question 32

 Which situation is best addressed by a backup?

 A. A user accidentally deletes production data\
 B. A server needs more CPU\
 C. A search query needs faster indexing\
 D. Traffic increases during lunch

 **Answer: A**

 **Explanation:** Backups provide a mechanism to recover lost or corrupted data.

---

 # Section 6 — Scalability

 ### Question 33

 What is scalability?

 A. The ability to handle increasing workloads by increasing capacity\
 B. The ability to permanently reduce resources\
 C. The ability to remove all servers\
 D. The ability to prevent future changes

 **Answer: A**

 **Explanation:** Scalability concerns a system's ability to handle increasing users, traffic, data, or workload.

---

 ### Question 34

 What is **vertical scalability**?

 A. Adding more servers\
 B. Making an existing server more powerful\
 C. Automatically removing servers\
 D. Adding database replicas only

 **Answer: B**

 **Explanation:** Vertical scaling, or scaling up, increases the resources of an existing machine, such as CPU, RAM, or storage.

---

 ### Question 35

 What is **horizontal scalability**?

 A. Increasing the CPU of one server\
 B. Adding more servers to distribute workload\
 C. Reducing memory\
 D. Replacing databases with caches

 **Answer: B**

 **Explanation:** Horizontal scaling, or scaling out, adds additional machines or instances.

---

 ### Question 36

 A server is upgraded from 16 GB RAM to 128 GB RAM. What type of scaling is this?

 A. Horizontal\
 B. Vertical\
 C. Elastic\
 D. Batch

 **Answer: B**

 **Explanation:** Increasing the resources of one existing server is vertical scaling.

---

 ### Question 37

 A company adds five additional web servers to handle increased traffic. What type of scaling is this?

 A. Vertical\
 B. Horizontal\
 C. Batch\
 D. Backup

 **Answer: B**

 **Explanation:** Adding more servers is horizontal scaling.

---

 ### Question 38

 A music application grows from 10,000 to 100,000 concurrent users. Which approach is generally most appropriate?

 A. Horizontal scalability\
 B. Removing servers\
 C. Only increasing storage\
 D. Disabling caching

 **Answer: A**

 **Explanation:** A large increase in concurrent users is well suited to distributing workload across multiple servers.

---

 ### Question 39

 What is a major limitation of vertical scaling?

 A. It can never improve performance\
 B. A single machine has physical and hardware limits\
 C. It always requires thousands of servers\
 D. It cannot use RAM

 **Answer: B**

 **Explanation:** A server can only be upgraded up to a certain physical and economic limit.

---

 ### Question 40

 Which is an advantage of horizontal scaling?

 A. It can distribute workload across multiple machines\
 B. It requires one infinitely powerful server\
 C. It eliminates the need for software architecture\
 D. It prevents all failures

 **Answer: A**

 **Explanation:** Horizontal scaling distributes work across multiple machines and can support large-scale growth.

---

 # Section 7 — Diagonal Scaling

 ### Question 41

 What is diagonal scaling?

 A. Only increasing RAM\
 B. Only adding servers\
 C. Combining vertical and horizontal scaling\
 D. Automatically deleting resources

 **Answer: C**

 **Explanation:** Diagonal scaling combines scaling up and scaling out.

---

 ### Question 42

 Which scenario best represents diagonal scaling?

 A. Increasing one server's RAM and later adding more servers\
 B. Only reducing CPU\
 C. Only adding one cache\
 D. Only creating a backup

 **Answer: A**

 **Explanation:** Increasing the capacity of existing machines and adding additional machines combines vertical and horizontal scaling.

---

 ### Question 43

 Why might diagonal scaling be useful?

 A. It combines the advantages of different scaling approaches\
 B. It eliminates all system complexity\
 C. It guarantees zero latency\
 D. It prevents future growth

 **Answer: A**

 **Explanation:** Diagonal scaling gives architects flexibility to increase individual machine capacity and add machines when needed.

---

 # Section 8 — Elasticity

 ### Question 44

 What is elasticity?

 A. The ability to dynamically add or remove resources according to workload\
 B. The ability to permanently use maximum resources\
 C. The ability to avoid cloud computing\
 D. The ability to store only static data

 **Answer: A**

 **Explanation:** Elasticity allows a system to adapt its resource capacity dynamically as workload changes.

---

 ### Question 45

 Which situation most strongly benefits from elasticity?

 A. Workload remains exactly constant\
 B. Traffic changes significantly between peak and off-peak periods\
 C. A system never receives requests\
 D. A server needs a permanent hardware upgrade

 **Answer: B**

 **Explanation:** Elasticity is particularly useful when demand varies significantly over time.

---

 ### Question 46

 A website normally requires 5 servers but needs 30 during a major sale. After the sale, demand drops and the system returns to 5 servers. This is an example of:

 A. Vertical scaling only\
 B. Elasticity\
 C. Batch processing\
 D. Static replication

 **Answer: B**

 **Explanation:** The system dynamically increases and decreases resources based on workload.

---

 ### Question 47

 Why is elasticity particularly associated with cloud computing?

 A. Cloud environments make dynamic resource allocation easier\
 B. Cloud systems cannot scale\
 C. Cloud systems never experience variable workloads\
 D. Cloud computing eliminates all databases

 **Answer: A**

 **Explanation:** Cloud platforms make it relatively easy to provision and release resources dynamically.

---

 ### Question 48

 Which statement best distinguishes scalability from elasticity?

 A. Scalability concerns handling growth; elasticity concerns dynamically adapting to workload changes\
 B. They are always identical\
 C. Elasticity only means vertical scaling\
 D. Scalability only applies to databases

 **Answer: A**

 **Explanation:** Scalability is the broader ability to handle increasing workloads, while elasticity focuses on dynamically adjusting capacity as demand changes.

---

 # Section 9 — Maintainability

 ### Question 49

 What is maintainability?

 A. The ability to modify, correct, update, and improve a system after deployment\
 B. The ability to increase CPU speed\
 C. The ability to store data permanently\
 D. The ability to eliminate users

 **Answer: A**

 **Explanation:** Maintainability describes how easily a system can be operated, understood, corrected, modified, and evolved.

---

 ### Question 50

 Which three principles are associated with maintainability?

 A. Throughput, latency, response time\
 B. Operability, simplicity, evolvability\
 C. Replication, RAID, backup\
 D. CPU, RAM, storage

 **Answer: B**

 **Explanation:** The lecture identifies **operability, simplicity, and evolvability** as the three important maintainability principles.

---

 ### Question 51

 What does **operability** mean?

 A. The system can operate efficiently and effectively in production\
 B. The system can only be used by developers\
 C. The system must have maximum CPU usage\
 D. The system cannot be monitored

 **Answer: A**

 **Explanation:** Operability concerns making a system easy to run and manage in its production environment.

---

 ### Question 52

 Which practice supports operability?

 A. Monitoring and logging\
 B. Removing all logs\
 C. Preventing deployment automation\
 D. Hiding system failures

 **Answer: A**

 **Explanation:** Monitoring, logging, deployment tools, and operational procedures make systems easier to operate and troubleshoot.

---

 ### Question 53

 What is the goal of **simplicity**?

 A. Add as many components as possible\
 B. Make the system easy for engineers to understand\
 C. Prevent all future changes\
 D. Increase system complexity

 **Answer: B**

 **Explanation:** Simplicity reduces unnecessary complexity, making the system easier to understand, debug, and maintain.

---

 ### Question 54

 A company redesigns a complicated system so that new engineers can understand it quickly. Which maintainability principle is being emphasized?

 A. Simplicity\
 B. Throughput\
 C. Elasticity\
 D. Replication

 **Answer: A**

 **Explanation:** Simplicity focuses on reducing complexity and making the system easier to understand.

---

 ### Question 55

 What does **evolvability** mean?

 A. Making it easy to adapt the system to future requirements\
 B. Making the system impossible to change\
 C. Increasing server CPU\
 D. Storing data in a cache

 **Answer: A**

 **Explanation:** Evolvability means designing systems so engineers can make changes as requirements and use cases evolve.

---

 ### Question 56

 A company expects its business requirements to change significantly over the next five years. Which maintainability principle is especially important?

 A. Evolvability\
 B. RAID\
 C. Latency\
 D. Throughput

 **Answer: A**

 **Explanation:** Evolvability prepares the architecture for future changes and unanticipated use cases.

---

 # Section 10 — Cloud Computing

 ### Question 57

 Which concept is most directly associated with dynamically adding and removing resources in cloud environments?

 A. Elasticity\
 B. Batch processing\
 C. Human fault\
 D. Simplicity

 **Answer: A**

 **Explanation:** Elasticity allows cloud systems to dynamically adjust resources according to workload.

---

 ### Question 58

 An application has highly unpredictable traffic. Which concept would be particularly useful?

 A. Elasticity\
 B. Static capacity\
 C. Single-server architecture\
 D. Manual backups only

 **Answer: A**

 **Explanation:** Elasticity allows the system to increase resources during unexpected peaks and reduce them during periods of low demand.

---

 # Section 11 — Compute Limits vs. Data Management

 ### Question 59

 CPU usage is at 95% and requests are becoming slow. What is a likely bottleneck?

 A. Compute resources\
 B. Search indexing only\
 C. Human permissions\
 D. Data backup

 **Answer: A**

 **Explanation:** Very high CPU utilization suggests that computational resources may be limiting performance.

---

 ### Question 60

 A database contains 50 million records and searching by name becomes increasingly slow, while CPU usage remains stable. What is the more likely problem?

 A. Data management\
 B. CPU capacity\
 C. Human fault\
 D. Hardware disk failure necessarily

 **Answer: A**

 **Explanation:** Stable CPU usage combined with degraded search performance suggests a data-access or search problem rather than a pure compute limitation.

---

 ### Question 61

 Which solution is particularly appropriate for very slow keyword searches over a huge dataset?

 A. Dedicated search index\
 B. Remove the database\
 C. Increase network latency\
 D. Disable caching

 **Answer: A**

 **Explanation:** A search index is optimized for efficient keyword-based retrieval from large datasets.

---

 ### Question 62

 Why should an engineer identify the bottleneck before selecting a scaling technique?

 A. Different bottlenecks require different solutions\
 B. Every problem requires vertical scaling\
 C. Every problem requires horizontal scaling\
 D. Scaling never affects performance

 **Answer: A**

 **Explanation:** A CPU bottleneck, database bottleneck, network bottleneck, and search bottleneck may require completely different solutions.

---

 # Section 12 — Integrated Scenario Questions

 ### Question 63

 An online store experiences a sudden tenfold increase in traffic during a holiday sale. Which combination is most appropriate?

 A. Horizontal scaling and elasticity\
 B. Batch processing and human access controls only\
 C. Vertical scaling only and no monitoring\
 D. Remove database replicas

 **Answer: A**

 **Explanation:** Horizontal scaling allows the workload to be distributed across multiple servers, while elasticity allows resources to increase during the peak and decrease afterward.

---

 ### Question 64

 A company has one database server. If it fails, the entire application becomes unavailable. What architectural problem exists?

 A. A single point of failure\
 B. Excessive elasticity\
 C. Excessive throughput\
 D. Too much simplicity

 **Answer: A**

 **Explanation:** Depending on one critical server creates a single point of failure. Replication or redundancy can reduce this risk.

---

 ### Question 65

 Which architecture would best support an e-commerce application requiring customer storage, fast product search, real-time order handling, and historical analytics?

 A. Database + search index + stream processing \+ batch processing\
 B. Cache only\
 C. Search index only\
 D. One text file

 **Answer: A**

 **Explanation:** Each workload has a suitable specialized component: database for storage, search index for search, stream processing for real-time events, and batch processing for historical analysis.

---

 ### Question 66

 An e-commerce system must remain available even if one database server fails. Which design is best?

 A. One large database server with no backup\
 B. Multiple database replicas\
 C. One cache without a database\
 D. Batch processing only

 **Answer: B**

 **Explanation:** Database replication provides redundancy and allows another database instance to continue serving requests after a failure.

---

 ### Question 67

 A music streaming service increases from 10,000 to 100,000 concurrent users over several weeks. The system is becoming slow. What is the most appropriate primary scaling strategy?

 A. Horizontal scalability\
 B. Remove servers\
 C. Disable caching\
 D. Only increase database backups

 **Answer: A**

 **Explanation:** A large increase in concurrent users is well suited to distributing the workload across multiple servers.

---

 ### Question 68

 The same music streaming service experiences extremely high traffic between 7 PM and 10 PM but low traffic overnight. What additional concept is especially useful?

 A. Elasticity\
 B. Hardware fault\
 C. Simplicity only\
 D. Batch processing only

 **Answer: A**

 **Explanation:** Elasticity allows resources to increase during peak hours and decrease during low-demand periods.

---

 ### Question 69

 A social network has 50 million users. User searches become slower, but CPU usage remains low. What should the engineers investigate first?

 A. Search/data management\
 B. CPU capacity only\
 C. Human permissions\
 D. RAID configuration only

 **Answer: A**

 **Explanation:** Low CPU usage suggests that CPU is not the main bottleneck. The search mechanism, database access, indexing, or data organization should be investigated.

---

 ### Question 70

 A programmer deploys buggy code to production. Which combination provides the strongest mitigation?

 A. Testing, monitoring, and rollback capability\
 B. Removing monitoring\
 C. Adding more RAM only\
 D. Increasing network latency

 **Answer: A**

 **Explanation:** Testing helps prevent faulty code from being deployed, monitoring detects problems, and rollback allows recovery to a known-good version.

---

 # Section 13 — Exam-Style Application

 ### Question 71

 A company processes 10,000 orders per hour. It later grows to 200,000 orders per hour. Which scaling approach is generally the strongest choice?

 A. Horizontal scalability\
 B. Only vertical scalability\
 C. No scaling\
 D. Only backup

 **Answer: A**

 **Explanation:** Horizontal scaling allows the workload to be distributed across multiple machines and provides more flexible long-term growth.

---

 ### Question 72

 Why might vertical scaling eventually become insufficient?

 A. A single machine has physical and economic limits\
 B. Vertical scaling cannot use RAM\
 C. Vertical scaling always reduces performance\
 D. Vertical scaling prevents databases

 **Answer: A**

 **Explanation:** Hardware cannot be upgraded indefinitely. At some point, adding more machines becomes more practical.

---

 ### Question 73

 Which scenario most clearly requires **elasticity** rather than simply scalability?

 A. A company expects user numbers to grow permanently over five years\
 B. A website experiences unpredictable traffic spikes and drops every day\
 C. A server needs more RAM permanently\
 D. A database needs a backup

 **Answer: B**

 **Explanation:** Elasticity is especially valuable when resource requirements fluctuate significantly over time.

---

 ### Question 74

 A company wants new engineers to understand its software quickly and make changes safely. Which two maintainability principles are most directly relevant?

 A. Simplicity and evolvability\
 B. Throughput and latency\
 C. RAID and replication\
 D. Vertical and horizontal scaling

 **Answer: A**

 **Explanation:** Simplicity makes the system easier to understand, while evolvability makes future changes easier.

---

 ### Question 75

 A production system is difficult to monitor, troubleshoot, and operate. Which maintainability principle is weakest?

 A. Operability\
 B. Scalability\
 C. Replication\
 D. Throughput

 **Answer: A**

 **Explanation:** Operability concerns how easily a system can be operated and managed in production.

---

 ### Question 76

 Which architecture is most likely to provide both high availability and scalability?

 A. One powerful server with no redundancy\
 B. Multiple servers with replication and distributed workload\
 C. One database with no backup\
 D. One server running every component

 **Answer: B**

 **Explanation:** Multiple servers allow horizontal scaling, while replication provides redundancy and improves availability.

---

 # Section 14 — Fault Classification

 ### Question 77

 Match the following correctly:

 1. Hard drive stops working
2. Programmer introduces a bug
3. Operator accidentally deletes data

 A. Human → Hardware → Software\
 B. Hardware → Software → Human\
 C. Software → Human → Hardware\
 D. Hardware → Human → Software

 **Answer: B**

 **Explanation:**

 1. Hard drive failure → **Hardware fault**
2. Programmer's bug → **Software fault**
3. Operator's mistake → **Human fault**

---

 ### Question 78

 A hard drive fails, but the application continues running because another storage device contains the required data. What concept made this possible?

 A. Redundancy\
 B. Latency\
 C. Simplicity\
 D. Batch processing

 **Answer: A**

 **Explanation:** Redundant hardware allows the system to continue operating after an individual component fails.

---

 ### Question 79

 A production database is accidentally deleted, and the company restores it from yesterday's copy. What technique was used?

 A. Backup and recovery\
 B. Horizontal scaling\
 C. Search indexing\
 D. Elasticity

 **Answer: A**

 **Explanation:** A backup provides a recoverable copy of data after accidental deletion or corruption.

---

 # Section 15 — Comprehensive Mixed Questions

 ### Question 80

 Which statement is the **most accurate** description of a good data-intensive architecture?

 A. It should use one tool for every problem\
 B. It should prioritize CPU performance above everything else\
 C. It should combine appropriate specialized components while balancing reliability, scalability, and maintainability\
 D. It should avoid redundancy to reduce complexity

 **Answer: C**

 **Explanation:** Good architecture balances multiple quality requirements and uses specialized components according to workload needs.

---

 ### Question 81

 Which of the following is **NOT** one of the three maintainability principles discussed?

 A. Operability\
 B. Simplicity\
 C. Evolvability\
 D. Replication

 **Answer: D**

 **Explanation:** The three maintainability principles are operability, simplicity, and evolvability. Replication is a reliability technique.

---

 ### Question 82

 Which of the following is **NOT** a performance metric?

 A. Throughput\
 B. Latency\
 C. Response time\
 D. Evolvability

 **Answer: D**

 **Explanation:** Evolvability is a maintainability principle. Throughput, latency, and response time are performance-related metrics.

---

 ### Question 83

 Which of the following is primarily a **reliability** concern?

 A. Making the application easier for engineers to modify\
 B. Continuing to operate when a server fails\
 C. Increasing the number of requests per second\
 D. Reducing code complexity

 **Answer: B**

 **Explanation:** Reliability concerns continued correct operation despite faults and failures.

---

 ### Question 84

 Which of the following is primarily a **maintainability** concern?

 A. Making future changes easier\
 B. Adding database replicas\
 C. Increasing server CPU\
 D. Processing more orders per second

 **Answer: A**

 **Explanation:** Making future modifications easier is the goal of evolvability, which is part of maintainability.

---

 ### Question 85

 Which statement about cloud environments is most accurate?

 A. Cloud environments make elasticity particularly practical\
 B. Cloud systems cannot scale horizontally\
 C. Cloud systems eliminate software faults\
 D. Cloud systems do not require reliability mechanisms

 **Answer: A**

 **Explanation:** Cloud environments make it easier to provision and release resources dynamically, making elasticity practical.

---

 ### Question 86

 A system's CPU usage is stable, but network latency increases as traffic increases. Which statement is most appropriate?

 A. CPU is definitely the bottleneck\
 B. The problem may be related to networking/data transmission rather than computation\
 C. The system needs more RAM automatically\
 D. The database must be deleted

 **Answer: B**

 **Explanation:** Stable CPU usage suggests computation may not be the main issue. Increasing network latency points toward communication or data-transfer limitations.

---

 ### Question 87

 A system has unpredictable workload changes. Which architecture characteristic is most valuable?

 A. Elasticity\
 B. Fixed capacity\
 C. No redundancy\
 D. Maximum complexity

 **Answer: A**

 **Explanation:** Elasticity allows the system to dynamically adapt resources to unpredictable workload changes.

---

 ### Question 88

 Which approach would best improve the performance of frequently requested data?

 A. Caching\
 B. Removing indexes\
 C. Disabling replication\
 D. Increasing human permissions

 **Answer: A**

 **Explanation:** Caching keeps frequently requested information in a faster-access layer and reduces repeated work against the main database.

---

 ### Question 89

 A company needs fast searches over millions of products. Which combination is most appropriate?

 A. Database + dedicated search index\
 B. Batch processing only\
 C. Backup + RAID only\
 D. CPU + RAM only

 **Answer: A**

 **Explanation:** The database stores the product information, while the search index provides efficient keyword-based searching.

---

 ### Question 90

 Which statement best summarizes the overall lecture?

 A. The only important property of a system is CPU performance\
 B. Data systems should focus only on storage\
 C. Effective systems balance performance, reliability, scalability, and maintainability\
 D. Horizontal scaling solves every system problem

 **Answer: C**

 **Explanation:** The lecture emphasizes that modern data-intensive systems must balance multiple concerns: performance, reliability, scalability, and maintainability.

---

 # Final Rapid-Review Questions

 ### Question 91

 **Database → ?**

 A. Store application data\
 B. Dynamic scaling\
 C. Fast keyword search only\
 D. Human security

 **Answer: A**

---

 ### Question 92

 **Cache → ?**

 A. Historical analysis\
 B. Faster access to frequently used data\
 C. Hardware redundancy\
 D. Future software changes

 **Answer: B**

---

 ### Question 93

 **Search index → ?**

 A. Fast searching\
 B. Backup\
 C. Server scaling\
 D. Monitoring

 **Answer: A**

---

 ### Question 94

 **Stream processing → ?**

 A. Continuous incoming data\
 B. Historical data only\
 C. Hardware failures\
 D. Server upgrades

 **Answer: A**

---

 ### Question 95

 **Batch processing → ?**

 A. Large amounts of accumulated data\
 B. Dynamic resource allocation\
 C. Human access control\
 D. Hardware redundancy

 **Answer: A**

---

 ### Question 96

 **Vertical scaling → ?**

 A. Add machines\
 B. Make one machine more powerful\
 C. Remove resources dynamically\
 D. Replicate data

 **Answer: B**

---

 ### Question 97

 **Horizontal scaling → ?**

 A. Add machines\
 B. Add RAM to one machine\
 C. Reduce workload\
 D. Create backups

 **Answer: A**

---

 ### Question 98

 **Elasticity → ?**

 A. Dynamically adjust resources\
 B. Permanently increase CPU\
 C. Store data\
 D. Search data

 **Answer: A**

---

 ### Question 99

 **Reliability → ?**

 A. Keep working despite faults\
 B. Make code simpler\
 C. Increase throughput only\
 D. Add new features

 **Answer: A**

---

 ### Question 100

 **Maintainability → ?**

 A. Make the system easy to operate, understand, and change\
 B. Make one server infinitely powerful\
 C. Eliminate all data\
 D. Maximize latency

 **Answer: A**

---

 # High-Priority Questions to Memorize

 If you have limited study time, make sure you can answer these without hesitation:

 1. **What is a data-intensive application?**\
    → An application whose main challenges involve data volume, complexity, and rate of change.
2. **What does a database do?**\
    → Stores and retrieves application data.
3. **What does a cache do?**\
    → Speeds up access to frequently used data.
4. **What does a search index do?**\
    → Provides fast searching over large datasets.
5. **Stream vs. batch processing?**\
    → Stream = continuous data; Batch = accumulated data processed in groups.
6. **What is reliability?**\
    → The ability to continue operating correctly despite faults.
7. **Hardware fault example?**\
    → Hard-drive failure.
8. **Software fault example?**\
    → A bug introduced by a programmer.
9. **Human fault example?**\
    → An operator accidentally deleting production data.
10. **What is vertical scaling?**\
     → Making one machine more powerful.
11. **What is horizontal scaling?**\
     → Adding more machines.
12. **What is diagonal scaling?**\
     → Combining vertical and horizontal scaling.
13. **What is elasticity?**\
     → Dynamically adding/removing resources according to workload.
14. **When is elasticity especially useful?**\
     → When workload is highly variable or unpredictable.
15. **What is maintainability?**\
     → The ability to modify, correct, update, and improve a system.
16. **Three maintainability principles?**\
     → Operability, simplicity, evolvability.
17. **What is throughput?**\
     → Amount of work completed per unit time.
18. **What is latency?**\
     → Time required for an operation.
19. **What is response time?**\
     → Total time experienced by the user waiting for a response.
20. **Replication vs. backup?**\
     → Replication helps availability; backup helps recovery from data loss.
21. **What should you do when a system becomes slow?**\
     → Identify the bottleneck before choosing a solution.
22. **50 million records make searches slow while CPU remains stable. What is likely wrong?**\
     → Data management/search; consider a dedicated search index.
23. **Traffic increases from 10,000 to 100,000 users. What scaling approach?**\
     → Horizontal scalability.
24. **Traffic varies dramatically between peak and off-peak hours. What concept?**\
     → Elasticity.
25. **What is the overall architectural goal?**\
     → Balance **performance, reliability, scalability, and maintainability**.
