#  Neptune Graph Ingestion Script (ISO_Main1.xlsx) -- load the dataset

This script ingests company-attribute linkage data from an Excel file (`ISO_Main1.xlsx`) into an **Amazon Neptune** graph database using **Gremlin** queries.

##  Overview

The goal is to model complex corporate relationships in a graph format with the following structure:

- **Company** nodes linked to **AttributeValue** nodes (e.g., phone, email, chat ID)
- **AttributeValue** nodes linked to **AttributeType** nodes (e.g., 'Phone', 'Email')
- **Companies** sharing the same `AttributeValue` are linked to each other via `LINKED_COMPANY` edges

This helps uncover hidden relationships between companies based on shared contact identifiers.

---

## Input File

**`ISO_Main1.xlsx`**  
Expected columns:
- `Company_Name`
- `Attribute` (e.g., Phone, Email, Chat ID)
- `Attribute_Value` (e.g., specific email, phone number)

---

## Graph Model

```text
(Company) --[HAS_VALUE]--> (AttributeValue) --[HAS_TYPE]--> (AttributeType)

(Company) --[LINKED_COMPANY { via: Attribute_Value }]--> (Company)
```


How It Works
1. Preprocessing & Cleanup
Drops rows with missing data

Escapes special characters from string fields

2. Node Creation
Creates nodes for each:

Company

AttributeValue

AttributeType

3. Edge Creation
Company --HAS_VALUE--> AttributeValue

AttributeValue --HAS_TYPE--> AttributeType

If two or more companies share the same attribute value (e.g., same email), they are connected by:

Company --LINKED_COMPANY { via: value }--> Company

4. Gremlin POST Requests
Queries are submitted using HTTP POST to the Neptune Gremlin endpoint:

```
https://<your-neptune-cluster>:8182/gremlin
```
Configuration
In the script, update your Neptune endpoint:

```
neptune_endpoint = "https://<your-neptune-endpoint>:8182/gremlin"
```
Dependencies
Install required libraries:
```
pip install pandas requests tqdm openpyxl

```

After running the code your will get 
![image](https://github.com/user-attachments/assets/c2a66116-599e-4105-bc27-7e69007eadf1)


