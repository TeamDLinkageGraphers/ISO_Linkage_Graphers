## 📌 Augment Linkage: Evidence and Exploration

Welcome to the GitHub repository for our DAEN 690 Capstone Project: Augment Linkage: Evidence and Exploration.
This project investigates fentanyl trafficking networks by uncovering complex corporate linkages using interactive dashboards, graph databases, and curated data modeling.


### Project Overview

We explore the hidden communication and payment-based connections among companies using:

Shared identifiers (email, phone, chat)

Multiple normalized datasets (KV and 3NF)

Multi-platform visualizations via Tableau, Neo4j, and AWS Neptune


### Project Structure


| Branch                       | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| `tableau-Interactive-graphs` | Tree graph dashboard using Tableau Tree Extension      |
| `Neo4j-Graph-Implementation` | Native graph database visualization with Cypher        |
| `AWS-Neptune`                | Cloud-based graph ingestion and querying using Gremlin |


#### 1. Tableau Interactive Dashboard
This dashboard was created using data_linkages.csv and Tree Extensions.

Tree Graph of Base Companies and Associated Companies

Colored edges by linkage method (Blue = Email, Orange = Phone)

Filters: Company selector, linkage value filter

Tooltip with company and contact details

Packaged workbook: DAEN 690_tableau Final 1.twbx


Built with: Tableau Desktop 2021.4+, Tree Extension Gallery
Data source: Associate_Reset_VRelease.xlsx



#### 2. Neo4j Graph Modeling
Neo4j was used for relationship modeling and graph querying via Cypher.

Graph Schema:
(BaseConsolidation)-[:HAS_METHOD]->(LinkageMethod)
(LinkageMethod)-[:HAS_LINKAGE]->(LinkageValue)
(LinkageValue)-[:HAS_ASSOCIATED_COMPANY]->(AssociatedCompany)


Identified top 2 linkage values per company based on frequency

Visualized top 10 companies (Shanghai Hao Zhun, Amadis Chemical, etc.)

Frequency-based evidence weighting added for suspicious linkages

Input: Company_Linkage_data.csv
Output: Real-time graph via Neo4j Browser
Files: company_linkage_queries.cypher


#### 3. AWS Neptune Visualization
We used two graph ingestion pipelines on Neptune:

A. Curated Association Data (KV)
Based on Updated_Top_Companies_with_Evidence_Clean(in).csv

Graphs filtered by evidence score (≥ 3000)

Nodes: Company, AttributeValue, AttributeType

B. 3NF Linkage Model
Based on ISO_Linkage1.xlsx, ISO_Company.xlsx

Graphs created using shared linkage value model (chat, phone, email)

Inferred company-to-company edges via shared identifiers

Visualized via Gremlin + Jupyter Notebook


### How to Use

| Platform        | Instructions                                                   |
| --------------- | -------------------------------------------------------------- |
| **Tableau**     | Load `.twbx` file, enable Tree Extension, use filter dropdowns |
| **Neo4j**       | Open Neo4j Browser, run `.cypher` queries, observe the linkage |
| **AWS Neptune** | Update Gremlin endpoint in notebook, run ingestion cells       |


### Datasets Used

Associate_Reset_VRelease.xlsx

Company_Linkage_data.csv

Updated_Top_Companies_with_Evidence_Clean(in).csv

ISO_Linkage1.xlsx, ISO_Company.xlsx, ISO_Main1.xlsx

### Sample Visualizations

#### Neo4j
![image](https://github.com/user-attachments/assets/3f8261f9-4253-444a-92ca-aac90b63813f)


#### Tableau
![image](https://github.com/user-attachments/assets/90e8b6ae-d031-4829-8ca6-83101be9696e)

#### AWS Neptune Curated Associations
![image](https://github.com/user-attachments/assets/79fa97b0-62e3-4ebf-971e-a7c1d2b50ebf)

#### AWS 

#### Key_Value Formated Data
![image](https://github.com/user-attachments/assets/ea3bacdd-5591-49d9-b70e-4fedb3893197)

#### Common Linkage Format
![image](https://github.com/user-attachments/assets/e662f807-3633-42d2-92ed-9711f8a8fdf4)



### Contributors
Sai Surya Gadiraju

Sai Charan Somineni

Pooja Manjunatha Swamy

Jerry Vishal Joseph

Shreya Reddy Jukanna

Akhila Kudipudi

Sai Sameer Sri Vastav Pillutla






