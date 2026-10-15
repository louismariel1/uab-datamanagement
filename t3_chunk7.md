# Chunk 7 — Exam Compression: NoSQL Graph Data Models

## 1. Graph Database — Definition

A graph database stores data using nodes, edges (relationships), and properties instead of tables or documents.

- Nodes: Represent entities (e.g., people, products).
- Edges: Represent relationships between nodes.
- Properties: Store information about nodes or relationships.

Main advantage: Efficiently handles many-to-many relationships and frequently changing connections.

## 2. Two Common Graph Database Models

There are two main graph models:

1. RDF graphs — Focus on data integration and semantic meaning.
2. Property graphs — Focus on analytics, efficient querying, and graph traversal.

| Feature | RDF Graph | Property Graph |
| --- | --- | --- |
| Full name | Resource Description Framework | Property graph |
| Main purpose | Data integration and semantic data | Analytics and connected-data queries |
| Basic structure | Subject–Predicate–Object triples | Nodes, relationships, properties |
| Identifiers | URIs identify resources | Nodes use labels and key-value properties |
| Relationships | Predicates connect resources | Directed, named relationships with types and properties |
| Typical use | Web of Data, semantic applications, reasoning | Fast querying and traversal of connected data |

## 3. RDF Graphs — Must Memorize

RDF = Resource Description Framework

- A standard for exchanging data on the Web.
- Integrates and interconnects data from different sources.
- Suitable for semantic data and Web of Data applications.
- Supports explicit meaning and reasoning.

### Basic unit: Triple

$$
\boxed{(\text{Subject},\text{Predicate},\text{Object})}
$$

- Subject: The entity being described.
- Predicate: The relationship or property.
- Object: The related entity or value.

Example: `Alice — knows — Bob`

- Subject = Alice
- Predicate = knows
- Object = Bob

URI (Uniform Resource Identifier): Identifies resources uniquely, enabling consistent references across different data sources.

## 4. Property Graphs — Must Memorize

A property graph consists of nodes, relationships, and properties.

- Nodes: Represent entities; labels identify their roles.
- Node properties: Key-value pairs describing entities.
- Relationships: Directed, named connections between two nodes.
- Relationships have a type, direction, start node, and end node.
- Relationships can also contain properties.

Example:

`(Alice:Person) —[WORKS_AT {since: 2024}]→ (Company)`

- `Alice` and `Company` = nodes.
- `Person` = node label.
- `WORKS_AT` = relationship type.
- Arrow = relationship direction.
- `since: 2024` = relationship property.

Main advantage: Efficient storage of connected data, fast querying, and rapid graph traversal.

## 5. High-Yield Exam Questions

Active recall · 10 questions

 Reveal all answers

1.What are the three main components of a graph database?

2.What are the two common graph database models?

3.What is the main focus of RDF graphs?

4.What is the main focus of property graphs?

5.What is the basic unit of RDF?

6.What does RDF stand for?

7.What does URI stand for, and what is its purpose?

8.What information can a property-graph relationship have?

9.Why are graph databases useful for many-to-many relationships?

10.What determines the best NoSQL database choice?

## 6. Final 30-Second Revision

Memorize these facts:

- Graph DB = Nodes + Edges + Properties.
- RDF = Integration + Semantics + Reasoning.
- RDF triple = Subject + Predicate + Object.
- URI = Unique resource identification.
- Property graph = Labeled nodes + Directed relationships + Key-value properties.
- Property graphs = Analytics + Fast queries + Traversal.
- NoSQL models = Document, Key-Value, Column-Based, Graph.
- Best database = Depends on application requirements.

Exam tip: If asked to compare RDF and property graphs, the most important distinction is RDF emphasizes semantic interoperability and data integration, while property graphs emphasize practical querying and analysis of connected data.
