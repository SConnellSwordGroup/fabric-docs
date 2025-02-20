---
title: Lakehouse tutorial - Ingest data into the lakehouse
description: In this tutorial, you ingest additional dimensions and fact tables from the Wide World Importers (WWI) into the lakehouse.
ms.reviewer: sngun
ms.author: arali
author: ms-arali
ms.topic: tutorial
ms.custom:
  - build-2023
  - ignite-2023
ms.date: 5/23/2023
---

# Lakehouse tutorial: Ingest data into the lakehouse

In this tutorial, you ingest additional dimensional and fact tables from the Wide World Importers (WWI) into the lakehouse.

## Prerequisites

* [Create a lakehouse](tutorial-build-lakehouse.md)

## Ingest data

In this section, you use the **Copy data activity** of the Data Factory pipeline to ingest sample data from an Azure storage account to the **Files** section of the lakehouse you created earlier.

1. Select **Workspaces** in the left navigation pane, and then select your new workspace from the **Workspaces** menu. The items view of your workspace appears.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\create-data-pipeline.png" alt-text="Screenshot showing how to create a new data pipeline.":::

1. From the **+New** button in the workspace page, select **Data pipeline**.

1. In the **New pipeline** dialog box, specify the name as **IngestDataFromSourceToLakehouse** and select **Create**. A new data factory pipeline is created and opened.

1. On your newly created data factory pipeline, select **Add pipeline activity** to add an activity to the pipeline and select **Copy data**. This action adds copy data activity to the pipeline canvas.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\pipeline-copy-data.png" alt-text="Screenshot showing where to select Add pipeline activity and Copy data.":::

1. Select the newly added copy data activity from the canvas. Activity properties appear in a pane below the canvas (you may need to expand the pane upwards by dragging the top edge). Under the **General** tab in the properties pane, specify the name for the copy data activity **Data Copy to Lakehouse**.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\data-copy-to-lakehouse.png" alt-text="Screenshot showing where to add the copy activity name on the General tab.":::

1. Under **Source** tab of the selected copy data activity, select **External** as **Data store type** and then select **+ New** to create a new connection to data source.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\data-store-source-external.png" alt-text="screenshot showing where to select External and + New on the Source tab." lightbox="media\tutorial-lakehouse-data-ingestion\data-store-source-external.png":::

1. For this tutorial, all the sample data is available in a public container of Azure blob storage. You connect to this container to copy data from it. On the **New connection** wizard, select **Azure Blob Storage** and then select **Continue**.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\new-connection-azure-blob-storage.png" alt-text="Screenshot of the New connection wizard, showing where to select Azure Blob Storage.":::

1. On the next screen of the **New connection** wizard, enter the following details and select **Create** to create the connection to the data source.

   | Property | Value |
   |---|---|
   | Account name or URI | `https://azuresynapsestorage.blob.core.windows.net/sampledata` |
   |Connection | Create new connection |
   | Connection name | wwisampledata |
   | Authentication kind | Anonymous |

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\connection-settings-details.png" alt-text="Screenshot of the Connection settings screen, showing where to enter the details and select Create.":::

1. Once the new connection is created, return to the **Source** tab of the copy data activity, and the newly created connection is selected by default. Specify the following properties before moving to the destination settings.

   | Property | Value |
   |---|---|
   | Data store type | External |
   | Connection | wwisampledata |
   | File path type | File path |
   | File path | Container name (first text box): sampledata<br>Directory name (second text box): WideWorldImportersDW/parquet |
   | Recursively | Checked |
   | File Format | Binary |

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\source-tab-details.png" alt-text="Screenshot of the source tab showing where to enter the specific details.":::

1. Under the **Destination** tab of the selected copy data activity, specify the following properties:

   | Property | Value |
   |---|---|
   | Data store type | Workspace |
   | Workspace data store type | Lakehouse |
   | Lakehouse | wwilakehouse |
   | Root Folder | Files |
   | File path | Directory name (first text box): wwi-raw-data |
   | File Format | Binary |

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\destination-tab-details.png" alt-text="Screenshot of the Destination tab, showing where to enter specific details.":::

1. You have finished configuring the copy data activity. Select the **Save** button on the top ribbon (under **Home**) to save your changes, and select **Run** to execute your pipeline and its activity. You can also schedule pipelines to refresh data at defined intervals to meet your business requirements. For this tutorial, we run the pipeline only once by clicking on **Run** button.

   This action triggers data copy from the underlying data source to the specified lakehouse and might take up to a minute to complete. You can monitor the execution of the pipeline and its activity under the **Output** tab, which appears when you click anywhere on the canvas. Optionally, you can select the glasses icon, which appears when you hover over the name, to look at the details of the data transfer.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\save-run-output-tab.png" alt-text="Screenshot showing where to select Save and Run, and where to find the run details and glasses icon on the Output tab.":::

1. Once the data is copied, go to the items view of the workspace and select your new lakehouse (**wwilakehouse**) to launch the **Lakehouse explorer**.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\item-view-select-lakehouse.png" alt-text="Screenshot showing where to select the lakehouse to launch the Lakehouse explorer.":::

1. Validate that in the **Lakehouse explorer** view, a new folder **wwi-raw-data** has been created and data for all the tables have been copied there.

   :::image type="content" source="media\tutorial-lakehouse-data-ingestion\validate-destination-table.png" alt-text="Screenshot showing the source data is copied into the Lakehouse explorer.":::

## Next steps

Advance to the next article to learn about
> [!div class="nextstepaction"]
> [Prepare and transform data](tutorial-lakehouse-data-preparation.md)
