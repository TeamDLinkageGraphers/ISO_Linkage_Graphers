Neo4j Graph Implementation – Example: Top 10 Companies
This repository demonstrates how Neo4j graph databases can be used to model and analyze company linkages using structured data. The Cypher queries provided here illustrate how we extract relationships between base companies and their associated entities through shared linkage methods and values.

While the dataset contains many companies, we showcase only the top 10 companies as examples. The graph logic is scalable and can be applied across the full dataset.

Objective
Our goal is to reveal hidden connections between companies based on shared phone numbers, email addresses, and other linkage values. This helps in identifying patterns, risks, or potential fraudulent activity in business networks.

The graph visually represents:
- Companies as primary nodes
- Linkage methods like "Phone" or "Email" as intermediate connectors
- Linkage values as evidence points
- Associated companies as the endpoints

Graph Model Overview
Each Cypher query constructs the following node structure:
(:BaseConsolidation)-[:HAS_METHOD]->(:LinkageMethod)
(:LinkageMethod)-[:HAS_LINKAGE]->(:LinkageValue)
(:LinkageValue)-[:HAS_ASSOCIATED_COMPANY]->(:AssociatedCompany)

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
1. Loads data from Updated_Top_Companies_with_Associations.csv
2. Filters by BaseConsolidationName
3. Groups data by LinkageMethod and LinkageValue
4. Counts the number of associated companies (evidence weightage)
5. Builds the graph by creating nodes and relationships

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
├── Updated_Top_Companies_with_Associations.csv
└── cypher/
   ├── company_1_Shanghai_Hao_Zhun.cypher
   ├── company_2_Amadis_Chemical.cypher
   ├── ...
   └── company_10_Career_Henan_Chemical.cypher

Usage Instructions
1. Place the Updated_Top_Companies_with_Associations.csv file in your Neo4j import directory.
2. Open Neo4j Desktop or connect via Neo4j Browser.
3. Run any .cypher script from the cypher/ folder to visualize the graph.
4. Each script builds nodes and links based on the specified company.

Scaling to All Companies
The method demonstrated for 10 companies can be scaled across the entire dataset. For automation:
- Use APOC procedures to loop through unique base company names.
- Create parameterized Cypher scripts that dynamically insert base names.

Built With
- Neo4j – Graph Database
- Cypher – Graph Query Language
- CSV Dataset – Structured linkage data
- Manual + Scripted Analysis – For top companies and relationship extraction