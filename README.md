# Work-with-data-in-a-Microsoft-Fabric-eventhouse

# Microsoft Fabric - Eventhouse Lab

This repository contains the steps and example queries used in a Microsoft Fabric lab to create and query a real-time Eventhouse using Kusto Query Language (KQL) and Transact-SQL (T-SQL).

## 🚀 Overview

In this lab, you will:
- Create a workspace with Fabric capacity.
- Set up an Eventhouse with sample data.
- Query the Eventhouse using both **KQL** and **T-SQL**.
- Summarize, filter, and sort data for analysis.
- Clean up resources when done.

## 🧪 Lab Instructions

### 1. Create a Workspace
- Go to [Microsoft Fabric](https://app.fabric.microsoft.com/home?experience=fabric).
- Select **Workspaces** > Create new workspace.
- Enable **Fabric capacity**.

### 2. Create an Eventhouse
- Go to **Workloads** > **Real-Time Intelligence**.
- Select **Explore Real-Time Intelligence Sample** to create a sample Eventhouse (`RTISample`).

### 3. Query with KQL

#### Retrieve Data
```kusto
Bikestream | take 100
```

#### Project Specific Columns
```kusto
Bikestream
| project Street, No_Bikes
| take 10
```

#### Rename Columns
```kusto
Bikestream 
| project Street, ["Number of Empty Docks"] = No_Empty_Docks
| take 10
```

#### Summarize Data
```kusto
Bikestream
| summarize ["Total Number of Bikes"] = sum(No_Bikes)
```

#### Group and Replace Nulls
```kusto
Bikestream
| summarize ["Total Number of Bikes"] = sum(No_Bikes) by Neighbourhood
| project Neighbourhood = case(isempty(Neighbourhood) or isnull(Neighbourhood), "Unidentified", Neighbourhood), ["Total Number of Bikes"]
```

#### Sort Data
```kusto
| sort by Neighbourhood asc
-- or
| order by Neighbourhood asc
```

#### Filter Data
```kusto
| where Neighbourhood == "Chelsea"
```

### 4. Query with Transact-SQL (T-SQL)

#### Retrieve Data
```sql
SELECT TOP 100 * FROM Bikestream;
SELECT TOP 10 Street, No_Bikes FROM Bikestream;
```

#### Rename Columns
```sql
SELECT TOP 10 Street, No_Empty_Docks AS [Number of Empty Docks] FROM Bikestream;
```

#### Summarize and Group Data
```sql
SELECT Neighbourhood, SUM(No_Bikes) AS [Total Number of Bikes]
FROM Bikestream
GROUP BY Neighbourhood;
```

#### Handle Nulls
```sql
SELECT CASE
         WHEN Neighbourhood IS NULL OR Neighbourhood = '' THEN 'Unidentified'
         ELSE Neighbourhood
       END AS Neighbourhood,
       SUM(No_Bikes) AS [Total Number of Bikes]
FROM Bikestream
GROUP BY CASE
           WHEN Neighbourhood IS NULL OR Neighbourhood = '' THEN 'Unidentified'
           ELSE Neighbourhood
         END;
```

#### Sort & Filter
```sql
ORDER BY Neighbourhood ASC;

HAVING Neighbourhood = 'Chelsea';
```

### 5. Clean Up Resources
- Go to your workspace.
- Select **Workspace settings** > **Remove this workspace**.

## 📁 License
Provided for educational use only.

© 2025 Microsoft. All rights reserved.
