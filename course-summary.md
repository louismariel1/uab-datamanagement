Absolutely. I’ve got both chunks now. The first chunk is mainly **course structure, syllabus, evaluation, and grading**, while the second introduces the **motivation for data management and its connection to AI**.

 ## SQL / Data Management — Lecture 0 Summary

 ### 1\. Course overview

 The course is **Data Management (2026–27)**, from the Computer Architecture & Operating Systems (CAOS) department.

 The course is organized around **four major blocks**:

 1. **Data Systems**
   - Fundamental concepts for managing large amounts of data.
2. **Data Models**
   - Relational databases
   - NoSQL databases
   - In-memory databases
3. **Large-data management and processing**
   - Apache Spark
   - Spark DataFrames
   - Spark MLlib
4. **Cloud data management**
   - Cloud database services
   - Data warehouses

 So, although the course is relevant to SQL, **SQL is only one part of a much broader data-management ecosystem**.

---

 # 2\. Why is Data Management important?

 The central motivation of the lecture is:

 > **AI depends fundamentally on data.**

 The lecture presents AI as depending on three major ingredients:

 **Computing power + Algorithms \+ Data → Value**

 The key point is that having powerful algorithms and computing resources is not enough. AI systems need large quantities of useful data from which they can learn patterns.

 The notes emphasize:

 - AI is fundamentally driven by data.
- AI learns patterns from large amounts of data.
- Valuable insights can come from both **structured and unstructured datasets**.

 Therefore, managing data efficiently becomes a fundamental requirement for modern AI and computing systems.

---

 # 3\. The four fundamental data-management questions

 A very important framework from the lecture is that data management must answer four questions.

 ### ① How do I represent and store data?

 Different technologies are appropriate for different types of data.

 The lecture mentions:

 - **SQL / relational databases**
- **NoSQL databases**
- **Graph databases**

 For example:

 - SQL → tables, rows, columns, relationships
- NoSQL → flexible/non-tabular data models
- Graph → entities and relationships represented as a graph

---

 ### ② How do I access data?

 Once data is stored, we need efficient ways of retrieving it.

 Important concepts include:

 - **Indexes**
- **Query optimization**

 An index can make finding particular records much faster, while query optimization concerns finding an efficient way to execute a query.

 This is particularly important when databases become very large.

---

 ### ③ How do I process data?

 Data doesn't just need to be stored and retrieved; it often needs to be transformed, analyzed, or used for computation.

 The lecture identifies:

 - **Batch processing**
- **Streaming processing**

 **Batch processing** means processing data in groups, typically periodically.

 **Streaming processing** means processing data continuously as it arrives.

 The choice depends on the application and how quickly results are required.

---

 ### ④ How do I scale data management?

 As the amount of data grows, one machine may no longer be sufficient.

 The lecture highlights:

 - **Cloud**
- **Parallel and distributed systems**

 The goal is to handle increasing amounts of data by distributing computation and storage across resources.

---

 # 4\. Where should data live?

 The lecture identifies several possibilities:

 - **Locally**
- **In the cloud**
- **In distributed systems**

 This is an important design question because data-management systems must consider not only _what_ data is stored but also _where_ it is stored.

 For large-scale applications, cloud and distributed architectures become especially important.

---

 # 5\. The central idea: AI is only as good as its data infrastructure

 One of the most important statements in the lecture is essentially:

 > **AI is only as good as the data that can be stored, accessed, processed, and scaled.**

 This connects the entire course together.

 Think of the chain as:

 **Data → Storage → Access → Processing → Scaling → AI/Value**

 If any stage is inadequate, the usefulness of the resulting AI/data application can suffer.

 For example:

 - Data cannot be **stored** efficiently → information is difficult to maintain.
- Data cannot be **accessed** efficiently → applications become slow.
- Data cannot be **processed** efficiently → analysis takes too long.
- Data cannot **scale** → the system fails when data volume grows.

---

 # 6\. Course technologies

 The course progressively moves from traditional databases toward large-scale data systems.

 A useful mental map is:

```
                 DATA MANAGEMENT
                       │
       ┌───────────────┼────────────────┐
       │               │                │
   Data Models     Data Systems     Processing
       │               │                │
   ┌───┼───┐       Relational       Apache Spark
   │   │   │       NoSQL             Spark DataFrames
  SQL NoSQL Graph  In-memory         Spark MLlib
                       │
                       ▼
                 Cloud Systems
                       │
              ┌────────┴────────┐
              │                 │
        Cloud Databases     Data Warehouses
```

 This explains why the course is broader than simply learning SQL syntax.

