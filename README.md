# Augment Linkage: Evidence and Exploration

Welcome to the GitHub repository for our DAEN 690 Capstone Project: **Augment Linkage: Evidence and Exploration**.  
This project focuses on uncovering complex corporate networks involved in fentanyl trafficking using interactive graph-based visualizations and scalable cloud architectures.

## Project Structure

This repository is structured into multiple branches, each targeting a specific technology stack and visualization goal:

| Branch | Description | Status |
|--------|-------------|--------|
| [`tableau-Interactive-graphs`](../../tree/tableau-Interactive-graphs) | Implements dynamic dashboards and filters in Tableau to visualize company linkage networks. | ✅ Active |
| [`Neo4j-Graph-Implementation`](../../tree/Neo4j-Graph-Implementation) | Uses Neo4j to model relationships between base and associated companies using Cypher queries. | ✅ Active |
| [`AWS-Neptune`](../../tree/AWS-Neptune) | Contains processed, cleaned, and curated datasets formatted for graph loading in both Neo4j and Neptune. | ✅ Active 

---

## Overview

Our project aims to uncover hidden corporate linkages and communication channels using multiple data sources, including emails, phone numbers, and chat handles. We use both **local graph modeling** (Neo4j) and **cloud-native graph databases** (AWS Neptune), with visual insights powered by **Tableau**.

---

## 🔍 Explore Sub-Branches

Each sub-branch can be accessed directly for in-depth analysis and code:

- 🔗 [Tableau Dashboards](../../tree/tableau-Interactive-graphs)
- 🔗 [Neo4j Graph Implementation](../../tree/Neo4j-Graph-Implementation)
- 🔗 [AWS Neptune](../../tree/AWS-Neptune)


---

## 📊 Visual Results

Each submodule outputs visual graphs or dashboards such as:

- Weighted node graphs with linkage types
- Top 25 base company hubs
- Interactive exploration of communication trails
- Role-based filtering on Tableau
- Gremlin-powered deep dives into associated entities

---

## 📁 Datasets Used

We process and analyze:
- TraCCC’s 3NF and KV data
- Identity linkage and association datasets (Reset versions)
- Manually cleaned Excel sheets for Spring 2025 analysis

See the [`AWS Neptune`](../../tree/AWS-Neptune) branch for more.

---

## 🚀 How to Run

Each branch contains its own `README.md` with environment setup instructions. Start from:
- `Neo4j`: Local deployment + Cypher
- `Neptune`: Jupyter Workbench via AWS console + Gremiln
- `Tableau`: Load `.twbx` dashboard using cleaned datasets

---

## Contributors

- Sai Surya Gadiraju  
- Sai Charan Somineni  
- Pooja Manjunatha Swamy  
- Jerry Vishal Joseph  
- Shreya Reddy Jukanna  
- Akhila Kudipudi  
- Sai Sameer Sri Vastav Pillutla

---

## 📜 License

MIT License. See `LICENSE` file for more details.
