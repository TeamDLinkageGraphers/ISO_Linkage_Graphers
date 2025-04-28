# Graph Visualization with AWS Neptune

## Overview
This project models corporate linkages related to fentanyl trafficking using AWS Neptune. It includes data loading, graph creation, and visualization capabilities.

## Repository Structure
├──[View 1st Part of Graph Visuals](https://nbviewer.org/github/TeamDLinkageGraphers/ISO_Linkage_Graphers/blob/d92742aae5fc281ee4e8aed9923ec7693eb57474/1st%20part%20of%20the%20graph%20visuals.ipynb)


├── data/

├── requirements.txt

└── README.md


## Prerequisites
- **AWS Account** with Neptune cluster access
- **Neptune Instance**:
  - Endpoint: `db-neptune-1-instance-1.c8qttgkgfep5.us-east-1.neptune.amazonaws.com`
  - Port: `8182`
- **Python 3.8+** with pip

## Configuration

Update the Neptune endpoint in 1st_part_of_graph_visuals.ipynb:
neptune_endpoint = "https://db-neptune-1-instance-1.c8qttgkgfep5.us-east-1.neptune.amazonaws.com:8182/gremlin"
headers = {"Content-Type": "application/json"}

def send_gremlin(query):
    payload = {"gremlin": query}

## Setup Instructions
```bash
# Clone repository
git clone https://github.com/TeamDLinkageGraphers/ISO_Linkage_Graphers.git
cd [ISO_Linkage_Graphers]

# Install dependencies
pip install -r requirements.txt





