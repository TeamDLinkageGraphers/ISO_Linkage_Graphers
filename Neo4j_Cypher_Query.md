## Introduction 
NEO4j 

The following steps are consistently followed in all Cypher queries provided for different base companies:

Data Loading & Cleaning:
The CSV file is loaded, and necessary fields (Base_Consolidation_Name, Associated_Company_Name, Linkage_Value, and Linkage_Method) are trimmed and extracted. Null or empty values are filtered out.

Company-Specific Filtering:
The query filters rows to isolate records related to one specific base company at a time.

Top Linkage Identification:
For that company:

The data is grouped by linkage method and value.
Frequency of usage is calculated.
The top two most frequently used linkages (e.g., phone numbers, emails) are selected.

Graph Node Creation:

A node is created for the base company.
Linkage method and linkage value nodes are created.
Relationships are established:
Base Company → Linkage Method → Linkage Value.

Associated Companies Connection:
All companies connected via the selected linkage values are added as nodes.
They are linked to the corresponding linkage value.

Final Result:
A subgraph is created that shows how each company connects to others through its top communication linkages.

This logic is repeated across all queries to generate consistent graph structures for each company.


1st company - Shanghai Hao Zhun Biological Technology Co., Ltd
```
//file with cypher query for top companies 
//Cyphe query for 1st company - Shanghai Hao Zhun Biological Technology Co., Ltd
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row # to load the data file 
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Shanghai Hao Zhun Biological Technology Co., Ltd'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''
// Step 1: Aggregate
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// Step 2: Create base and method structure
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// Step 3: Link associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// Final Return
RETURN DISTINCT base, lm, lv, ac;
```

2nd company - Amadis Chemical Company Limited
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Amadis Chemical Company Limited'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Aggregate linkage data
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create base and full linkage structure
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

//  Return all components
RETURN DISTINCT base, lm, lv, ac;
```

3rd company - Conier Chem & Pharma Limited
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Conier Chem & Pharma Limited'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Aggregate linkage data
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create base and method nodes
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

//  Final RETURN for graph visualization
RETURN DISTINCT base, lm, lv, ac;
```

4th company - Hebei Miaoyin Technology Co., Ltd
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Hebei Miaoyin Technology Co., Ltd'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Aggregate linkage data
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create base and method nodes
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

//  Final Return
RETURN DISTINCT base, lm, lv, ac;
```

5th company - Shanghai Run Biotech Co., Ltd
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Shanghai Run Biotech Co., Ltd'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Aggregate top linkage values
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create base company and linkage method nodes
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Connect associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// STEP 4: Return full graph structure
RETURN DISTINCT base, lm, lv, ac;
```

6th company - Laidiou Biological Technology Co., Ltd
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Laidiou Biological Technology Co., Ltd'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Aggregate top linkage values
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create Base and Method nodes
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link Associated Companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// FINAL: Return full graph
RETURN DISTINCT base, lm, lv, ac;
```

7th company - Henan Sunlake Enterprise Corporation
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Henan Sunlake Enterprise Corporation'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Aggregate linkage data
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create base and method nodes
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// FINAL: Return entire graph
RETURN DISTINCT base, lm, lv, ac;
```

8th company - Dc Chemicals
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Dc Chemicals'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''

// STEP 1: Group linkage info per value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create Base and Method nodes
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link each value to associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// FINAL: Return everything
RETURN base, lm, lv, ac;
```

9th company - Hubei Enxing Biotechnology Co., Ltd
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Hubei Enxing Biotechnology Co., Ltd'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''
WITH baseName, linkMethod, linkValue, assocName

// STEP 1: Group linkage info per value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create main Base + Method node
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords
UNWIND topRecords AS rec
WITH base, rec
WHERE rec.method IS NOT NULL AND rec.method <> ''
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 3: Link each value to associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// FINAL GRAPH RETURN
RETURN base, lm, lv, ac;
```

10th company - Career Henan Chemical Co
```
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH trim(row.`Base_Consolidation_Name`) AS baseName,
     trim(row.`Associated_Company_Name`) AS assocName,
     trim(row.`Linkage_Value`) AS linkValue,
     trim(row.`Linkage_Method`) AS linkMethod
WHERE baseName = 'Career Henan Chemical Co'
  AND assocName IS NOT NULL AND assocName <> ''
  AND linkValue IS NOT NULL AND linkValue <> ''
  AND linkMethod IS NOT NULL AND linkMethod <> ''
WITH baseName, linkMethod, linkValue, assocName

// STEP 1: Group linkage info per value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName,
     collect({
         value: linkValue,
         method: linkMethod,
         frequency: frequency,
         companies: assocCompanies
     })[0..2] AS topRecords

// STEP 2: Create base node
MERGE (base:BaseConsolidation {name: baseName})
WITH base, topRecords

// STEP 3: Linkage methods and values
UNWIND topRecords AS rec
MERGE (lm:LinkageMethod {type: rec.method})
MERGE (base)-[:HAS_METHOD]->(lm)
MERGE (lv:LinkageValue {value: rec.value, method: rec.method})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 4: Associated companies
UNWIND rec.companies AS compName
WITH base, lm, lv, trim(compName) AS companyName
WHERE companyName IS NOT NULL AND companyName <> ''
MERGE (ac:AssociatedCompany {name: companyName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// Final result: Show complete graph
RETURN DISTINCT base, lm, lv, ac;
```
