#  Neptune Graph Ingestion Script For Key Value and 3NF Common Linkage Data) 

This script ingests company-attribute linkage data from an Excel file (`ISO_Main1.xlsx` for the Key Value) into an **Amazon Neptune** graph database using **Gremlin** queries.

** The main dataset used for the keyvalue and 3NF commom linkage data is Consolidated_TraCCC_KV_Master(In_Work7). **
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

Datasets used  for the 3NF:
```
ISO_Linkage1.xlsx
ISO_Company.xlsx

```

How It Works
# 🔗 3NF Company Linkage Ingestion Script (AWS Neptune)
Check the Key_Vlaue-3NF-2.ipynb file.

This script processes and ingests structured 3NF (Third Normal Form) data from Excel files into an **Amazon Neptune** graph database. It builds a detailed graph of companies, communication methods (emails, phones, chats), and payment methods to help identify hidden linkages and shared attributes among organizations.

---

##  Inut Files

- **ISO_Linkage1.xlsx**  
  Contains company-to-linkage relationships such as email, phone, or chat-based connections.  
  **Columns:**
  - `Company Name`
  - `Linkage Method` (Email, Phone, Chat, etc.)
  - `Linkage Value Type` (e.g., WeChat, WhatsApp for chat)
  - `Linkage Value`

- **ISO_Company.xlsx**  
  Contains metadata about companies and their payment preferences.  
  **Columns:**
  - `Company`
  - `Payment method`

---

##  Graph Schema

```text
(Company) 
  ├── has_email / has_phone / has_chat_{type} ──> (LinkageValue)
  ├── uses_payment_method ──> (PaymentMethod)
  ├── LinkedCompany (via shared linkage value) ──> (Company)

(LinkageMethod) ── has_value ──> (LinkageValue)
(LinkageValueType) ── has_value ──> (LinkageValue)
(LinkageMethod) ── has_type ──> (LinkageValueType)
(LinkageValue) ── used_by ──> (Company)

```

Key Functionalities
Node Creation
Company, LinkageMethod, LinkageValue, LinkageValueType, PaymentMethod nodes are created uniquely.

Duplicate nodes are avoided using fold().coalesce() logic.

🔄 Edge Creation
Adds multiple edge types such as:

has_email, has_phone, has_chat_wechat, etc. based on method

has_value, has_type, used_by, uses_payment_method

Dynamically constructs edge labels based on sanitized input.


1. Company-to-Company Linkages
If two or more companies share the same Linkage Value (e.g., same phone or email), they are connected using:

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

### Open Graph Explorer
From this we can see the visulization 
Here the Interface for reference.
![image](https://github.com/user-attachments/assets/bde623d0-a883-4178-89ac-3b768371410a)



