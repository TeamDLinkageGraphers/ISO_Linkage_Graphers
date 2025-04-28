//file with cypher query for top companies 
//Cyphe query for 1st company - Shanghai Hao Zhun Biological Technology Co., Ltd
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Shanghai Hao Zhun Biological Technology Co., Ltd'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

// STEP 1: Aggregate linkage data for each unique combination of base, method, and value.
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies}) AS linkRecords

// STEP 2: For each base and method, keep only the top 2 linkage records (most weighted)
WITH baseName, linkMethod, linkRecords[0..2] AS topRecords

// STEP 3: Create the BaseConsolidation node and the LinkageMethod node (head and hands)
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 4: Create LinkageValue node and set frequency + display
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 5: Attach associated companies to the linkage value node
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT base, lm, lv, ac;

//2nd company - Amadis Chemical Company Limited
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Amadis Chemical Company Limited'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

// STEP 1: Aggregate linkage data for each unique combination of base, method, and value.
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies}) AS linkRecords

// STEP 2: For each base and method, keep only the top 2 linkage records (most weighted)
WITH baseName, linkMethod, linkRecords[0..2] AS topRecords

// STEP 3: Create the BaseConsolidation node and the LinkageMethod node (head and hands)
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 4: Create LinkageValue node and set frequency + display
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 5: Attach associated companies to the linkage value node
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT base, lm, lv, ac;

//3rd company - Conier Chem & Pharma Limited
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Conier Chem & Pharma Limited'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

// STEP 1: Aggregate linkage data for each unique combination of base, method, and value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create the BaseConsolidation and LinkageMethod nodes
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH lm, topRecords, linkMethod

// STEP 3: Create LinkageValue nodes
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH lv, rec

// STEP 4: Link associated companies to the linkage value
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT lv, ac

//4th company - Hebei Miaoyin Technology Co., Ltd
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Hebei Miaoyin Technology Co., Ltd'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

// STEP 1: Aggregate linkage data for each unique combination of base, method, and value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create the BaseConsolidation and LinkageMethod nodes
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 3: Create LinkageValue nodes
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 4: Link associated companies to the linkage value
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT base, lm, lv, ac

//5th company - Shanghai Run Biotech Co., Ltd
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Shanghai Run Biotech Co., Ltd'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

// STEP 1: Aggregate linkage data for each unique combination of base, method, and value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create the BaseConsolidation and LinkageMethod nodes
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod  // ✅ This WITH was missing before UNWIND

// STEP 3: Create LinkageValue nodes
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec  // ✅ Needed for the next UNWIND

// STEP 4: Link associated companies to the linkage value
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT base, lm, lv, ac

//6th company - Laidiou Biological Technology Co., Ltd
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Laidiou Biological Technology Co., Ltd'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

// STEP 1: Aggregate linkage data for each unique combination of base, method, and value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create the BaseConsolidation and LinkageMethod nodes
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 3: Create LinkageValue nodes
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 4: Link associated companies to the linkage value
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT base, lm, lv, ac

//7th company - Henan Sunlake Enterprise Corporation
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Henan Sunlake Enterprise Corporation'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL

WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

WITH baseName, linkMethod, topRecords  // ✅ required to keep data before MERGE

MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

RETURN DISTINCT base, lm, lv, ac

//8th company - Dc Chemicals
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Dc Chemicals'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL
WITH baseName, linkMethod, linkValue, assocName

// STEP 1: Group linkage info per value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create main Base + Method node
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 3: Link top 2 values to method
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 4: Link each value to associated companies
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// FINAL GRAPH RETURN
RETURN base, lm, lv, ac

//9th company - Hubei Enxing Biotechnology Co., Ltd
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE baseName = 'Hubei Enxing Biotechnology Co., Ltd'
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL
WITH baseName, linkMethod, linkValue, assocName

// STEP 1: Group linkage info per value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create main Base + Method node
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 3: Link top 2 values to method
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 4: Link each value to associated companies
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// FINAL GRAPH RETURN
RETURN base, lm, lv, ac

//10th company - Career Henan Chemical Co
LOAD CSV WITH HEADERS FROM 'file:///Company_Linkage_data.csv' AS row
WITH row.`Base Consolidation Name` AS baseName,
     row.`Associated Name` AS assocName,
     row.`Linkage Value` AS linkValue,
     row.`Linkage Method` AS linkMethod
WHERE toLower(trim(baseName)) = toLower(trim('Career Henan Chemical Co'))
  AND assocName IS NOT NULL
  AND linkValue IS NOT NULL
  AND linkMethod IS NOT NULL
WITH baseName, linkMethod, linkValue, assocName

// STEP 1: Group linkage info per value
WITH baseName, linkMethod, linkValue,
     collect(DISTINCT assocName) AS assocCompanies,
     count(*) AS frequency
ORDER BY frequency DESC
WITH baseName, linkMethod,
     collect({value: linkValue, frequency: frequency, companies: assocCompanies})[0..2] AS topRecords

// STEP 2: Create main Base + Method node
MERGE (base:BaseConsolidation {name: baseName})
MERGE (lm:LinkageMethod {type: linkMethod})
MERGE (base)-[:HAS_METHOD]->(lm)
WITH base, lm, topRecords, linkMethod

// STEP 3: Link top 2 values to method
UNWIND topRecords AS rec
MERGE (lv:LinkageValue {value: rec.value, method: linkMethod})
SET lv.frequency = rec.frequency,
    lv.display = rec.value + " (" + toString(rec.frequency) + ")"
MERGE (lm)-[:HAS_LINKAGE]->(lv)
WITH base, lm, lv, rec

// STEP 4: Link each value to associated companies
UNWIND rec.companies AS compName
MERGE (ac:AssociatedCompany {name: compName})
MERGE (lv)-[:HAS_ASSOCIATED_COMPANY]->(ac)

// RETURN GRAPH
RETURN base, lm, lv, ac
