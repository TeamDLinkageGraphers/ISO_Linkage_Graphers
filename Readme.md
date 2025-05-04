# Augment Linkage Evidence & Exploration  
_Linked‑data analytics with AWS Neptune_

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/github/license/<ORG>/<REPO>)
![Neptune](https://img.shields.io/badge/AWS-Neptune-green)

## 1. Project Overview
This repository contains code, notebooks, and cleaned datasets that power our graph‑based investigation into fentanyl trafficking networks.  
We ingest **∼10 k+ corporate‑linkage records** (emails, phones, chat handles, etc.), transform them into a **four‑layer graph model** (Company → Contact Type → Contact Value → Associated Company), and load the structure into **AWS Neptune** for scalable, real‑time analysis :contentReference[oaicite:0]{index=0}:contentReference[oaicite:1]{index=1}.

Key capabilities
- Automated CSV‑to‑Neptune loader with minimal config  
- Re‑usable Gremlin traversals for path search, centrality, and community detection :contentReference[oaicite:2]{index=2}:contentReference[oaicite:3]{index=3}  
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
