# Curated Association Dataset Visulization on AWS Neptune  

# Graph-Based Visualization of Fentanyl Trafficking Networks using AWS Neptune
Dataset used  Curated_Companies Associations data.xlsx

This Jupyter Notebook provides an interactive, graph-powered exploration of curated company association data. Using **AWS Neptune** as the backend graph database and **Gremlin** queries, it visualizes critical network structures within the fentanyl trafficking supply chain.

---

## Visualizations Implemented

### 1. **High-Weight Company Graph**
- **Purpose**: Visualize only companies with high `Company_Evidence_Weight` (≥ 3000).
- **Outcome**: Displays a compact network of strong associations using filtered subgraphs.

### 2. **Threshold-Based Graph Explorer**
- **Purpose**: Allow dynamic exploration of the graph based on evidence score threshold.
- **Outcome**: Visual graphs generated from top-ranked companies using filtered CSV input.

---

## Data and Filters

- Input File: `Updated_Top_Companies_with_Evidence_Clean(in).csv`
- Filtering Criteria:
  - `Company_Evidence_Weight >= 3000`
  - Non-null `Company_Evidence_Count`

---

## Technologies Used

- **AWS Neptune (Gremlin Endpoint)** – for graph database and traversal
- **Pandas** – for data preprocessing
- **Requests** – for making HTTP POST calls to Neptune
- **tqdm** – for progress bar during node/edge creation

---

##  How to Use

### Setup Requirements

Install required Python packages:
```bash
pip install pandas tqdm requests
```

Configure Neptune Endpoint
Update this line in the notebook with your AWS Neptune Gremlin endpoint:

```
neptune_endpoint = "https://<your-cluster>.neptune.amazonaws.com:8182/gremlin"
```

Run the Notebook
Load the Jupyter notebook.

Execute all cells sequentially:

Clears previous graph (g.V().drop())

Filters high-evidence companies from CSV

Creates nodes and edges in AWS Neptune

Optionally visualizes the graph (through exported results)


### Reset Graph
To wipe the existing Neptune graph before reloading:



