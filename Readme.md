# Augment Linkage Evidence & Exploration  
_Linked‑data analytics with AWS Neptune_

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License](LICENSE)
![Neptune](https://img.shields.io/badge/AWS-Neptune-green)

## 1. Project Overview
This repository contains code, notebooks, and cleaned datasets that power our graph‑based investigation into fentanyl trafficking networks.  
We ingest **∼10 k+ corporate‑linkage records** (emails, phones, chat handles, etc.), transform them into a **four‑layer graph model** (Company → Contact Type → Contact Value → Associated Company), and load the structure into **AWS Neptune** for scalable, real‑time analysis.

Key capabilities
- Automated CSV‑to‑Neptune loader with minimal config  
- Re‑usable Gremlin traversals for path search, centrality, and community detection  
- Jupyter Lab notebooks for ad‑hoc EDA and Neptune Graph Explorer launch  
- Lightweight visual dashboards (PyVis / Tableau workbook stubs)  

---

## 2. Repository Layout
```text
.
├── data/                   # ⇢ CSVs & sample extracts
│   ├── curated/            # 2024 curated linkage sets
│   └── raw/                # minimally‑structured TraCCC KV data
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_neptune_load.ipynb
│   └── 03_analysis_examples.ipynb
├── src/
│   ├── loaders/            # Python helpers for batch LOAD
│   └── gremlin/            # *.groovy traversal snippets
├── requirements.txt
└── README.md