---

 # 7\. Course structure and practical work

 The course mixes:

 - **Theory classes**
- **Lab/practice sessions**

 Students are expected to bring their own computer to practical sessions.

 The labs cover technologies including:

 - Redis
- Spark
- Spark MLlib
- Cloud databases

 The preliminary lab deadlines are:

 | Lab | Deadline | Grades |
| --- | --- | --- |
| Redis | 25/10/2026 | 9/11/2026 |
| Spark | 22/11/2026 | 7/12/2026 |
| Spark MLlib | 6/12/2026 | 21/12/2026 |
| Cloud DB | 20/12/2026 | 11/1/2027 |

---

 # 8\. Assessment

 The final grade has two components:

 | Component | Weight |
| --- | --- |
| Theory & Problems | **60%** |
| Practice/Labs | **40%** |

The overall weighted grade is therefore:

 $$
Final = 0.6(Theory) + 0.4(Practice)
$$

 However, there are **minimum requirements**.

 ### Theory

 Theory is calculated as:

 $$
Theory = \frac{Exam_1 + Exam_2}{2}
$$

 Each exam represents **30% of the final grade**.

 Important conditions:

 - You need at least **3.5 on Exam 1** to take Exam 2.
- If Exam 1 is below 3.5, a recovery exam is scheduled.
- The average of Exam 1 and Exam 2 must be **at least 5**.
- Otherwise, the theory component must be recovered.

 ### Practice

 Practice is:

 $$
Practice =
\frac{PLAB_1+PLAB_2+PLAB_3+PLAB_4}{4}
$$

 Important:

 - Labs are performed **in groups of 3**.
- Attendance is mandatory.
- The practice component **cannot be recovered**.
- You need a practice average of **at least 5**.

 ### Final-grade restriction

 The lecture gives this rule:

```
if Theory >= 5 AND Practice >= 5:
    Final = 0.6*Theory + 0.4*Practice
else:
    Final = min(0.6*Theory + 0.4*Practice, 4.5)
```

 This is **very important for exams**.

 It means that even if your weighted average would otherwise be above 5, failing either the Theory or Practice minimum can cap your final grade at **4.5**.

---

 # 9\. Recovery exam

 The recovery/re-assessment exam:

 - Is individual.
- Is only available to students who failed the **Theory** component.
- Is **not** intended to improve an already-passing theory grade.
- Covers the **entire course**.
- Has a maximum grade of **7/10**.
- Takes place during the resit week.

 The preliminary date given is:

 **27 January 2027, 09:00–11:00.**

---

 # 10\. Important dates

 ### Theory

 - **Exam 1:** 27 October 2026, 09:00–11:00
- **Exam 2:** 15 January 2027, 09:00–11:00
- **Re-assessment:** 27 January 2027, 09:00–11:00

 These dates are explicitly marked **preliminary**, so they may be subject to change.

---

 # 11\. The big picture you should remember

 For studying, I would reduce the entire introductory lecture to this conceptual framework:

 ### Data management asks:

 **1\. How do we store data?**

 → SQL, NoSQL, Graph, In-memory databases

 **2\. How do we access data efficiently?**

 → Indexes, Query Optimization

 **3\. How do we process data?**

 → Batch, Streaming, Spark

 **4\. How do we scale?**

 → Cloud, Parallel & Distributed Systems

 And all of this supports modern data-driven applications, particularly **AI**:

 $$
\boxed{
\text{Computing Power + Algorithms + Data}
\rightarrow
\text{AI}
\rightarrow
\text{Value}
}
$$

 The fundamental message is:

 $$
\boxed{
\text{Good AI requires data that can be stored, accessed, processed and scaled.}
}
$$

 ### What you should focus on for a SQL course

 The administrative information is useful, but conceptually the most important material from this lecture is the **data-management roadmap**:

 **SQL → databases → data access → query optimization → large-scale processing → distributed systems → cloud data systems → AI/data applications.**

 This lecture is essentially setting up **why databases and SQL matter before the course starts teaching the actual database/query concepts.**
