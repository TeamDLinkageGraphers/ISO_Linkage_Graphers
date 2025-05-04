# Company Linkage Analytics on AWS Neptune  
_Entity Relationship Modeling & Visualization with Gremlin + Python_

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/downloads/release/python-3100/)
[![AWS Neptune](https://img.shields.io/badge/AWS-Neptune-green)](https://aws.amazon.com/neptune/)
[![License: GNU](https://img.shields.io/badge/License-GNU-blue.svg)](LICENSE)

---

## Project Summary

This project models **company-level contact linkages** using a layered graph stored in **AWS Neptune** and queried using **Gremlin**. It includes:

- Structured ingestion of communication linkages (e.g., phone, email, chat IDs)
- Construction of a **Company → ContactType → ContactValue → LinkedCompany** graph
- Interactive exploration of entity connections using Neptune’s built-in Graph Explorer
- Simple Gremlin traversals for neighborhood analysis

These notebooks enable investigators and analysts to explore communication-based associations between entities at scale.

---

## Core Notebooks

| Notebook | Purpose |
|----------|---------|
| `Company_Attribute_Linkages_AWS_Neptune.ipynb` | Loads linkage data, constructs Neptune graph using Gremlin |
| `Graph_Visualizations_AWS_Neptune.ipynb`      | Launches and navigates Neptune’s Graph Explorer |

---

## Setup Instructions

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

## Running the Project

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

Once the graph is loaded, explore it using **AWS Neptune’s built-in Graph Explorer**:

- In the Neptune Workbench, ensure your notebook instance is in “Ready” state
- From the **Actions** dropdown, select **Open Graph Explorer**
- Use the right-side panel to filter node labels (e.g., `Company`, `ContactValue`)
- Re-run the layout to improve the visual structure
- Click nodes to trace their neighbors, linkage values, and traversal paths

---

## Example Queries

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

## Requirements

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

## Outputs

- Graph structure visible in Neptune’s **Graph Explorer**
- Gremlin query results (e.g., linked company names, paths)
- Attribute filters and label-based layouts inside the Explorer view

---

## License

This project is licensed under the GNU License. See [LICENSE](LICENSE) for details.

---

## Acknowledgements

- TraCCC, GMU, and MITRE for guidance on fentanyl trafficking network analysis  
- AWS Neptune samples and Gremlin community tools
