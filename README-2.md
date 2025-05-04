# Company Linkage Analytics on AWS Neptune  
_Entity Relationship Modeling & Visualization with Gremlin + Python_

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/downloads/release/python-3100/)
[![AWS Neptune](https://img.shields.io/badge/AWS-Neptune-green)](https://aws.amazon.com/neptune/)
[![License: GNU](https://img.shields.io/badge/License-GNU-blue.svg)](LICENSE)

---

## 📌 Project Summary

This project models **company-level contact linkages** using a layered graph stored in **AWS Neptune** and queried using **Gremlin**. It includes:

- Structured ingestion of communication linkages (e.g., phone, email, chat IDs)
- Construction of a **Company → ContactType → ContactValue → LinkedCompany** graph
- Visualization of relationships and communities using PyVis
- Simple Gremlin traversals for neighborhood analysis

These notebooks enable investigators and analysts to explore communication-based associations between entities at scale.

---

## 🧠 Core Notebooks

| Notebook | Purpose |
|----------|---------|
| `Company_Attribute_Linkages_AWS_Neptune.ipynb` | Loads linkage data, constructs Neptune graph using Gremlin |
| `Graph_Visualizations_AWS_Neptune.ipynb`      | Generates interactive network diagrams from query results |

---

## ⚙️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/<your-org>/<your-repo>.git
cd <your-repo>
```

### 2. Create Environment & Install Dependencies

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Set Up AWS Neptune Endpoint

Create a `.env` file with the following:

```env
AWS_REGION=us-east-1
NEPTUNE_ENDPOINT=db-xxxx.cluster-xyz.us-east-1.neptune.amazonaws.com
NEPTUNE_PORT=8182
```

---

## 🚀 Running the Project

### A. Build and Load the Graph

> Run the notebook: `Company_Attribute_Linkages_AWS_Neptune.ipynb`

This notebook:
- Reads structured company linkage data
- Constructs nodes: `:Company`, `:ContactType`, `:ContactValue`
- Creates edges:
  ```
  (:Company)-[:HAS_TYPE]->(:ContactType)
  (:ContactType)-[:HAS_VALUE]->(:ContactValue)
  (:ContactValue)-[:LINKED_WITH]->(:Company)
  ```

Edges can carry metadata like linkage count, source trust, or timestamps.

### B. Visualize Connections

> Run the notebook: `Graph_Visualizations_AWS_Neptune.ipynb`

Once the graph is constructed, you can explore its structure visually using **Neptune’s built-in Graph Explorer**:

- Go to your **Neptune Workbench instance**
- Click on the notebook (ensure its status is “Ready”)
- From the **Actions** menu, choose **“Open Graph Explorer”**

This will open an interactive UI that shows:
- Connected components
- High-degree nodes (companies sharing many contact points)
- Visual layout controls to reposition and inspect clusters

The Graph Explorer supports various layout algorithms, node labels, and filtering options. You can re-run the layout for clarity or focus on specific attributes like `name`, `value`, or `linkage type`.

---

## 🧪 Example Queries

- **k-hop neighbors:**
```gremlin
g.V().hasLabel('Company').has('name', 'XYZ Corp')
  .repeat(out().simplePath()).times(2).path()
```

- **Find all companies linked by a specific phone:**
```gremlin
g.V().hasLabel('ContactValue').has('value', '123-456-7890')
  .in('HAS_VALUE').in('HAS_TYPE').values('name')
```

---

## 🛠️ Requirements

```
gremlinpython>=3.7
networkx>=3.3
pyvis>=0.3.2
boto3>=1.34
pandas>=2.1
jupyterlab>=4.1
python-dotenv>=1.0
```

---

## 🧩 Outputs

- `graph.html`: standalone PyVis visual exported for review  
- Gremlin query results (tabular & subgraph)  
- Visual breakdowns by contact method (email/phone/chat)

---

## 📜 License

This project is licensed under the GNU License. See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- TraCCC, GMU, and MITRE for guidance on fentanyl trafficking network analysis  
- AWS Neptune samples and Gremlin community tools
