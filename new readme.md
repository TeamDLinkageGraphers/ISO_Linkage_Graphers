# Graph Visualization with AWS Neptune

## Overview
This project models corporate linkages related to fentanyl trafficking using AWS Neptune. It includes data loading, graph creation, and visualization capabilities.

## Repository Structure
├── 1st_part_of_graph_visuals.ipynb

├── data/

├── requirements.txt

└── README.md

## Prerequisites
- AWS account with Neptune access
- Neptune endpoint: `db-neptune-1-instance-1.c8qttgkgfep5.us-east-1.neptune.amazonaws.com:8182`
- Python 3.8+

## Setup
```bash
git clone https://github.com/TeamDLinkageGraphers/ISO_Linkage_Graphers.git
cd your-repo
pip install -r requirements.txt


## Setup
Update Neptune endpoint in notebook:
neptune_endpoint = "https://db-neptune-1-instance-1.c8qttgkgfep5.us-east-1.neptune.amazonaws.com:8182/gremlin"

## Running
Start Jupyter:
```bash
jupyter notebook
