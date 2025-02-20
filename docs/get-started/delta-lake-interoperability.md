---
title: Delta Lake table format interoperability
description: Delta Lake table format interoperability reference for Microsoft Fabric
ms.reviewer: snehagunda
ms.author: dacoelho
author: DaniBunny
ms.topic: conceptual
ms.custom:
  - build-2023
  - ignite-2023
ms.date: 11/9/2023
ms.search.form: delta lake interoperability
---

# Delta Lake table format interoperability

In Microsoft Fabric, the Delta Lake table format is the standard for analytics. [Delta Lake](https://docs.delta.io/latest/delta-intro.html) is an open-source storage layer that brings ACID (Atomicity, Consistency, Isolation, Durability) transactions to big data and analytics workloads. 

All Microsoft Fabric experiences generate and consume Delta Lake tables, driving interoperability and a unified product experience. Delta Lake tables produced by one compute-engine, such as Synapse Data warehouse or Synapse Spark, can be consumed by any other engine, such as Power BI. When you ingest data into Fabric, it stores as Delta Tables by default. It’s easy to integrate external data containing Delta Lake tables using OneLake’s shortcuts capability.

## Delta Lake features and Fabric experiences

To achieve interoperability, all the Fabric experiences align on the Delta Lake features and Fabric capabilities. Some experiences can only write to Delta Lake tables, while others can read from it.

* **Writers:** Data Warehouse, Event streams, and exported Power BI semantic models into OneLake.
* **Readers:** SQL analytics endpoint, and PowerBI direct lake semantic models.
* **Writers and readers:** Fabric spark runtime, dataflows, data pipelines, and KQL DB.

The following matrix shows key Delta Lake features and its supportability on each Fabric capability.

|Fabric capability|Name-based column mappings|Deletion vectors|V-order writing|Table optimization and maintenance|Write partitions|Read partitions|Delta reader/writer version and default table features|
|---------|---------|---------|---------|---------|---------|---------|---------|
|Data Warehouse Delta Lake export|No|Yes|Yes|Yes|No|Yes|Reader: 3<br/>Writer: 7<br/>Deletion Vectors|
SQL analytics endpoint|No|Yes|N/A (not applicable)|N/A (not applicable)|N/A (not applicable)|Yes|N/A (not applicable)|
Fabric Spark runtime 1.2|Yes|Yes|Yes|Yes|Yes|Yes|Reader: 1<br/>Writer: 2|
Fabric Spark runtime 1.1|Yes|No|Yes|Yes|Yes|Yes|Reader: 1<br/>Writer: 2|
Dataflows|Yes|Yes|Yes|No|Yes|Yes|Reader: 1<br/>Writer: 2<br/>|
Data pipelines|No|No|Yes|No|Yes, overwrite only|Yes|Reader: 1<br/>Writer: 2|
Power BI direct lake semantic models|Yes|Yes|N/A (not applicable)|N/A (not applicable)|N/A (not applicable)|Yes|N/A (not applicable)|
Export Power BI semantic models into OneLake|Yes|N/A (not applicable)|Yes|No|Yes|N/A (not applicable)|Reader: 2<br/>Writer: 5|
KQL DB|No|No|No|No|Yes|Yes|Reader: 1<br/>Writer: 1|
EventStreams|No|No|No|No|Yes|N/A (not applicable)|Reader: 1<br/>Writer: 2|

> [!NOTE]
>
> * Name-based column mappings are not written by default in Fabric. The default Fabric experience will generate tables that are compatible across the service. Delta lake, produced by third-party services may have incompatible table features.
> * Some Fabric experiences do not have inherited table optimization and maintenance capabilities such as bin-compaction, V-order, and clean up of old unreferenced files. Tables ingested using those experiences should use techniques described on [Use table maintenance feature to manage delta tables in Fabric](../data-engineering/lakehouse-table-maintenance.md) to keep Delta Lake tables optimal for analytics. 

## Current limitations

The following Delta Lake features aren't currently supported in Microsoft Fabric.

* Column mapping using IDs.
* Delta Lake 3.x Uniform.
* Delta Lake 3.x Liquid clustering.
* TIMESTAMP_NTZ data type.
* Identity columns writing (proprietary Databricks feature).
* Delta Live Tables (proprietary Databricks feature).

## Next steps

* [What is Delta Lake?](/azure/synapse-analytics/spark/apache-spark-what-is-delta-lake)
* Learn more about [Delta Lake tables](../data-engineering/lakehouse-and-delta-tables.md) in Fabric Lakehouse and Synapse Spark.
* [Learn about Direct Lake in Power BI and Microsoft Fabric](/power-bi/enterprise/directlake-overview).
* Learn more about [querying tables from the Warehouse through its published Delta Lake Logs](../data-warehouse/query-delta-lake-logs.md).
