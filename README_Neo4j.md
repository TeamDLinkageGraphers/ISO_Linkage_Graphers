Neo4j Graph Implementation – Top Companies
What is Neo4j?
Neo4j is a high-performance, native graph database built to store and query complex, highly connected data. Unlike relational databases that use tables and joins, Neo4j uses nodes (entities), relationships (edges), and properties to represent and store data making it ideal for uncovering patterns, clusters, and connections.

Real-World Use Cases Include:
- Fraud Detection
- Supply Chain Analysis
- Recommendation Systems
- Network & IT Operations
- Knowledge Graphs in Enterprises
Why Use Neo4j for This Project?
Traditional SQL databases struggle when it comes to deeply connected data, requiring complex joins that are slow and difficult to manage. Our project investigates hidden linkages between companies—such as those sharing the same phone number or email—where such connections are inherently graph-like.

Benefits Neo4j Offers:
- Handles multi-level relationships effortlessly.
- Allows intuitive visualization of entity connections.
- Enables real-time querying across thousands of nodes and edges.
- Helps detect potential fraud patterns and high-risk clusters.

In this project, Neo4j helps us identify companies that are connected through common contact points offering insights traditional data models might miss.
Graph Model Explanation
Each Cypher query constructs the following node structure:
(:BaseConsolidation)-[:HAS_METHOD]->(:LinkageMethod)
(:LinkageMethod)-[:HAS_LINKAGE]->(:LinkageValue)
(:LinkageValue)-[:HAS_ASSOCIATED_COMPANY]->(:AssociatedCompany)

![Picture1](https://github.com/user-attachments/assets/14d72008-9bca-4846-9dcc-145f8bbabe0f)

Evidence Weightage
To measure the strength of each linkage, we use an evidence weightage system based on frequency.

How it's calculated:
- A linkage value (e.g., a phone number) that is shared by multiple associated companies is considered more significant.
- The frequency count (lv.frequency) reflects how many companies are linked through the same value.
- This helps prioritize stronger, more suspicious, or more influential linkages.

Each LinkageValue node includes:
- frequency: numeric score
- display: human-readable format (e.g., "xyz@email.com (5)")

Only the top 2 highest-frequency (most weighted) linkage values per method are included for clarity and focus.

What the Code Does
For each selected company:
1. Loads data from Company_Linkage_data.csv
2. Filters by BaseConsolidationName
3. Groups data by LinkageMethod and LinkageValue
4. Counts the number of associated companies (evidence weightage)
5. Builds the graph by creating nodes and relationships

![Picture2](https://github.com/user-attachments/assets/eea12f04-cd34-4647-8510-f055fcacaa1d)

![Picture3](https://github.com/user-attachments/assets/ce232115-0efd-4466-b00e-d1d7c1967514)

Top 10 Example Companies
These examples are used for demonstration. You can replicate the structure for additional companies as needed.
1. Shanghai Hao Zhun Biological Technology Co., Ltd
2. Amadis Chemical Company Limited
3. Conier Chem & Pharma Limited
4. Hebei Miaoyin Technology Co., Ltd
5. Shanghai Run Biotech Co., Ltd
6. Laidiou Biological Technology Co., Ltd
7. Henan Sunlake Enterprise Corporation
8. DC Chemicals
9. Hubei Enxing Biotechnology Co., Ltd
10. Career Henan Chemical Co


Repository Structure
neo4j-graph-implementation/
│
├── README.md
├──Company_Linkage_data.csv
└── company_linkage_queries.cypher
Usage Instructions
1. Place the Updated_Top_Companies_with_Associations.csv file in your Neo4j import directory.
2. Open Neo4j Desktop or connect via Neo4j Browser.
3. Run any .cypher script from the cypher query file to visualize the graph.
4. Each script builds nodes and links based on the specified company.


Built With
- Neo4j – Graph Database
- Cypher – Graph Query Language
- CSV Dataset – Structured linkage data
- Manual + Scripted Analysis – For top companies and relationship extraction



