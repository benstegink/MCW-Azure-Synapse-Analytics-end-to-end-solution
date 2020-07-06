![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Azure Synapse Analytics end-to-end solution
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
May 2020
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents** 

<!-- TOC -->
- [Azure Synapse Analytics end-to-end solution hands-on lab step-by-step](#azure-synapse-analytics-end-to-end-solution-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
  - [Resource naming throughout this lab](#resource-naming-throughout-this-lab)
  - [Exercise 1: Accessing the Azure Synapse Analytics workspace](#exercise-1-accessing-the-azure-synapse-analytics-workspace)
    - [Task 1: Launching Synapse Studio](#task-1-launching-synapse-studio)
  - [Exercise 2: Create and populate the supporting tables in the SQL Pool](#exercise-2-create-and-populate-the-supporting-tables-in-the-sql-pool)
    - [Task 1: Create the sale table](#task-1-create-the-sale-table)
    - [Task 2: Populate the sale table](#task-2-populate-the-sale-table)
    - [Task 3: Create the customer information table](#task-3-create-the-customer-information-table)
    - [Task 4: Populate the customer information table](#task-4-populate-the-customer-information-table)
    - [Task 5: Create the campaign analytics table](#task-5-create-the-campaign-analytics-table)
    - [Task 6: Populate the campaign analytics table](#task-6-populate-the-campaign-analytics-table)
    - [Task 7: Populate the product table](#task-7-populate-the-product-table)
  - [Exercise 3: Exploring raw parquet](#exercise-3-exploring-raw-parquet)
    - [Task 1: Query sales Parquet data with Synapse SQL Serverless](#task-1-query-sales-parquet-data-with-synapse-sql-serverless)
    - [Task 2: Query sales Parquet data with Azure Synapse Spark](#task-2-query-sales-parquet-data-with-azure-synapse-spark)
  - [Exercise 4: Exploring raw text based data with Azure Synapse SQL Serverless](#exercise-4-exploring-raw-text-based-data-with-azure-synapse-sql-serverless)
    - [Task 1: Query CSV data](#task-1-query-csv-data)
    - [Task 2: Query JSON data](#task-2-query-json-data)
  - [Exercise 5: Synapse Pipelines & Cognitive Search](#exercise-5-synapse-pipelines--cognitive-search)
    - [Task 1: Create the necessary storage account](#task-1-create-the-necessary-storage-account)
    - [Task 2: Create and train Azure Forms Recognizer model and Cognitive search](#task-2-create-and-train-azure-forms-recognizer-model-and-cognitive-search)
    - [Task 3: Configure a skillset with form recognizer](#task-3-configure-a-skillset-with-form-recognizer)
    - [Task 4: Create the Synapse pipeline](#task-4-create-the-synapse-pipeline)
  - [Exercise 6: Security](#exercise-6-security)
    - [Task 1: Column level security](#task-1-column-level-security)
    - [Task 2: Row level security](#task-2-row-level-security)
    - [Task 3: Dynamic data masking](#task-3-dynamic-data-masking)
  - [Exercise 7: Machine Learning](#exercise-7-machine-learning)
    - [Task 1: Training models](#task-1-training-models)
    - [Task 2: Registering the models with Azure Synapse Analytics](#task-2-registering-the-models-with-azure-synapse-analytics)
    - [Task 3: Making predictions with the registered models](#task-3-making-predictions-with-the-registered-models)
  - [Exercise 8: Monitoring](#exercise-8-monitoring)
    - [Task 1: Workload Importance](#task-1-workload-importance)
    - [Task 2: Workload Isolation](#task-2-workload-isolation)
    - [Task 3: Monitoring with Dynamic Management Views](#task-3-monitoring-with-dynamic-management-views)
    - [Task 4: Orchestration Monitoring with the Monitor Hub](#task-4-orchestration-monitoring-with-the-monitor-hub)
    - [Task 5: Monitoring SQL Requests with the Monitor Hub](#task-5-monitoring-sql-requests-with-the-monitor-hub)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete the resource group](#task-1-delete-the-resource-group)
<!-- /TOC -->

# Azure Synapse Analytics end-to-end solution hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on-lab, you will build an end-to-end data analytics with machine learning solution using Azure Synapse Analytics. The information will be presented in the context of a retail scenario. We will be heavily leveraging Azure Synapse Studio, a tool that conveniently unifies the most common data operations from ingestion, transformation, querying, and visualization.

## Overview

In this lab various features of Azure Synapse Analytics will be explored. Azure Synapse Analytics Studio is a single tool that every team member can use collaboratively. Synapse Studio will be the only tool used throughout this lab through data ingestion, cleaning, and transforming raw files to using Notebooks to train, register, and consume a Machine learning model. The lab will also provide hands-on-experience monitoring and prioritizing data related workloads.

## Solution architecture

![Architecture diagram explained in the next paragraph.](media/archdiagram.png)

This lab explores the cold data scenario of ingesting various types of raw data files. These files can exist anywhere. The file types used in this lab are CSV, parquet, and JSON. This data will be ingested into Synapse Analytics via Pipelines. From there, the data can be transformed and enriched using various tools such as data flows, Synapse Spark, and Synapse SQL (both provisioned and serverless). Once processed, data can be queried using Synapse SQL tooling. Azure Synapse Studio also provides the ability to author notebooks to further process data, create datasets, train, and create machine learning models. These models can then be stored in a storage account or even in a SQL table. These models can then be consumed via various methods, including T-SQL. The foundational component supporting all aspects of Azure Synapse Analytics is the ADLS Gen 2 Data Lake.

## Requirements

1. Microsoft Azure subscription

2. Azure Synapse Workspace / Studio

## Before the hands-on lab

Refer to the Before the hands-on lab setup guide manual before continuing to the lab exercises.

## Resource naming throughout this lab

For the remainder of this lab, the following terms will be used for various ASA (Azure Synapse Analytics) related resources (make sure you replace them with actual names and values from your environment):

| Azure Synapse Analytics Resource  | To be referred to                                                                  |
|-----------------------------------|------------------------------------------------------------------------------------|
| Azure Subscription                | `WorkspaceSubscription`                                                            |
| Azure Region                      | `WorkspaceRegion`                                                                  |
| Workspace resource group          | `WorkspaceResourceGroup`                                                           |
| Workspace / workspace name        | `asaworkspace{suffix}`                                                             |
| Primary Storage Account           | `asadatalake{suffix}`                                                              |
| Default file system container     | `DefaultFileSystem`                                                                |
| SQL Pool                          | `SqlPool01`                                                                        |
| SQL Serverless Endpoint           | `SqlServerless01`                                                                  |
| Azure Key Vault                   | `asakeyvault{suffix}`                                                              |

## Exercise 1: Accessing the Azure Synapse Analytics workspace

All exercises in this lab utilize the workspace Synapse Studio user interface. This exercise will outline the steps to launch Synapse Studio. Unless otherwise specified, all instruction including menu navigation will occur in Synapse Studio.

### Task 1: Launching Synapse Studio

1. Log into the [Azure Portal](https://portal.azure.com).

2. Expand the left menu, and select the **Resource groups** item.
  
    ![The Azure Portal left menu is expanded with the Resource groups item highlighted.](media/azureportal_leftmenu_resourcegroups.png)

3. From the list of resource groups, select `WorkspaceResourceGroup`.
  
4. From the list of resources, select the **Synapse Workspace** resource, `asaworkspace{suffix}`.
  
    ![In the resource list, the Synapse Workspace item is selected.](media/resourcelist_synapseworkspace.png)

5. On the **Overview** tab of the Synapse Workspace page, select the **Launch Synapse Studio** item from the top toolbar. Alternatively you can select the Workspace web URL link.

    ![On the Synapse workspace resource screen, the Overview pane is shown with the Launch Synapse Studio button highlighted in the top toolbar. The Workspace web URL value is also highlighted.](media/workspaceresource_launchsynapsestudio.png)

## Exercise 2: Create and populate the supporting tables in the SQL Pool

**Duration**: 45 minutes

The first step in querying meaningful data is to create tables to house the data. In this case, we will create four different tables: SaleSmall, CustomerInfo, CampaignAnalytics, and Sales. When designing tables in Azure Synapse Analytics, we need to take into account the expected amount of data in each table, as well as how each table will be used. Utilize the following guidance when designing your tables to ensure the best experience and performance.

Table design performance considerations

| Table Indexing | Recommended use |
|--------------|-------------|
| Clustered Columnstore | recommended for tables with greater than 100 million rows, offers the highest data compression with best overall query performance |
| Heap Tables | Smaller tables with less than 100 million rows, commonly used as a staging table prior to transformation |
| Clustered Index | large lookup tables (> 100 million rows) where querying will only result in a single row returned |
| Clustered Index + non-clustered secondary index | large tables (> 100 million rows) when single (or very few) records are being returned in queries |

| Table Distribution/Partition Type | Recommended use |
|--------------------|-------------|
| Hash distribution | tables that are larger than 2 GBs with infrequent insert/update/delete operations, works well for large fact tables in a star schema |
| Round robin distribution | default distribution, when little is known about the data or how it will be used. Use this distribution for staging tables |
| Replicated tables | smaller lookup tables less than 1.5 GB in size |

### Task 1: Create the sale table

Over the past 5 years, Wide World Importers has amassed over 3 billion rows of sales data. With this quantity of data, the storage consumed would be greater than 2 GB. While we will be using only a subset of this data for the lab, we will design the table for the production environment. Using the guidance outlined in the current Exercise description, we can ascertain that we will need a **Clustered Columnstore** table with a **Hash** table distribution based on the **CustomerId** field which will be used in most queries. For further performance gains, the table will be partitioned by transaction date to ensure queries that include dates or date arithmetic are returned in a favorable amount of time.

1. Expand the left menu and select the **Develop** item. From the **Develop** blade, expand the **+** button and select the **SQL script** item.

    ![The left menu is expanded with the Develop item selected. The Develop blade has the + button expanded with the SQL script item highlighted.](media/develop_newsqlscript_menu.png)

2. In the query tab toolbar menu, ensure you connect to your SQL Pool, `SQLPool01`.

    ![The query tab toolbar menu is displayed with the Connect to set to the SQL Pool.](media/querytoolbar_connecttosqlpool.png)

3. In the query window, copy and paste the following query to create the customer information table. Then select the **Run** button in the query tab toolbar.

    ```sql
      CREATE TABLE [wwi_mcw].[SaleSmall]
      (
        [TransactionId] [uniqueidentifier]  NOT NULL,
        [CustomerId] [int]  NOT NULL,
        [ProductId] [smallint]  NOT NULL,
        [Quantity] [tinyint]  NOT NULL,
        [Price] [decimal](9,2)  NOT NULL,
        [TotalAmount] [decimal](9,2)  NOT NULL,
        [TransactionDateId] [int]  NOT NULL,
        [ProfitAmount] [decimal](9,2)  NOT NULL,
        [Hour] [tinyint]  NOT NULL,
        [Minute] [tinyint]  NOT NULL,
        [StoreId] [smallint]  NOT NULL
      )
      WITH
      (
        DISTRIBUTION = HASH ( [CustomerId] ),
        CLUSTERED COLUMNSTORE INDEX,
        PARTITION
        (
          [TransactionDateId] RANGE RIGHT FOR VALUES (
            20180101, 20180201, 20180301, 20180401, 20180501, 20180601, 20180701, 20180801, 20180901, 20181001, 20181101, 20181201,
            20190101, 20190201, 20190301, 20190401, 20190501, 20190601, 20190701, 20190801, 20190901, 20191001, 20191101, 20191201)
        )
      );
    ```

4. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)
  
### Task 2: Populate the sale table

The data that we will be retrieving to populate the sale table is currently stored as a series of parquet files in the **asadatalake{SUFFIX}** data lake (Azure Data Lake Storage Gen 2). This storage account has already been added as a linked service in Azure Synapse Analytics when the environment was provisioned. Linked Services are synonymous with connection strings in Azure Synapse Analytics. Azure Synapse Analytics linked services provides the ability to connect to nearly 100 different types of external services ranging from Azure Storage Accounts to Amazon S3 and more.

1. Review the presence of the **asadatalake{SUFFIX}** linked service, by selecting **Manage** from the left menu, and selecting **Linked services** from the blade menu. Filter the linked services by the term **asadatalake** to find the **asadatalake{SUFFIX}** item. Further investigating this item will unveil that it makes a connection to the storage account using a storage account key.
  
   ![The Manage item is selected from the left menu. The Linked services menu item is selected on the blade. On the Linked services screen the term asadatalake{SUFFIX} is entered in the search box and the asadatalake{SUFFIX} Azure Blob Storage item is selected from the filtered results list.](media/manage_linkedservices_solliancepublicdata.png)

2. The sale data for each day is stored in a separate parquet file which is placed in storage following a known convention. In this lab, we are interested in populating the Sale table with only 2018 and 2019 data. Investigate the structure of the data by selecting the **Data** tab, and in the **Data** pane, select the **Linked** tab, and expanding the `asadatalake{SUFFIX}` Storage account.

    > **Note**: The current folder structure for daily sales data is as follows: /wwi-02/sale-small/Year=`YYYY`/Quarter=`Q#`/Month=`M`/Day=`YYYYMMDD` - where `YYYY` is the 4 digit year (eg. 2019), `Q#` represents the quarter (eg. Q1), `M` represents the numerical month (eg. 1 for January) and finally `YYYYMMDD` represents a numeric date format representation (eg. `20190516` for May 16, 2019).
    > A single parquet file is stored each day folder with the name **sale-small-YYYYMMDD-snappy.parquet** (replacing `YYYYMMDD` with the numeric date representation).

    ```text
    Sample path to the parquet folder for January 1st, 2019:
    /wwi-02/sale-small/Year=2019/Quarter=Q1/Month=1/Day=20190101/sale-small-20190101-snappy.parquet
    ```

3. Create a new Dataset by selecting **Data** from the left menu, expanding the **+** button on the Data blade and selecting **Dataset**. We will be creating a dataset that will point to the root folder of the sales data in the data lake.

4. In the **New dataset** blade, with the **All** tab selected, choose the **Azure Data Lake Storage Gen2** item. Select **Continue**.

    ![The New dataset blade is displayed with the All tab selected, the Azure Data Lake Storage Gen2 item is selected from the list.](media/new_dataset_type_selection.png)

5. In the **Select format** screen, choose the **Parquet** item. Select **Continue**.

    ![In the Select format screen, the Parquet item is highlighted.](media/dataset_format_parquet.png)

6. In the **Set properties** blade, populate the form as follows then select **OK**.
  
   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_sales_parquet** |
   | Linked service | Select **asadatalake{SUFFIX}** |
   | File path - Container | Enter **wwi-02** |  
   | File path - Folder | Enter **sale-small** |
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is displayed with fields populated with the values from the preceding table.](media/dataset_salesparquet_propertiesform.png)

7. Now we will need to define the destination dataset for our data. In this case we will be storing sale data in our SQL Pool. Create a new dataset by expanding the **+** button on the **Data** blade and selecting **Dataset**.

8. On the **New dataset** blade, with the **Azure** tab selected, enter **synapse** as a search term and select the **Azure Synapse Analytics (formerly SQL DW)** item. Select **Continue**.
  
9. On the **Set properties** blade, set the field values to the following, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_sale_asa** |
   | Linked service | Select `SQLPool01`. |
   | Table name | Select **wwi_mcw.SaleSmall**. |  
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is populated with the values specified in the preceding table.](media/dataset_saleasaform.png)
  
10. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to deploy the changes to the workspace.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

11. Since we want to filter on multiple sale year folders (Year=2018 and Year=2019) and copy only the 2018 and 2019 sales data, we will need to create a data flow to define the specific data that we wish to retrieve from our source dataset. To create a new data flow, start by selecting **Develop** from the left menu, and in the **Develop** blade, expand the **+** button and select **Data flow**.

    ![From the left menu, the Develop item is selected. From the Develop blade the + button is expanded with the Data flow item highlighted.](media/develop_newdataflow_menu.png)

12. In the side pane on the **General** tab, name the data flow by entering **ASAMCW_Exercise_2_2018_and_2019_Sales** in the **Name** field.

    ![The General tab is displayed with ASAMCW_Exercise_2_2018_and_2019_Sales entered as the name of the data flow.](media/dataflow_generaltab_name.png)

13. In the data flow designer window, select the **Add Source** box.

    ![The Add source box is highlighted in the data flow designer window.](media/dataflow_addsourcebox.png)

14. With the added source selected in the designer, in the lower pane with the **Source settings** tab selected, set the following field values:
  
    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **salesdata** |
    | Source type | Select **Dataset**. |
    | Dataset | Select **asamcw_sales_parquet**. |

    ![The Source settings tab is selected displaying the Output stream name set to salesdata and the selected dataset being asamcw_sales_parquet.](media/dataflow_source_sourcesettings.png)

15. Select the **Source options** tab, and add the following as **Wildcard paths**, this will ensure that we only pull data from the parquet files for the sales years of 2018 and 2019:

    1. sale-small/Year=2018/\*/\*/\*/\*

    2. sale-small/Year=2019/\*/\*/\*/\*

      ![The Source options tab is selected with the above wildcard paths highlighted.](media/dataflow_source_sourceoptions.png)

16. Select the **Project** tab, then select **Import projection**

17. At the bottom right of the **salesdata** source, expand the **+** button and select the **Sink** item located in the **Destination** section of the menu.

      ![The + button is highlighted toward the bottom right of the source element on the data flow designer.](media/dataflow_source_additem.png)

18. In the designer, select the newly added **Sink** element and in the bottom pane with the **Sink** tab selected, fill the form as follows:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **sale** |
    | Incoming stream | Select **salesdata**. |
    | Sink type | Select **Dataset**. |
    | Dataset | Select **asamcw_sale_asa**. |

    ![The Sink tab is displayed with the form populated with the values from the preceding table.](media/dataflow_sink_sinktab.png)

19. Select the **Mapping** tab and toggle the **Auto mapping** setting to the off position. You will need to select Input columns for the following:
  
    | Input column | Output column |
    |-------|-------|
    | Quantity | Quantity |
    | TransactionDate  | TransactionDateId |
    | Hour | Hour |
    | Minute | Minute |

    ![The Mapping tab is selected with the Auto mapping toggle set to the off position. The + Add mapping button is highlighted along with the mapping entries specified in the preceding table.](media/dataflow_sink_mapping.png)

20. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to deploy the new data flow to the workspace.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

21. We can now use this data flow as an activity in a pipeline. Create a new pipeline by selecting **Orchestrate** from the left menu, and in the **Orchestrate** blade, expand the **+** button and select **Pipeline**.

22. On the **Properties** blade, Enter **ASAMCW - Exercise 2 - Copy Sale Data** as the Name of the pipeline.

23. From the **Activities** menu, expand the **Move & transform** section and drag an instance of **Data flow** to the design surface of the pipeline.
  
    ![The Activities menu of the pipeline is displayed with the Move and transform section expanded. An arrow indicating a drag operation shows adding a Data flow activity to the design surface of the pipeline.](media/pipeline_sales_dataflowactivitymenu.png)

24. In the **Adding data flow** blade, ensure **Use existing data flow** is selected, and choose **ASAMCW_Exercise_2_2018_and_2019_Sales** from the select list and select **Finish**.

    ![The Adding data flow blade is displayed populated with the appropriate values.](media/pipeline_dataflowactivity_addingblade.png)

25. Select the **Settings** tab and set the form fields to the following values:

    | Field | Value |
    |-------|-------|
    | Data flow  | Select **ASAMCW_Exercise_2_2018_and_2019_Sales** |
    | Staging linked service | Select `asadatalake{SUFFIX}`. |
    | Staging storage folder - Container | Enter **staging** |
    | Staging storage folder - Folder | Enter **mcwsales** |

    ![The data flow activity Settings tab is displayed with the fields specified in the preceding table highlighted.](media/pipeline_sales_dataflowsettings.png)

26. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

27. Once published, expand the **Add trigger** item on the pipeline designer toolbar, and select **Trigger now**. In the **Pipeline run** blade, select **OK** to proceed with the latest published configuration. You will see notification toast windows indicating the pipeline is running and when it has completed.

28. View the status of the pipeline run by locating the **ASAMCW - Exercise 2 - Copy Sale Data** pipeline in the Orchestrate blade. Expand the actions menu, and select the **Monitor** item.

    ![In the Orchestrate blade, the Action menu is displayed with the Monitor item selected on the ASAMCW - Exercise 2 - Copy Sale Data pipeline.](media/orchestrate_pipeline_monitor_copysaledata.png)
  
29. You should see a run of the pipeline we created in the **Pipeline runs** table showing as in progress. It will take approximately 45 minutes for this pipeline operation to complete. You will need to refresh this table from time to time to see updated progress. Once it has completed. You should see the pipeline run displayed with a Status of **Succeeded**. _Feel free to proceed to the following tasks in this exercise while this pipeline runs_.
  
    ![On the pipeline runs screen, a successful pipeline run is highlighted in the table.](media/pipeline_run_sales_successful.png)

30. Verify the table has populated by creating a new query. Select the **Develop** item from the left menu, and in the **Develop** blade, expand the **+** button, and select **SQL script**. In the query window, be sure to connect to the SQL Pool database (`SQLPool01`), then paste and run the following query. When complete, select the **Discard all** button from the top toolbar.

  ```sql
    select count(TransactionId) from wwi_mcw.SaleSmall;
  ```

### Task 3: Create the customer information table

Over the past 5 years, Wide World Importers has amassed over 3 billion rows of sales data. With this quantity of data, the customer information lookup table is estimated to have over 100 million rows but will consume less than 1.5 GB of storage. While we will be using only a subset of this data for the lab, we will design the table for the production environment. Using the guidance outlined in the Exercising description, we can ascertain that we will need a **Clustered Columnstore** table with a **Replicated** table distribution to hold customer data.

1. Expand the left menu and select the **Develop** item. From the **Develop** blade, expand the **+** button and select the **SQL script** item.

    ![The left menu is expanded with the Develop item selected. The Develop blade has the + button expanded with the SQL script item highlighted.](media/develop_newsqlscript_menu.png)

2. In the query tab toolbar menu, ensure you connect to your SQL Pool, `SQLPool01`.

    ![The query tab toolbar menu is displayed with the Connect to set to the SQL Pool.](media/querytoolbar_connecttosqlpool.png)

3. In the query window, copy and paste the following query to create the customer information table. Then select the **Run** button in the query tab toolbar.
  
   ```sql
    CREATE TABLE [wwi_mcw].[CustomerInfo]
    (
      [UserName] [nvarchar](100)  NULL,
      [Gender] [nvarchar](10)  NULL,
      [Phone] [nvarchar](50)  NULL,
      [Email] [nvarchar](150)  NULL,
      [CreditCard] [nvarchar](21)  NULL
    )
    WITH
    (
      DISTRIBUTION = REPLICATE,
      CLUSTERED COLUMNSTORE INDEX
    )
    GO
   ```

   ![The query tab toolbar is displayed with the Run button selected.](media/querytoolbar_run.png)

4. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

### Task 4: Populate the customer information table

1. The data that we will be retrieving to populate the customer information table is currently stored in CSV format in the data lake (Azure Data Lake Storage Gen2 account). The storage account that possesses this data has already been added as a linked service in Azure Synapse Analytics when the environment was provisioned. 

2. Similar to the previous step, the destination for our data has also been added as a linked service. In this case, the destination for our data is our SQL Pool, `SQLPool01`. Repeat the previous step, this time filtering with the term **sqlpool** to verify the existence of the linked service.

3. The next thing that we will need to do is define a source dataset that will represent the information that we are copying over. This dataset will reference the CSV file containing customer information. From the left menu, select **Data**. From the **Data** blade, expand the **+** button and select **Dataset**.

    ![The Data item is selected from the left menu. On the Data blade, the + button is expanded with the Dataset item highlighted.](media/data_newdatasetmenu.png)

4. On the **New dataset** blade, with the **All** tab selected, choose the **Azure Data Lake Gen2** item. Select **Continue**.  
  
    ![On the New dataset blade, the All tab is selected and the Azure Blob Storage item is highlighted.](media/newdataset_azuredatalakegen2.png)

5. On the **Select format** blade, select **CSV Delimited Text**. Select **Continue**.

    ![On the Select format blade the CSV Delimited Text item is highlighted.](media/newdataset_selectfileformat_csv.png)

6. On the **Set properties** blade, set the fields to the following values, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_customerinfo_csv** |
   | Linked service | Select **asadatalake{SUFFIX}**.|
   | File Path - Container | Enter **wwi-02** |
   | File Path - Directory | Enter **customer-info** |
   | File Path - File | Enter **customerinfo.csv** |
   | First row as header | Checked |
   | Import schema | Select **From connection/store** |

    ![The Set properties form is displayed with the values specified in the previous table.](media/customerinfodatasetpropertiesform.png)

7. Now we will need to define the destination dataset for our data. In this case we will be storing customer information data in our SQL Pool. On the **Data** blade, expand the **+** button just as you did in **Step 3**.

8. On the **New dataset** blade, with the **Azure** tab selected, enter **synapse** as a search term and select the **Azure Synapse Analytics (formerly SQL DW)** item. Select **Continue**.

    ![On the New dataset blade, synapse is entered as the search term and Azure Synapse Analytics (formerly SQL DW) is selected from the filtered results.](media/newdataset_synapseitem.png)
  
9. On the **Set properties** blade, set the field values to the following, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_customerinfo_asa** |
   | Linked service | Select `SQLPool01`. |
   | Table name | Select **wwi_mcw.CustomerInfo**. |  
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is populated with the values specified in the preceding table.](media/dataset_customerinfoasaform.png)
  
10. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

11. Next, we will define a pipeline to populate data into the CustomerInfo table. From the left menu, select **Orchestrate**. From the Orchestrate blade, select the **+** button and select the **Pipeline** item.

    ![The Orchestrate menu item is selected from the left menu. On the Orchestrate blade, the + button is expanded with the Pipeline item highlighted.](media/orchestrate_newpipelinemenu.png)

12. In the bottom pane, on the **General** tab, enter **ASAMCW - Exercise 2 - Copy Customer Information** in the **Name** field.

    ![The General tab is shown with the name field populated as described above.](media/pipeline_customerinfo_generaltab.png)

13. In the **Activities** menu, expand the **Move & transform** item. Drag an instance of the **Copy data** activity to the design surface of the pipeline.

    ![In the Activities menu, the Move and transform section is expanded. An arrow denotes an instance of the Copy data activity being dragged over to the design surface of the pipeline.](media/pipeline_addcopydataactivity.png)

14. Select the **Copy data** activity on the pipeline design surface. In the bottom pane, on the **General** tab, enter **Copy Customer Information Data** in the **Name** field.

    ![The General tab is selected with the Name field set to Copy Customer Information Data.](media/pipeline_copycustomerinformation_general.png)

15. Select the **Source** tab in the bottom pane. In the **Source dataset** field, select **asamcw_customerinfo_csv**.

    ![The Source tab is selected with the Source dataset field set to asamcw_customerinfo_csv.](media/pipeline_copycustomerinformation_source.png)
  
16. Select the **Sink** tab in the bottom pane. In the **Sink dataset** field, select **asamcw_customerinfo_asa**, for the **Copy method** field, select **Bulk insert**, and for **Pre-copy script** enter:

    ```sql
      truncate table wwi_mcw.CustomerInfo
    ```

    ![The Sink tab is selected with the Sink dataset field set to asamcw_customerinfo_asa, the Copy method set to Bulk insert, and the Pre-copy script field set to the previous query.](media/pipeline_copycustomerinformation_sink.png)
  
17. Select the **Mapping** tab in the bottom pane. Select the **Import schemas** button. You will notice that Azure Synapse Analytics automated the mapping for us since the field names and types match.

    ![The Mapping tab is selected in the bottom pane. The source to destination field mapping is shown.](media/pipeline_copycustomerinformation_mapping.png)

18. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

19. Once published, expand the **Add trigger** item on the pipeline designer toolbar, and select **Trigger now**. In the **Pipeline run** blade, select **OK** to proceed with the latest published configuration. You will see notification toast windows indicating the pipeline is running and when it has completed.

20. View the status of the completed run by locating the **ASAMCW - Exercise 2 - Copy Customer Information** pipeline in the Orchestrate blade. Expand the actions menu, and select the **Monitor** item.

    ![In the Orchestrate blade, the Action menu is displayed with the Monitor item selected on the ASAMCW - Exercise 2 - Copy Customer Information pipeline.](media/pipeline_copycustomerinformation_monitormenu.png)
  
21. You should see a successful run of the pipeline we created in the **Pipeline runs** table.
  
    ![On the pipeline runs screen, a successful pipeline run is highlighted in the table.](media/pipeline_run_customerinfo_successful.png)

22. Verify the table has populated by creating a new query. Remember from **Task 1**, select the **Develop** item from the left menu, and in the **Develop** blade, expand the **+** button, and select **SQL script**. In the query window, be sure to connect to the SQL Pool database (`SQLPool01`), then paste and run the following query. When complete, select the **Discard all** button from the top toolbar.

  ```sql
    select * from wwi_mcw.CustomerInfo;
  ```
  
### Task 5: Create the campaign analytics table

The campaign analytics table will be queried primarily for dashboard and KPI purposes. Performance is a large factor in the design of this table, and as such  we can ascertain that we will need a **Clustered Columnstore** table with a **Hash** table distribution based on the **Region** field which will fairly evenly distribute the data.

1. Expand the left menu and select the **Develop** item. From the **Develop** blade, expand the **+** button and select the **SQL script** item.

    ![The left menu is expanded with the Develop item selected. The Develop blade has the + button expanded with the SQL script item highlighted.](media/develop_newsqlscript_menu.png)

2. In the query tab toolbar menu, ensure you connect to your SQL Pool, `SQLPool01`.

    ![The query tab toolbar menu is displayed with the Connect to set to the SQL Pool.](media/querytoolbar_connecttosqlpool.png)

3. In the query window, copy and paste the following query to create the customer information table. Then select the **Run** button in the query tab toolbar.

    ```sql
    CREATE TABLE [wwi_mcw].[CampaignAnalytics]
    (
        [Region] [nvarchar](50)  NULL,
        [Country] [nvarchar](30)  NOT NULL,
        [ProductCategory] [nvarchar](50)  NOT NULL,
        [CampaignName] [nvarchar](500)  NOT NULL,
        [Analyst] [nvarchar](25) NULL,
        [Revenue] [decimal](10,2)  NULL,
        [RevenueTarget] [decimal](10,2)  NULL,
        [City] [nvarchar](50)  NULL,
        [State] [nvarchar](25)  NULL
    )
    WITH
    (
        DISTRIBUTION = HASH ( [Region] ),
        CLUSTERED COLUMNSTORE INDEX
    );  
    ```

    ![The query tab toolbar is displayed with the Run button selected.](media/querytoolbar_run.png)

4. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

### Task 6: Populate the campaign analytics table

Similar to the customer information table, we will also be populating the campaign analytics table via a CSV file located in the data lake. This will require source and sink datasets to point to the CSV file in storage and the campaign analytics table that you just created in the SQL Pool. The source CSV file that was received is poorly formatted - we will need to add data transformations to make adjustments to this data before it is imported into the data warehouse.

1. The source dataset will reference the CSV file containing campaign analytics information. From the left menu, select **Data**. From the **Data** blade, expand the **+** button and select **Dataset**.

    ![The Data item is selected from the left menu. On the Data blade, the + button is expanded with the Dataset item highlighted.](media/data_newdatasetmenu.png)

2. On the **New dataset** blade, with the **All** tab selected, choose the **Azure Data Lake Storage Gen2** item. Select **Continue**.  
  
    ![The New dataset blade is displayed with the All tab selected, the Azure Data Lake Storage Gen2 item is selected from the list.](media/new_dataset_type_selection.png)

3. On the **Select format** blade, select **CSV Delimited Text**. Select **Continue**.

    ![On the Select format blade the CSV Delimited Text item is highlighted.](media/newdataset_selectfileformat_csv.png)

4. On the **Set properties** blade, set the fields to the following values, then select **OK**. You may choose to preview the data which will show a sample of the CSV file. Notice that since we are not setting the first row as the header, the header columns appear as the first row. Also, notice that the city and state values do not appear. This is because of the mismatch in the number of columns in the header row compared to the rest of the file. Soon, we will exclude the first row as we transform the data.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_campaignanalytics_csv** |
   | Linked service | Select **asadatalake{SUFFIX}**.|
   | File Path - Container | Enter **wwi-02** |
   | File Path - Directory | Enter **campaign-analytics** |
   | File Path - File | Enter **campaignanalytics.csv** |
   | First row as header | Unchecked |
   | Import schema | Select **From connection/store** |

    ![The Set properties form is displayed with the values specified in the previous table.](media/campaignanalyticsdatasetpropertiesform.png)

5. Now we will need to define the destination dataset for our data. In this case we will be storing campaign analytics data in our SQL Pool. On the **Data** blade, expand the **+** button and select **Dataset**.

6. On the **New dataset** blade, with the **Azure** tab selected, enter **synapse** as a search term and select the **Azure Synapse Analytics (formerly SQL DW)** item. Select **Continue**.

    ![On the New dataset blade, synapse is entered as the search term and Azure Synapse Analytics (formerly SQL DW) is selected from the filtered results.](media/newdataset_synapseitem.png)
  
7. On the **Set properties** blade, set the field values to the following, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_campaignanalytics_asa** |
   | Linked service | Select `SQLPool01`. |
   | Table name | Select **wwi_mcw.CampaignAnalytics**. |  
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is populated with the values specified in the preceding table.](media/dataset_campaignanalyticsasaform.png)
  
8. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

9. Since our source data is malformed and does not contain an Analyst column, we will need to create a data flow to transform the source data. A data flow allows you to graphically define dataset filters and transformations without writing code. These data flows can be leveraged as an activity in an orchestration pipeline. Create a new data flow, start by selecting **Develop** from the left menu, and in the **Develop** blade, expand the **+** button and select **Data flow**.

    ![From the left menu, the Develop item is selected. From the Develop blade the + button is expanded with the Data flow item highlighted.](media/develop_newdataflow_menu.png)

10. Select the Properties icon on the right side of the dataflow designer toolbar. In the **Properties** blade name the data flow by entering **ASAMCW_Exercise_2_Campaign_Analytics_Data** in the **Name** field.

    ![The Properties blade is displayed with ASAMCW_Exercise_2_Campaign_Analytics_Data entered as the name of the data flow.](media/dataflow_campaignanalytics_propertiesblade.png)

11. In the data flow designer window, select the **Add Source** box.

    ![The Add source box is highlighted in the data flow designer window.](media/dataflow_addsourcebox.png)

12. Under **Source settings**, configure the following:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **campaignanalyticscsv** |
    | Source type | Select **Dataset** |
    | Dataset | Select **asamcw_campaignanalytics_csv**. |
    | Skip line count | Enter **1** |  

    ![The Source settings tab is displayed with a form populated with the values defined in the preceding table.](media/dataflow_campaignanalytics_sourcesettings.png)

13. When you create data flows, certain features are enabled by turning on debug, such as previewing data and importing a schema (projection). Due to the amount of time it takes to enable this option, as well as environmental constraints of the lab environment, we will bypass these features. The data source has a schema we need to set. To do this, select **Script** from the right side of the dataflow designer toolbar menu.

    ![A portion of the dataflow designer toolbar is shown with the Script icon highlighted.](media/dataflow_toolbarscriptmenu.png)

14. Replace the script with the following to provide the column mappings (`output`), then select **OK**:

    ```json
        source(output(
            {_col0_} as string,
            {_col1_} as string,
            {_col2_} as string,
            {_col3_} as string,
            {_col4_} as string,
            {_col5_} as double,
            {_col6_} as string,
            {_col7_} as double,
            {_col8_} as string,
            {_col9_} as string
        ),
        allowSchemaDrift: true,
        validateSchema: false,
        skipLines: 1) ~> campaignanalyticscsv
    ```

    > NOTE: We are changing the mappings as the source file was corrupted with the wrong headers.

15. Select the **campaignanalyticscsv** data source, then select **Projection**. The projection should display the following schema:

    ![The Projection tab is displayed with columns defined as described in the column mapping script.](media/dataflow_campaignanalytics_projectiontab.png)

16. Select the **+** to the bottom right of the **campaignanalyticscsv** source, then select the **Select** schema modifier from the context menu.

    ![The + button on the bottom right of the campaignanalyticscsv source is highlighted.](media/dataflow_campaignanalytics_addstep.png)

17. In the bottom pane, under **Select settings**, configure the following:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **mapcampaignanalytics** |

    For **Input Columns**, under the **Name as** column, enter the following list values in order:
      - Region
      - Country
      - ProductCategory
      - CampaignName
      - RevenuePart1
      - Revenue
      - RevenueTargetPart1
      - RevenueTarget
      - City
      - State

    ![The Select settings tab is displayed with the form filled as described in the preceding table.](media/dataflow_mapcampaignanalytics_selectsettings.png)

18. Select the **+** to the right of the **mapCampaignAnalytics** source, then select the **Derived Column** schema modifier from the context menu.

19. Under **Derived column's settings**, configure the following:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **convertandaddcolums** |

    For **Columns**, add the following (Note you will need to type in the **Analyst** column):

    | Column | Expression | Description |
    | --- | --- | --- |
    | Revenue | **toDecimal(replace(concat(toString(RevenuePart1), toString(Revenue)), '\\\\', ''), 10, 2, '$###,###.##')** | Concatenate the **RevenuePart1** and **Revenue** fields, replace the invalid `\` character, then convert and format the data to a decimal type. |
    | RevenueTarget | **toDecimal(replace(concat(toString(RevenueTargetPart1), toString(RevenueTarget)), '\\\\', ''), 10, 2, '$###,###.##')** | Concatenate the **RevenueTargetPart1** and **RevenueTarget** fields, replace the invalid `\` character, then convert and format the data to a decimal type. |
    | Analyst | **iif(isNull(City), '',  replace('DataAnalyst'+ City,' ',''))** | If the city field is null, assign an empty string to the Analyst field, otherwise concatenate DataAnalyst to the City value, removing all spaces. |

    ![The derived column's settings are displayed as described.](media/dataflow_campaignanalytics_derivedcolumns.png)

20. Select the **+** to the right of the **convertandaddcolumns** step, then select the **Select** schema modifier from the context menu.

21. Under **Select settings**, configure the following:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **selectcampaignanalyticscolumns** |
    | Input columns | Delete the **RevenuePart1** and **RevenueTargetPart1** columns |

    ![The Select settings are displayed showing the updated column mappings.](media/dataflow_campaignanalytics_select2.png)

22. Select the **+** to the right of the **selectcampaignanalyticscolumns** step, then select the **Sink** destination from the context menu.

23. In the bottom pane, on the **Sink** tab, configure it as follows:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **campaignanlyticsasa** |
    | Dataset | Select **asamcw_campaignanalytics_asa**. |

    ![The Sink settings form is displayed populated with the values defined in the previous table.](media/dataflow_campaignanalytics_sink.png)

24. Select **Settings** tab, and for **Table action** select **Truncate table**.

    ![The sink Settings tab is displayed with the Table action set to Truncate table.](media/dataflow_campaignanalytics_sinksettings.png)

25. Your completed data flow should look similar to the following:

    ![The completed data flow is displayed.](media/dataflow_campaignanalytics_complete.png)
  
26. Select **Publish all** to save your new data flow.

    ![Publish all is highlighted.](media/publishall_toolbarmenu.png "Publish all")

27. Now that the data flow is published, we can use it in a pipeline. Create a new pipeline by selecting **Orchestrate** from the left menu, then in the **Orchestrate** blade, expand the **+** button and select **Pipeline**.

28. Expand the **Properties** pane on the right side of the pipeline designer. Enter **ASAMCW - Exercise 2 - Copy Campaign Analytics Data** in the **Name** field.

    ![The pipeline properties blade is displayed with the Name field populated with ASAMCW - Exercise 2 - Copy Campaign Analytics Data.](media/pipeline_properties_blade.png)

29. From the **Activities** menu, expand the **Move & transform** section and drag an instance of **Data flow** to the design surface of the pipeline.
  
    ![The Activities menu of the pipeline is displayed with the Move and transform section expanded. An arrow indicating a drag operation shows adding a Data flow activity to the design surface of the pipeline.](media/pipeline_sales_dataflowactivitymenu.png)

30. In the **Adding data flow** blade, select the data flow **ASAMCW_Exercise_2_Campaign_Analytics_Data**, then **Finish**. Select the Mapping Data Flow activity on the design surface.

31. In the bottom pane, select the **Settings** tab and set the form fields to the following values:

    | Field | Value |
    |-------|-------|
    | Data flow  | Select **ASAMCW_Exercise_2_Campaign_Analytics_Data** |
    | Staging linked service | Select `asadatalake{SUFFIX}`. |
    | Staging storage folder - Container | Enter **staging** |
    | Staging storage folder - Directory | Enter **mcwcampaignanalytics** |

    ![The data flow activity Settings tab is displayed with the fields specified in the preceding table highlighted.](media/pipeline_campaigndata_dataflowsettings.png)

32. In the top toolbar, select **Publish all** to publish the new pipeline. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

33. Once published, expand the **Add trigger** item on the pipeline designer toolbar, and select **Trigger now**. In the **Pipeline run** blade, select **OK** to proceed with the latest published configuration. You will see notification toast window indicating the pipeline is running and when it has completed.

34. View the status of the pipeline run by locating the **ASAMCW - Exercise 2 - Copy Campaign Analytics Data** pipeline in the Orchestrate blade. Expand the actions menu, and select the **Monitor** item.

    ![In the Orchestrate blade, the Action menu is displayed with the Monitor item selected on the ASAMCW - Exercise 2 - Copy Sale Data pipeline.](media/orchestrate_pipeline_monitor_copysaledata.png)
  
35. You should see a run of the pipeline we created in the **Pipeline runs** table showing as in progress. You will need to refresh this table from time to time to see updated progress. Once it has completed. You should see the pipeline run displayed with a Status of **Succeeded**.

36. Verify the table has populated by creating a new query. Select the **Develop** item from the left menu, and in the **Develop** blade, expand the **+** button, and select **SQL script**. In the query window, be sure to connect to the SQL Pool database (`SQLPool01`), then paste and run the following query. When complete, select the **Discard all** button from the top toolbar.

  ```sql
    select count(Region) from wwi_mcw.CampaignAnalytics;
  ```

### Task 7: Populate the product table

When the lab environment was provisioned, the **wwi_mcw.Product** table and datasets required for its population were created. Throughout this exercise, you have gained experience creating datasets, data flows, and pipelines. The population of the product table would be repetitive, so we will simply trigger an existing pipeline to populate this table.

1. From the left menu, select **Orchestrate**. From the **Orchestrate** blade, expand the **Pipelines** section and locate and select the **ASAMCW - Exercise 2 - Copy Product Information** pipeline.

2. Expand the **Add trigger** item on the pipeline designer toolbar, and select **Trigger now**. In the **Pipeline run** blade, select **OK** to proceed with the latest published configuration. You will see notification toast windows indicating the pipeline is running and when it has completed.

3. View the status of the pipeline run by locating the **ASAMCW - Exercise 2 - Copy Product Information** pipeline in the Orchestrate blade. Expand the actions menu, and select the **Monitor** item.

4. You should see a run of the pipeline we created in the **Pipeline runs** table showing as in progress (or succeeded). Once it has completed. You should see the pipeline run displayed with a Status of **Succeeded**.

5. Verify the table has populated by creating a new query. Select the **Develop** item from the left menu, and in the **Develop** blade, expand the **+** button, and select **SQL script**. In the query window, be sure to connect to the SQL Pool database (`SQLPool01`), then paste and run the following query. When complete, select the **Discard all** button from the top toolbar.

  ```sql
    select * from wwi_mcw.Product;
  ```

## Exercise 3: Exploring raw parquet

**Duration**: 15 minutes

Understanding data through data exploration is one of the core challenges faced today by data engineers and data scientists. Depending on the underlying structure of the data as well as the specific requirements of the exploration process, different data processing engines will offer varying degrees of performance, complexity, and flexibility.

In Azure Synapse Analytics, you have the possibility of using either the Synapse SQL Serverless engine, the big-data Spark engine, or both.

In this exercise, you will explore the data lake using both options.

### Task 1: Query sales Parquet data with Synapse SQL Serverless

When you query Parquet files using Synapse SQL Serverless, you can explore the data with T-SQL syntax.

1. From the left menu, select **Data**.

2. Expand **Storage accounts**. Expand the `asadatalake{SUFFIX}` ADLS Gen2 account and select **wwi-02**.

3. Navigate to the **sale-small/Year=2010/Quarter=Q4/Month=12/Day=20101231** folder. Right-click on the **sale-small-20101231-snappy.parquet** file, select **New SQL script**, then **Select TOP 100 rows**.

    ![The Storage accounts section is expanded with the context menu visible on the asadatalake{SUFFIX} account with the Select TOP 100 rows option highlighted.](media/data-hub-parquet-select-rows.png)

4. Ensure **SQL on-demand** is selected in the **Connect to** dropdown list above the query window, then run the query. Data is loaded by the Synapse SQL Serverless endpoint and processed as if was coming from any regular relational database.

    ![The SQL on-demand connection is highlighted on the query window toolbar.](media/sql-on-demand-selected.png "SQL on-demand")

5. Modify the SQL query to perform aggregates and grouping operations to better understand the data. Replace the query with the following, making sure that the file path in **OPENROWSET** matches your current file path, be sure to substitute `asadatalake{SUFFIX}` for the appropriate value in your environment:

    ```sql
    SELECT
        TransactionDate, ProductId,
        CAST(SUM(ProfitAmount) AS decimal(18,2)) AS [(sum) Profit],
        CAST(AVG(ProfitAmount) AS decimal(18,2)) AS [(avg) Profit],
        SUM(Quantity) AS [(sum) Quantity]
    FROM
        OPENROWSET(
            BULK 'https://asadatalake{SUFFIX}.dfs.core.windows.net/wwi-02/sale-small/Year=2010/Quarter=Q4/Month=12/Day=20101231/sale-small-20101231-snappy.parquet',
            FORMAT='PARQUET'
        ) AS [r] GROUP BY r.TransactionDate, r.ProductId;
    ```

    ![The T-SQL query above is displayed within the query window.](media/sql-serverless-aggregates.png "Query window")

6. Now let's figure out how many records are contained within the Parquet files for 2019 data. This information is important for planning how we optimize for importing the data into Azure Synapse Analytics. To do this, replace your query with the following (be sure to update the name of your data lake in BULK statement, by replacing `asadatalake{SUFFIX}`):

    ```sql
    SELECT
        COUNT_BIG(*)
    FROM
        OPENROWSET(
            BULK 'https://asadatalake{SUFFIX}.dfs.core.windows.net/wwi-02/sale-small/Year=2019/*/*/*/*',
            FORMAT='PARQUET'
        ) AS [r];
    ```

    > Notice how we updated the path to include all Parquet files in all subfolders of `sale-small/Year=2019`.

    The output should be **339507246** records.

### Task 2: Query sales Parquet data with Azure Synapse Spark

1. Select **Data** from the left menu, then browse to the data lake storage account folder **sale-small/Year=2010/Quarter=Q4/Month=12/Day=20101231** located in `asadatalake{SUFFIX}` if needed, then right-click the Parquet file and select New notebook.

    ![The Parquet file is displayed with the New notebook menu item highlighted.](media/new-spark-notebook-sales.png "New notebook")

2. This will generate a notebook with PySpark code to load the data in a dataframe and display 100 rows with the header.

3. Attach the notebook to a Spark pool.

    ![The Spark pool list is displayed.](media/attach-spark-pool.png "Attach to Spark pool")

4. Select **Run all** on the notebook toolbar to execute the notebook.

    > **Note:** The first time you run a notebook in a Spark pool, Synapse creates a new session. This can take approximately 5 minutes.
    > **Note:** To run just the cell, either hover over the cell and select the _Run cell_ icon to the left of the cell, or select the cell then type **Ctrl+Enter** on your keyboard.

5. Create a new cell underneath by selecting **{} Add code** when hovering over the blank space at the bottom of the notebook.

    ![The Add Code menu option is highlighted.](media/new-cell.png "Add code")

6. The Spark engine can analyze the Parquet files and infer the schema. To do this, enter the following in the new cell:

    ```python
    data_path.printSchema()
    ```

    Your output should look like the following:

    ```text
    root
        |-- TransactionId: string (nullable = true)
        |-- CustomerId: integer (nullable = true)
        |-- ProductId: short (nullable = true)
        |-- Quantity: short (nullable = true)
        |-- Price: decimal(29,2) (nullable = true)
        |-- TotalAmount: decimal(29,2) (nullable = true)
        |-- TransactionDate: integer (nullable = true)
        |-- ProfitAmount: decimal(29,2) (nullable = true)
        |-- Hour: byte (nullable = true)
        |-- Minute: byte (nullable = true)
        |-- StoreId: short (nullable = true)
    ```

7. Now let's use the dataframe to perform the same grouping and aggregate query we performed with the SQL Serverless pool. Create a new cell and enter the following:

    ```python
    from pyspark.sql import SparkSession
    from pyspark.sql.types import *
    from pyspark.sql.functions import *

    profitByDateProduct = (data_path.groupBy("TransactionDate","ProductId")
        .agg(
            sum("ProfitAmount").alias("(sum)ProfitAmount"),
            round(avg("Quantity"), 4).alias("(avg)Quantity"),
            sum("Quantity").alias("(sum)Quantity"))
        .orderBy("TransactionDate"))
    profitByDateProduct.show(100)
    ```

 > We import required Python libraries to use aggregation functions and types defined in the schema to successfully execute the query.

## Exercise 4: Exploring raw text based data with Azure Synapse SQL Serverless

**Duration**: 10 minutes

A common format for exporting and storing data is with text based files. These can delimited text files such as CSV as well as JSON structured data files. Azure Synapse Analytics also provides ways of querying into these types of raw files to gain valuable insights into the data without having to wait for them to be processed.

### Task 1: Query CSV data

1. Create a new SQL Script by selecting **Develop** from the left menu, then in the **Develop** blade, expanding the **+** button and selecting **SQL Query**.

2. Ensure **SQL on-demand** is selected in the **Connect to** dropdown list above the query window.

    ![The SQL on-demand connection is highlighted on the query window toolbar.](media/sql-on-demand-selected.png "SQL on-demand")

3. In this scenario, we will be querying into the CSV file that was used to populate the product table. This file is located in the `asadatalake{SUFFIX}` account at: **wwi-02/data-generators/generator-product.csv**. We will select all data from this file. Copy and paste the following query into the query window and select **Run** from the query window toolbar menu. Remember to replace `asadatalake{SUFFIX}` with your storage account name.

    ```sql
    SELECT
       csv.*
    FROM
        OPENROWSET(
            BULK 'https://asadatalake{SUFFIX}.dfs.core.windows.net/wwi-02/data-generators/generator-product/generator-product.csv',
            FORMAT='CSV',
            FIRSTROW = 1
        ) WITH (
            ProductID INT,
            Seasonality INT,
            Price DECIMAL(10,2),
            Profit DECIMAL(10,2)
        ) as csv
    ```

    > **Note**: In this query we are querying only a single file. Azure Synapse Analytics allows you to query across a series of CSV files (structured identically) by using wildcards in the path to the file(s).

4. You are also able to perform aggregations on this data. Replace the query with the following, and select **Run** from the toolbar menu. Remember to replace `asadatalake{SUFFIX}` with your storage account name.

    ```sql
    SELECT
        Seasonality,
        SUM(Price) as TotalSalesPrice,
        SUM(Profit) as TotalProfit
    FROM
        OPENROWSET(
            BULK 'https://asadatalake{SUFFIX}.dfs.core.windows.net/wwi-02/data-generators/generator-product/generator-product.csv',
            FORMAT='CSV',
            FIRSTROW = 1
        ) WITH (
            ProductID INT,
            Seasonality INT,
            Price DECIMAL(10,2),
            Profit DECIMAL(10,2)
        ) as csv
    GROUP BY
        csv.Seasonality
    ```

5. After you have run the previous query, switch the view on the **Results** tab to **Chart** to see a visualization of the aggregation of this data. Feel free to experiment with the chart settings to obtain the best visualization!

    ![The result of the previous aggregation query is displayed as a chart in the Results pane.](media/querycsv_serverless_chart.png)

6. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

### Task 2: Query JSON data

1. Create a new SQL Script by selecting **Develop** from the left menu, then in the **Develop** blade, expanding the **+** button and selecting **SQL Query**.

2. Ensure **SQL on-demand** is selected in the **Connect to** dropdown list above the query window.

    ![The SQL on-demand connection is highlighted on the query window toolbar.](media/sql-on-demand-selected.png "SQL on-demand")

3. Replace the query with the following, remember to replace `asadatalake{SUFFIX}` with the name of your storage account:

    ```sql
    SELECT
        products.*
    FROM
        OPENROWSET(
            BULK 'https://asadatalake{SUFFIX}.dfs.core.windows.net/wwi-02/product-json/json-data/*.json',
            FORMAT='CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b',
            ROWTERMINATOR = '0x0b'
        )
        WITH (
            jsonContent NVARCHAR(200)
        ) AS [raw]
    CROSS APPLY OPENJSON(jsonContent)
    WITH (
        ProductId INT,
        Seasonality INT,
        Price DECIMAL(10,2),
        Profit DECIMAL(10,2)
    ) AS products
    ```

4. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)


## Exercise 5: Synapse Pipelines & Cognitive Search

**Duration**: 15 minutes

In this exercise you will create a Synapse Pipeline that will orchestrate updating the part prices from a supplier invoice. You will accomplish this by a combination of a Synapse Pipeline with an Azure Cognitive Search Skillset that invokes the Form Recognizer service as a custom skill. The pipeline will work as follows:

- Invoice is uploaded to Azure Storage.
- An Azure Cognitive Search index is started
- The index of any new or updated invoices invokes an Azure Cognitive Search skillset.
- The first skill in the skillset invokes an Azure Function, passing it the URL to the PDF invoice. 
- The Azure Function invokes the Form Recognizer service, passing it the URL and SAS token to the PDF invoice. Forms recognizer returns the OCR results to the function.
- The Azure Function returns the results to skillset. The skillset then extracts only the product names and costs and sends that to a configure knowledge store that writes the extracted data to JSON files in Azure Blob Storage.
- The Synapse pipeline reads these JSON files from Azure Storage in a Data Flow activity and performs an upsert against the product catalog table in the Synapse SQL Pool. 

### Task 1: Create the necessary storage account

1. Create a new storage account in your Resource group

    ![displaying creating a new storage account](media/ex5-task1a-000.png)

1. In the properties for the storage account set:
    - Resource group to the group you're using for this workshop
    - Storage account name to something related to invoices
    - Location should be the same location you're Synapse workspace is in
    - Performance: Standard
    - Account kind: StorageV2
    - Replication: Locally-redundant storage
    - Access tier: Hot

        Click "Review + create"

        ![display the configuration settings described above](media/ex5-task1a-001.png)

3. Click Create
4. Once it's created, navigate to the resource

    ![display the completed provisioning and clicking to go to the resource](media/ex5-task1a-002.png)

1. Click Access keys

    ![displaying the location of access keys link in the navigation on the left](media/ex5-task1a-003.png)

1. Copy the Connection string under "key1". Save it to notepad, Visual Studio Code, or another text file. We'll use this several times

    ![display the button to click in order to copy the connection string](media/ex5-task1a-004.png)

1. Click on Shared access signature
1. Make sure all the checkboxes are selected and click Generate SAS

    ![display configuration for the shared access signature](media/ex5-task1a-012.png)

1. Copy the generated Blob service SAS URL to the same text file as above

    ![display the shared access signature blob service SAS URL that was generated](media/ex5-task1a-013.png)

1. Under the "Blob service" header, click "Containers"

    ![display the link on the left side navigation to go to Containers](media/ex5-task1a-005.png)

1. Create the following three containers

    - invoices
    - invoices-json
    - invoices-staging

    1. Click on +Container

    ![display the +container button](media/ex5-task1a-006.png)

    2. Type in the name and click Create

    ![display creating a new container](media/ex5-task1a-007.png)

    3. Repeat this for the other two containers

    ![displaying all the containers created](media/ex5-task1a-008.png)

8. Use [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) to open up the invoices container and create two folders Test and Train

    1. Create a Train folder

    ![displaying Azure Storage explorer and the steps to create the Train](media/ex5-task1a-009.png)

    2. Upload the 5 invoices to it from /artifacts/sample_invoices/Train

    ![displaying uploading the Train files to Azure](media/ex5-task1a-010.png)

    3. Repeat the same for the Test folder and use the files in /Test instead of Train
    
    When you're done you should have a Train folder with Invoices 1 - 5 in it and a Test folder with Invoices 6 and 7

    ![showing the Train and Test folders](media/ex5-task1a-011.png)

### Task 2: Create and train Azure Forms recognizer model and Cognitive Search

1. Browse to your Azure Portal, create a resource, and create a new instance of Cognitive Services

    ![display creating Cognitive Services](media/ex5-task2a-01.png)

2. Click Create

    ![display the create button](media/ex5-task2a-02.png)

3. Set the following configuration settings:

    - Name: choose a name for processing invoice forms that you want to use
    - Subscription: the subscription that you're using for his exercise
    - Location: the same location you've been using for all other resources
    - Pricing: Free F0
    - Resource group: the same resource group you've been using for this exercise
    - Check the confirmation checkbox

    Then click Create

    ![display the configuraiton settings for Cognitive Services](media/ex5-task2a-03.png)

4. Wait for the service to provision then navigate to the resource
5. Click on "Keys and Endpoint on the left side"

    ![display the keys and endpoint navigation link in the left side navigation](media/ex5-task2a-04.png)

6. Copy and Paste both KEY 1 and the ENDPOINT. Put these in the same location as the storage connection string you copied earlier

    ![display the endpoint and keys for Cognitive services](media/ex5-task2a-05.png)

7. Browse to your Azure Portal a create a new resource and create a new instance of Azure Cognitive Search

    ![search for and select Azure Cognitive Search](media/ex5-task1-006.png)

1. Choose the subscription and resource group you've been using. Set the URL of the Cognitive Search Service and switch the pricing tier to free

    ![displaying the basics tab of the new search service and the configuration settings](media/ex5-task1-007.png)

2. Click "Review + create"

    ![displaying the review + create button](media/ex5-task1-008.png)

3. Click "Create"
4. Wait for the service to be provisioned
9. Make sure python is installed on computer
10. Open Visual Studio Code
11. Open the file /artifacts/pocofrmreader.py
12. Update Lines 7, 9, and 17 with the appropriate
    
    - Line 7: The endpoint of Azure Cognitive Services
    - Line 9: The Blob Service SAS URL storage account with your Train and Test invoice folders. Make sure to update the URL to include /invoices 
    - Line 13: The key for your Azure Cognitive Service endpoint
    
    ![display lines 7 - 13 of the python script to update](media/ex5-task2a-06.png)

13. Save the file
14. Click Run, Start Debugging

    ![display clicking Run in the file menu, then start debugging](media/ex5-task2a-07.png)

15. Choose Python File

    ![display python file selection](media/ex5-task2a-08.png)

16. When it completes, you should see an output similar to what is seen in the screenshot below. The output should also contain a modelID. Copy and paste this into your text file to use later

    ![display the output of the python script, including the  modelID](media/ex5-task2a-09.png)

### Task 3: Configure a skillset with Form Recognizer

1. Open Visual Studio Code
2. Make sure you have the Azure Functions extension installed.

   ![The header of the azure functions module is show](media/ex5-task1-000.png)

3. In Visual Studio Code open the folder /environment-setup/functions

   ![The functions folder to open is shown](media/ex5-task1-001.png)

4. In __init__.py update lines 66, 68, 70, and 73 to have the appropriate values for your environment

   ![The lines of code thta need to be updated are displayed](media/ex5-task1-step2.png)

5. Use the Azure Functions extension to publish to a new Azure function in the same Resource Group as your Synapse workspace

   ![the button to publish the function is displayed](media/ex5-task1-002.png)

    1. Select the same subscription as your Synapse workspace
    2. Choose to "Create new Function App in Azure..."
    3. Give you function a name. This function is what will call the Azure Form Recognizer

      ![setting the function name in visual studio](media/ex5-task1-003.png)

    4. For the runtime select Python 3.7

      ![setting the python runtime version](media/ex5-task1-004.png)

    5. Deploy the function to the same region as your Synapse workspace

        ![choosing the deployment region](media/ex5-task1-005.png)

    6. Your function is now ready for user in the skillset
6. Now that we have the function publish and all our resources created, we can create the skillset. This will be accomplished using postman. If you don't have it downloaded and installed, you can get it from [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
7. Open Postman
8. Import the postman collection  from /environment-setup/skillset

    ![view the import option in the menu](media/ex5-task3-004.png)

    ![view the upload file button](media/ex5-task3-005.png)

    ![view adding the file to postman](media/ex5-task3-006.png)

9. Click Import
10.  This will give you 4 items on the left side. Create Index, Create Datasource, Create the skillset, and Create the Indexer.

    ![display the four options on the left side of postman](media/ex5-task3-007.png)

11. The first thing we need to do, is edit some properties that effect all of these. Hover over "Create a KnowledgeStore", click ..., and then click Edit

    ![display the edit menu option](media/ex5-task3-008.png)

12. Select the variables tab at the top

    ![displyas the tabs across the top](media/ex5-task3-009.png)

13. We are going to need to edit each one of these variables to match the following:
    
    -  admin-key: The key from the search service you created

        ![displyas the search service key to copy](media/ex5-task3-010.png)

    - search-service-name: The name of the search service that you just got the key from

        ![displyas the search service name](media/ex5-task3-011.png)

    - storage-account-name: The name of the storage account used for blob storage for your invoices

        ![displyas the name of the storage account](media/ex5-task3-012.png)

    - storage-connection-string: The connection string from the same storage account

        ![displyas the connection string to the storage account](media/ex5-task3-013.png)

    - datasourcename: invoices
    - indexer-name: invoice-indexer
    - index-name: invoice-index
    - skillset-name: invoice-skillset
    - storage-container-name: invoices
    - skillset-function: the function URL from the function you published

        ![displays function url from the app service, functions, the function name, overview, and get function url](media/ex5-task3-021.png)

    - cognitive-services-key: the key from the cognitive service being used for forms processing

        ![displays the key from the cognitive service, keys and endpoint, key 1](media/ex5-task3-022.png)

    - cognitive-service-path: this is the URL to the cognitive service from /subscription all the way through /accounts/[servicename]. An example is: /subscriptions/[SUBSCRIPTOIN ID]/resourceGroups/synapse-mcw-v2-zst/providers/Microsoft.CognitiveServices/accounts/wwiformsreader

        ![displays the url for the service path](media/ex5-task3-023.png)

14. Update the collection

    ![displays all the updated variables and the update button](media/ex5-task3-014.png)

15. Select "Create Index" and click the "Body" tab

    ![displays create index and body](media/ex5-task3-015.png)

16. Copy and paste all the text from /environment-setup/skillset/index.body.txt into the body of the request
 17. Click "Send"
 
   ![displays the send button](media/ex5-task3-016.png)

18. You should get a response that it was created

    ![displays the responce that it was created](media/ex5-task3-017.png)

19. Do the same for Create Datasource, Create the Skillset, and Create the indexer using the corresponding text files in the same location
20. After you Send the Indexer request, if you navigate to your search service you should see your indexer running. It will take a couple minutes to run.

   ![displays the running indexer](media/ex5-task3-018.png)

21. Once the indexer has run, it will show two successful documents. If you go to your Blob storage account and look at the invoices-json you will also have two folders with .json documents in them.

    ![displays the sucessful index](media/ex5-task3-019.png)

    ![displays the json documents](media/ex5-task3-020.png)
    
### Task 4: Create the Synapse Pipeline

1.  Open your Synapse workspace

![displays open synapse studio](media/ex5-task4-001.png)

2.  Expand the left menu and select the **Develop** item. From the **Develop** blade, expand the **+** button and select the **SQL script** item.

    ![The left menu is expanded with the Develop item selected. The Develop blade has the + button expanded with the SQL script item highlighted.](media/develop_newsqlscript_menu.png)

3. In the query tab toolbar menu, ensure you connect to your SQL Pool, `SQLPool01`.

    ![The query tab toolbar menu is displayed with the Connect to set to the SQL Pool.](media/querytoolbar_connecttosqlpool.png)

4. In the query window, copy and paste the following query to create the invoice information table. Then select the **Run** button in the query tab toolbar.

    ```sql
      CREATE TABLE [wwi_mcw].[Invoices]
      (
        [TransactionId] [uniqueidentifier]  NOT NULL,
        [CustomerId] [int]  NOT NULL,
        [ProductId] [smallint]  NOT NULL,
        [Quantity] [tinyint]  NOT NULL,
        [Price] [decimal](9,2)  NOT NULL,
        [TotalAmount] [decimal](9,2)  NOT NULL
      );
    ```

5. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

6. Click on "Manage" on the left menu

![display the manage link on the left side navigation](media/ex5-task4-002.png)

7. Click on "Linked services"

![display the Linked service link on the left side navigation](media/ex5-task4-003.png)

8. Click "+ New" to create a new linked service to the blob storage

![display the new button](media/ex5-task4-004.png)

9. Select Azure Blob Storage, click "Continue"

![display the Azure Blob Storage button when adding a new service](media/ex5-task4-005.png)

10. Configure the linked service in the way shown here. Provide a name that makes sense. Select the Azure subscription and Storage account name. Leave everything else set to its default. Click "Create"

![display the Azure Blob Storage connected service configuration](media/ex5-task4-006.png)

11. Click "+ New" to create a new linked service to the blob storage

![display the new button](media/ex5-task4-004.png)

12. Select Azure Synapse Analytics and click Continue

![display the Azure Synapse Analytics button when adding a new service](media/ex5-task4-007.png)

13. Configure the linked service in the way shown here. Provide a name that makes sense. Select the Azure subscription, server name, database name. Switch the Authentication type to managed identity. Leave everything else with their defaults. Click "Create"

![display the Azure Synapse Analytics connected service configuration](media/ex5-task4-008.png)

14. You should see the two new linked services now.

![display the Azure Synapse Analytics connected service configuration](media/ex5-task4-010.png)

15. Using the same steps and 3 and 4, run a new SQL query against the  master database to create a new master key.

```CREATE MASTER KEY ENCRYPTION BY PASSWORD =’Pass@Word01’;```

![display the SQL Query to create the master database](media/ex5-task4-011.png)

16. Click Orchestrate in the left navigation

![display the orchestrate link in the left navigation](media/ex5-task4-012.png)

17. Click + and then Pipeline to create a new pipeline

![display + link to create a new pipeline](media/ex5-task4-013.png)

18. Choose a name for your new pipeline

![display the pipeline properties](media/ex5-task4-014.png)

19. Click "Add trigger" then "New/Edit" to create an event to start the pipeline

![display the add new trigger](media/ex5-task4-015.png)

20. Select the dropdown and click +New

![display the new trigger option](media/ex5-task4-016.png)

21. For this exercise, we're going to do a schedule. However, in the future you'll also be able to use an event-based trigger that would fire off new JSON files being added to blob storage
22. Set the trigger to start every 5 minutes, then click OK

![display the trigger properties for the trigger to start every 5 minutes](media/ex5-task4-017.png)

23. Click OK on the Run Parameters, nothing needs to be done here
24. Next we need to add a Data Flow to the pipeline. Under Activities, expand "Move & transform" then click and drag Data flow onto the canvas

![display selecting Data flow and dragging in onto the canvase](media/ex5-task4-018.png)

25. Create a new Data Flow and give it a name, click finish

![display the new data flow properties](media/ex5-task4-019.png)

26. Now we need to add our blob storage as a source for our data flow. Click "Add Source"

![display the add source button](media/ex5-task4-020.png)

27. Name your output stream, leave the source type to Dataset, Leave all the options and sampling set to their defaults. Click + New next to Dataset to add a new dataset

![dispaly the Source settings](media/ex5-task4-021.png)

28. Choose Azure Blob Storage

![dispaly the Azure Blob Storage option](media/ex5-task4-022.png)

29. Choose Json and click Continue

![dispaly the Json option](media/ex5-task4-023.png)

30. Provide a name and then choose the Azure Storage linked service that you created earlier

![dispaly the Data set properties](media/ex5-task4-024.png)

31. For the file path chose invoices-json. Set import schema to "From sample file"

![dispaly the file path settings](media/ex5-task4-025.png)

32. Click to select a file and use the file /environment-setup/synapse/sampleformrecognizer.json. Click OK

![dispaly the Data set properties](media/ex5-task4-026.png)

33. With the jsonInvoice selected, switch to the "Source options tab" at the bottom. Add \*/\* to Wildcard paths

![dispaly the Data set properties](media/ex5-task4-048.png)

34. Click + to add another step in your data flow

![dispaly adding another step to the data flow](media/ex5-task4-028.png)

35. This one will be a derived column. This will be used to do some processing of the incoming data.

![dispaly adding another step to the data flow](media/ex5-task4-029.png)

36. Give it a name and add 3 columns that are going to be configured in the following ways
    
    - productprice: toDecimal(replace(productprice,'$',''))
    - totalcharges: toDecimal(replace(replace(totalcharges,'$',''),',',''))
    - quantity: toInteger(replace(quantity,',',''))

     ![dispaly the drives column settings](media/ex5-task4-030.png)

37. Click + again to add another step in your data flow

![dispaly adding another step to the data flow](media/ex5-task4-028.png)

38. This time select "Alter Row"

![dispaly adding alter row](media/ex5-task4-031.png)

39. Name the Output stream, leave incoming stream set to the default. Change Altar row conditions to "Upsert If" and then set the expression to notEquals(transactionid,"")

![dispaly adding alter row](media/ex5-task4-032.png)

40. Click + again to add another step in your data flow

![dispaly adding another step to the data flow](media/ex5-task4-028.png)

41. This time add a Sink

![dispaly adding a sink](media/ex5-task4-033.png)

42. Give it a name, leave everything else set to the defaults, and click "+New" to add a new Dataset

![dispaly sink settings](media/ex5-task4-034.png)

43. This time choose "Azure Synapse Analytics" and click "Continue"

![dispaly azure synapse analytics button](media/ex5-task4-035.png)

44. Provide a name, choose the linked Service we created earlier, then select the database we created in the beginning of this exercise. If you don't see it in the list of your table names, click the refresh button and it should show up. Click OK

![display SQL Dataset properties](media/ex5-task4-036.png)

45. Click the settings table and check the box to "Allow upsert", set the Key columns to transactionid

![display the settings tab](media/ex5-task4-037.png)

46. Click the Mapping tab, disable Auto mapping and configure the mappings between the json file and the database. Use "+ Add mapping" -> "Fixed mapping" to add a mapping

![display the settings tab](media/ex5-task4-038.png)

47. Click back to your pipeline using the tab at the top of your workspace

![display the mapping tab](media/ex5-task4-039.png)

48. Select your dataflow, and then go to the settings tab. We need to configure the staging settings

![display the mapping tab](media/ex5-task4-040.png)

49. Under the "PolyBase" settings, set the Staging linked service to the storage account we've been using and set the container to invoices-json

![display the completed settings tab](media/ex5-task4-041.png)

50. Click Publish All at the top to save your pipeline

![display the tab to go back to your pipeline](media/ex5-task4-042.png)

51. Click Publish

52. Shortly, you should see a notification that Publishing completed

![display publishing completed](media/ex5-task4-043.png)

53. Click on Monitor

![display publishing completed](media/ex5-task4-044.png)

54. Within 5 minutes, you should see a processing pipeline

![display publishing completed](media/ex5-task4-045.png)

55. After about 3 or 4 minutes it will complete.

![show completed pipeline](media/ex5-task4-046.png)

56. Then you can go query your database and you'll see the data from the two Test invoices

![show the data in the databases](media/ex5-task4-047.png)

## Exercise 6: Security

**Duration**: 15 minutes

### Task 1: Column level security

It is important to identify data columns of that hold sensitive information. Types of sensitive information could be social security numbers, email addresses, credit card numbers, financial totals, and more. Azure Synapse Analytics allows you define permissions that prevent users or roles select privileges on specific columns.

1. Create a new SQL Script by selecting **Develop** from the left menu, then in the **Develop** blade, expanding the **+** button and selecting **SQL Query**.

2. Copy and paste the following query into the query window. Then, step through each statement by highlighting it in the query window, and selecting **Run** from the query window toolbar menu. The query is documented inline. Ensure you are connected to **SQLPool01** when running the queries.

    ```sql
        /*  Column-level security feature in Azure Synapse simplifies the design and coding of security in applications.
        It ensures column level security by restricting column access to protect sensitive data. */

    /* Scenario: In this scenario we will be working with two users. The first one is the CEO, he has access to all
        data. The second one is DataAnalystMiami, this user doesn't have access to the confidential Revenue column
        in the CampaignAnalytics table. Follow this lab, one step at a time to see how Column-level security removes access to the
        Revenue column to DataAnalystMiami */

    --Step 1: Let us see how this feature in Azure Synapse works. Before that let us have a look at the Campaign Analytics table.
    select  Top 100 * from wwi_mcw.CampaignAnalytics
    where City is not null and state is not null

    /*  Consider a scenario where there are two users.
        A CEO, who is an authorized  personnel with access to all the information in the database
        and a Data Analyst, to whom only required information should be presented.*/

    -- Step:2 Verify the existence of the “CEO” and “DataAnalystMiami” users in the Datawarehouse.
    SELECT Name as [User1] FROM sys.sysusers WHERE name = N'CEO';
    SELECT Name as [User2] FROM sys.sysusers WHERE name = N'DataAnalystMiami';


    -- Step:3 Now let us enforcing column level security for the DataAnalystMiami.
    /*  The CampaignAnalytics table in the warehouse has information like ProductID, Analyst, CampaignName, Quantity, Region, State, City, RevenueTarget and Revenue.
        The Revenue generated from every campaign is classified and should be hidden from DataAnalystMiami.
    */

    REVOKE SELECT ON wwi_mcw.CampaignAnalytics FROM DataAnalystMiami;
    GRANT SELECT ON wwi_mcw.CampaignAnalytics([Analyst], [CampaignName], [Region], [State], [City], [RevenueTarget]) TO DataAnalystMiami;
    -- This provides DataAnalystMiami access to all the columns of the Sale table but Revenue.

    -- Step:4 Then, to check if the security has been enforced, we execute the following query with current User As 'DataAnalystMiami', this will result in an error
    --  since DataAnalystMiami doesn't have select access to the Revenue column
    EXECUTE AS USER ='DataAnalystMiami';
    select TOP 100 * from wwi_mcw.CampaignAnalytics;
    ---
    -- The following query will succeed since we are not including the Revenue column in the query.
    EXECUTE AS USER ='DataAnalystMiami';
    select [Analyst],[CampaignName], [Region], [State], [City], [RevenueTarget] from wwi_mcw.CampaignAnalytics;

    -- Step:5 Whereas, the CEO of the company should be authorized with all the information present in the warehouse.To do so, we execute the following query.
    Revert;
    GRANT SELECT ON wwi_mcw.CampaignAnalytics TO CEO;  --Full access to all columns.

    -- Step:6 Let us check if our CEO user can see all the information that is present. Assign Current User As 'CEO' and the execute the query
    EXECUTE AS USER ='CEO'
    select * from wwi_mcw.CampaignAnalytics
    Revert;
    ```

    ![The query tab toolbar is displayed with the Run button selected.](media/querytoolbar_run.png)

3. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

### Task 2: Row level security

In many organizations it is important to filter certain rows of data by user. In the case of WWI, they wish to have data analysts only see their data. In the campaign analytics table, there is an Analyst column that indicates to which analyst that row of data belongs. In the past, organizations would create views for each analyst - this was a lot of work and unnecessary overhead. Using Azure Synapse Analytics, you can define row level security that compares the user executing the query to the Analyst column, filtering the data so they only see the data destined for them.

1. Create a new SQL Script by selecting **Develop** from the left menu, then in the **Develop** blade, expanding the **+** button and selecting **SQL Query**.

2. Copy and paste the following query into the query window. Then, step through each statement by highlighting it in the query window, and selecting **Run** from the query window toolbar menu. The query is documented inline.

    ```sql
    /* Row level Security (RLS) in Azure Synapse enables us to use group membership to control access to rows in a table.
    Azure Synapse applies the access restriction every time the data access is attempted from any user.
    Let see how we can implement row level security in Azure Synapse.*/

    -- Row-Level Security (RLS), 1: Filter predicates
    -- Step:1 The Sale table has two Analyst values: DataAnalystMiami and DataAnalystSanDiego. 
    --     Each analyst has jurisdiction across a specific Region. DataAnalystMiami on the South East Region
    --      and DataAnalystSanDiego on the Far West region.
    SELECT DISTINCT Analyst, Region FROM wwi_mcw.CampaignAnalytics order by Analyst ;

    /* Scenario: WWI requires that an Analyst only see the data for their own data from their own region. The CEO should see ALL data.
        In the Sale table, there is an Analyst column that we can use to filter data to a specific Analyst value. */

    /* We will define this filter using what is called a Security Predicate. This is an inline table-valued function that allows
        us to evaluate additional logic, in this case determining if the Analyst executing the query is the same as the Analyst
        specified in the Analyst column in the row. The function returns 1 (will return the row) when a row in the Analyst column is the same as the user executing the query (@Analyst = USER_NAME()) or if the user executing the query is the CEO user (USER_NAME() = 'CEO')
        whom has access to all data.
    */

    -- Review any existing security predicates in the database
    SELECT * FROM sys.security_predicates

    --Step:2 Create a new Schema to hold the security predicate, then define the predicate function. It returns 1 (or True) when
    --  a row should be returned in the parent query.
    GO
    CREATE SCHEMA Security
    GO
    CREATE FUNCTION Security.fn_securitypredicate(@Analyst AS sysname)  
        RETURNS TABLE  
    WITH SCHEMABINDING  
    AS  
        RETURN SELECT 1 AS fn_securitypredicate_result
        WHERE @Analyst = USER_NAME() OR USER_NAME() = 'CEO'
    GO
    -- Now we define security policy that adds the filter predicate to the Sale table. This will filter rows based on their login name.
    CREATE SECURITY POLICY SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(Analyst)
    ON wwi_mcw.CampaignAnalytics
    WITH (STATE = ON);

    ------ Allow SELECT permissions to the Sale Table.------
    GRANT SELECT ON wwi_mcw.CampaignAnalytics TO CEO, DataAnalystMiami, DataAnalystSanDiego;

    -- Step:3 Let us now test the filtering predicate, by selecting data from the Sale table as 'DataAnalystMiami' user.
    EXECUTE AS USER = 'DataAnalystMiami'
    SELECT * FROM wwi_mcw.CampaignAnalytics;
    revert;
    -- As we can see, the query has returned rows here Login name is DataAnalystMiami

    -- Step:4 Let us test the same for  'DataAnalystSanDiego' user.
    EXECUTE AS USER = 'DataAnalystSanDiego';
    SELECT * FROM wwi_mcw.CampaignAnalytics;
    revert;
    -- RLS is working indeed.

    -- Step:5 The CEO should be able to see all rows in the table.
    EXECUTE AS USER = 'CEO';  
    SELECT * FROM wwi_mcw.CampaignAnalytics;
    revert;
    -- And he can.

    --Step:6 To disable the security policy we just created above, we execute the following.
    ALTER SECURITY POLICY SalesFilter  
    WITH (STATE = OFF);

    DROP SECURITY POLICY SalesFilter;
    DROP FUNCTION Security.fn_securitypredicate;
    DROP SCHEMA Security;
    ```

    ![The query tab toolbar is displayed with the Run button selected.](media/querytoolbar_run.png)

3. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

### Task 3: Dynamic data masking

As an alternative to column level security, SQL Administrators also have the option of masking sensitive data. This will result in data being obfuscated when returned in queries. The data is still stored in a pristine state in the table itself. SQL Administrators can grant unmask privileges to users that have permissions to see this data.

1. Create a new SQL Script by selecting **Develop** from the left menu, then in the **Develop** blade, expanding the **+** button and selecting **SQL Query**.

2. Copy and paste the following query into the query window. Then, step through each statement by highlighting it in the query window, and selecting **Run** from the query window toolbar menu. The query is documented inline.

    ```sql
    ----- Dynamic Data Masking (DDM) ---------
    /*  Dynamic data masking helps prevent unauthorized access to sensitive data by enabling customers
        to designate how much of the sensitive data to reveal with minimal impact on the application layer.
        Let see how */

    /* Scenario: WWI has identified sensitive information in the CustomerInfo table. They would like us to
        obfuscate the CreditCard and Email columns of the CustomerInfo table to DataAnalysts */

    -- Step:1 Let us first get a view of CustomerInfo table.
    SELECT TOP (100) * FROM wwi_mcw.CustomerInfo;

    -- Step:2 Let's confirm that there are no Dynamic Data Masking (DDM) applied on columns.
    SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
    FROM sys.masked_columns AS c  
    JOIN sys.tables AS tbl
        ON c.[object_id] = tbl.[object_id]  
    WHERE is_masked = 1
        AND tbl.name = 'CustomerInfo';
    -- No results returned verify that no data masking has been done yet.

    -- Step:3 Now lets mask 'CreditCard' and 'Email' Column of 'CustomerInfo' table.
    ALTER TABLE wwi_mcw.CustomerInfo  
    ALTER COLUMN [CreditCard] ADD MASKED WITH (FUNCTION = 'partial(0,"XXXX-XXXX-XXXX-",4)');
    GO
    ALTER TABLE wwi_mcw.CustomerInfo
    ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()');
    GO
    -- The columns are successfully masked.

    -- Step:4 Let's see Dynamic Data Masking (DDM) applied on the two columns.
    SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
    FROM sys.masked_columns AS c  
    JOIN sys.tables AS tbl
        ON c.[object_id] = tbl.[object_id]  
    WHERE is_masked = 1
        AND tbl.name ='CustomerInfo';

    -- Step:5 Now, let us grant SELECT permission to 'DataAnalystMiami' on the 'CustomerInfo' table.
   GRANT SELECT ON wwi_mcw.CustomerInfo TO DataAnalystMiami;  

    -- Step:6 Logged in as  'DataAnalystMiami' let us execute the select query and view the result.
    EXECUTE AS USER = 'DataAnalystMiami';  
    SELECT * FROM wwi_mcw.CustomerInfo;

    -- Step:7 Let us remove the data masking using UNMASK permission
    GRANT UNMASK TO DataAnalystMiami;
    EXECUTE AS USER = 'DataAnalystMiami';  
    SELECT *
    FROM wwi_mcw.CustomerInfo;
    revert;
    REVOKE UNMASK TO DataAnalystMiami;  

    ----step:8 Reverting all the changes back to as it was.
    ALTER TABLE wwi_mcw.CustomerInfo
    ALTER COLUMN CreditCard DROP MASKED;
    GO
    ALTER TABLE wwi_mcw.CustomerInfo
    ALTER COLUMN Email DROP MASKED;
    GO
    ```

    ![The query tab toolbar is displayed with the Run button selected.](media/querytoolbar_run.png)

3. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

## Exercise 7: Machine Learning

**Duration**: 45 minutes

Azure Synapse Analytics provides a unified environment for both data science and data engineering. What this means in practice, is that your data scientists can train and deploy models using Azure Synapse Analytics and your data engineers can write T-SQL queries that use those models to make predictions against tabular data stored in a SQL Pool database table.

In this lab, you will create several machine learning models and use them to make predictions using the T-SQL `Predict` statement.

For context, the following are the high level steps taken to create a Spark ML based model and deploy it so it is ready for use from T-SQL.

![The process for registering and using a model](media/lab06-machine-learning-process.png "Review model registration process")

All of the steps are performed within your Azure Synapse Analytics Studio.

- Within a notebook, a data scientist will:

  a. Train a model using Spark ML, the machine learning library included with Apache Spark. Models can also be trained using other approaches, including by using Azure Machine Learning automated ML. The main requirement is that the model format must be supported by ONNX.

  b. Convert the model to the ONNX format using the `onnxml` tools.

  c. Save a hexadecimal encoded version of the ONNX model to a table in the SQL Pool database. This is an interim step while this feature is in preview.

- To use the model for making predictions, in a SQL Script a data engineer will:

  a. Read the model into a binary variable by querying it from the table in which it was stored.

  b. Execute a query using the `FROM PREDICT` statement as you would a table. This statement defines both the model to use and the query to execute that will provide the data used for prediction. You can then take these predictions and insert them into a table for use by downstream analytics applications.

> What is ONNX? [ONNX](https://onnx.ai/) is an acronym for the Open Neural Network eXchange and is an open format built to represent machine learning models, regardless of what frameworks were used to create the model. This enables model portability, as models in the ONNX format can be run using a wide variety of frameworks, tools, runtimes and platforms. Think of it like a universal file format for machine learning models.

### Task 1: Training models

1. Open the **ASAMCW - Exercise 6 - Machine Learning** notebook (select **Develop** from the left menu, from the **Develop** menu, expand the **Notebooks** section and select the notebook)
2. Run the notebook step by step (DO NOT `RUN ALL`) to complete this exercise. Some of the most important tasks you will perform are:

- Exploratory data analysis (basic stats)
- Use PCA for dimensionality reduction
- Train ensemble of trees classifier (using XGBoost)
- Train classifier using Auto ML

Please note that each of these tasks will be addressed through several cells in the notebook.

### Task 2: Registering the models with Azure Synapse Analytics

In this task, you will explore the model registration process in Azure Synapse Analytics that enables trained model for use from T-SQL. This task picks up where you left off, with the ONNX model being made available in Azure Storage.

1. The next step is to convert the ONNX model to hexadecimal. The resulting hex encoded model is also upload to Azure Storage. A sample script that performs this functionality is available in [this PowerShell script](https://github.com/microsoft/MCW-Azure-Synapse-Analytics-end-to-end-solution/raw/master/Hands-on%20lab/artifacts/convert-to-hex.ps1). We will execute the following PowerShell script that leverages this functionality. Open the **Cloud Console** and execute the following:

   ```PowerShell
   './Synapse-MCW/Hands-on lab/artifacts/convert-automl-model-to-hex.ps1'
   ```

   You will be prompted for the following information:
    | Prompt |
    |--------|  
    | Enter the name of the resource group containing the Azure Synapse Analytics Workspace |
    | Enter the unique suffix you used in the deployment |

2. From the left menu, select **Data**.

3. Expand the **Databases** section, right click your SQL Pool, `SQLPool01`, and then select **New SQL Script**, and then **Empty script**.

   ![Showing the context menu, selecting New SQL Script, Empty Script](media/lab06-new-sql-script.png "Create new script")

4. Replace the contents of this script with following:

    ```sql
    SELECT
        *
    FROM
        [wwi_mcw].[ASAMCWMLModelExt]
    ```

    The result shows your persisted ONNX model in hexadecimal format:

    ![Persisted ONNX model in hexadecimal format](media/lab06-persisted-model.png)

5. **ASAMCWMLModelExt** is an external table that maps to the data lake location where the trained model was persisted (and then converted to hexadecimal format). Take a moment to read through the code that was used to create the external table (**you don't need to run this code as it was already run during the deployment of your environment**):

    ``` sql
      -- Replace <data_lake_account_key> with the key of the primary data lake account

      CREATE DATABASE SCOPED CREDENTIAL StorageCredential
      WITH
      IDENTITY = 'SHARED ACCESS SIGNATURE'
      ,SECRET = '<data_lake_account_key>';

      -- Create an external data source with CREDENTIAL option.
      -- Replace <data_lake_account_name> with the actual name of the primary data lake account

      CREATE EXTERNAL DATA SOURCE ASAMCWModelStorage
      WITH
      (
          LOCATION = 'wasbs://wwi-02@<data_lake_account_name>.blob.core.windows.net'
          ,CREDENTIAL = StorageCredential
          ,TYPE = HADOOP
      );

      CREATE EXTERNAL FILE FORMAT csv
      WITH (
          FORMAT_TYPE = DELIMITEDTEXT,
          FORMAT_OPTIONS (
              FIELD_TERMINATOR = ',',
              STRING_DELIMITER = '',
              DATE_FORMAT = '',
              USE_TYPE_DEFAULT = False
          )
      );

      CREATE EXTERNAL TABLE [wwi_mcw].[ASAMCWMLModelExt]
      (
      [Model] [varbinary](max) NULL
      )
      WITH
      (
          LOCATION='/ml/onnx-hex' ,
          DATA_SOURCE = ModelStorage ,
          FILE_FORMAT = csv ,
          REJECT_TYPE = VALUE ,
          REJECT_VALUE = 0
      );
      GO

      CREATE TABLE [wwi_mcw].[ASAMCWMLModel]
      (
          [Id] [int] IDENTITY(1,1) NOT NULL,
          [Model] [varbinary](max) NULL,
          [Description] [varchar](200) NULL
      )
      WITH
      (
          DISTRIBUTION = REPLICATE,
          HEAP
      );
      GO
    ```

6. Import the persisted ONNX model in hexadecimal format into the main models table (to be later used with the `PREDICT` function) by executing the following query on **SQLPool01**:

    ```sql
    -- Register the model by inserting it into the table.
    INSERT INTO
        [wwi_mcw].[ASAMCWMLModel]
    SELECT
        Model, 'Product Seasonality Classifier'
    FROM
        [wwi_mcw].[ASAMCWMLModelExt]
    ```

### Task 3: Making predictions with the registered models

In this task, you will author a T-SQL query that uses the previously trained model to make predictions.

> **Note**: In order to make use of the `PREDICT` T-SQL function, your environment will need to be whitelisted. If your environment is not whitelisted, then you should read through the steps of this task as a reference as you will not be able to run the prediction queries.

1. From the left menu, select **Data**.

2. Expand the **Databases** section, right-click your SQL Pool and then select **New SQL Script**, and then **Empty script**.

   ![The database context menu is shown, with New SQL Script and Empty Script selected.](media/lab06-new-sql-script.png "Create new script")

3. Replace the contents of this script with following:

    ```sql
    SELECT TOP 100
        *
    FROM
        [wwi_mcw].[ProductPCA]
    WHERE
        ProductId > 4500
    ```

    This is the input data you will use to make the predictions.

4. Select **Run** from the menubar.

   ![The Run button is selected from the toolbar.](media/lab06-select-run.png "Select Run")

5. Create another new SQL script and replace the contents with the following:

   ```sql
   -- Retrieve the latest hex encoded ONNX model from the table
   DECLARE @model varbinary(max) = (SELECT Model FROM [wwi_mcw].[ASAMCWMLModel] WHERE Id = (SELECT Top(1) max(ID) FROM [wwi_mcw].[ASAMCWMLModel]));

   -- Run a prediction query
   SELECT d.*, p.*
   FROM PREDICT(MODEL = @model, DATA = [wwi_mcw].[ProductPCA] AS d) WITH (prediction real) AS p;
   ```

6. Run the script and view the results, notice that the `Prediction` column is the model's prediction of the `Seasonality` property of each product.

   ![The query results pane is displayed with the prediction results.](media/lab06-view-prediction-results.png "View prediction results")

## Exercise 8: Monitoring

**Duration**: 25 minutes

Azure Synapse Analytics provides a rich monitoring experience within the Azure portal to surface insights regarding your data warehouse workload.

You can monitor active SQL requests using the SQL requests area of the Monitor Hub. This includes details like the pool, submitter, duration, queued duration, workload group assigned, importance, and the request content.

Pipeline runs can be monitored using the Monitor Hub and selecting Pipeline runs. Here you can filter pipeline runs and drill in to view the activity runs associated with the pipeline run and monitor the running of in-progress pipelines.

### Task 1: Workload Importance

Running mixed workloads can pose resource challenges on busy systems. Solution Architects seek ways to separate classic data warehousing activities (such as loading, transforming, and querying data) to ensure that enough resources exist to hit SLAs.

Synapse SQL pool workload management in Azure Synapse consists of three high-level concepts: Workload Classification, Workload Importance and Workload Isolation. These capabilities give you more control over how your workload utilizes system resources.

Workload importance influences the order in which a request gets access to resources. On a busy system, a request with higher importance has first access to resources. Importance can also ensure ordered access to locks.

Setting importance in Synapse SQL for Azure Synapse allows you to influence the scheduling of queries. Queries with higher importance will be scheduled to run before queries with lower importance. To assign importance to queries, you need to create a workload classifier.

1. Navigate to the **Develop** hub.

    ![The Develop menu item is highlighted.](media/develop-hub.png "Develop hub")

2. From the **Develop** menu, select the + button and choose **SQL Script** from the context menu.

    ![The SQL script context menu item is highlighted.](media/synapse-studio-new-sql-script.png "New SQL script")

3. In the toolbar menu, connect to the **SQL Pool** database to execute the query.

    ![The connect to option is highlighted in the query toolbar.](media/synapse-studio-query-toolbar-connect.png "Query toolbar")

4. In the query window, replace the script with the following to confirm that there are NO queries currently being run by users logged in as `asa.sql.workload01`, representing the CEO of the organization or `asa.sql.workload02` representing the data analyst working on the project:

    ```sql
    --First, let's confirm that there are no queries currently being run by users logged in as CEONYC or AnalystNYC.

    SELECT s.login_name, r.[Status], r.Importance, submit_time,
    start_time ,s.session_id FROM sys.dm_pdw_exec_sessions s
    JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
    WHERE s.login_name IN ('asa.sql.workload01','asa.sql.workload02') and Importance
    is not NULL AND r.[status] in ('Running','Suspended')
    --and submit_time>dateadd(minute,-2,getdate())
    ORDER BY submit_time ,s.login_name
    ```

5. Select **Run** from the toolbar menu to execute the SQL command.

    ![The run button is highlighted in the query toolbar.](media/synapse-studio-query-toolbar-run.png "Run")

6. You will flood the system with queries and see what happens for `asa.sql.workload01` and `asa.sql.workload02`. To do this, we'll run a Azure Synapse Pipeline which triggers queries.

7. Select the `Orchestrate` Tab.
8. **Run** the **Exercise 7 - Execute Data Analyst and CEO Queries** Pipeline, which will run / trigger the `asa.sql.workload01` and `asa.sql.workload02` queries. You can run the pipeline with the Debug option if you have an instance of the Integration Runtime running.
9. Select **Add trigger**, then **Trigger now**. In the dialog that appears, select **OK**.

    ![The add trigger and trigger now menu items are highlighted.](media/trigger-data-analyst-and-ceo-queries-pipeline.png "Add trigger")

10. Let's see what happened to all the queries we just triggered as they flood the system. In the query window, replace the script with the following:

    ```sql
    SELECT s.login_name, r.[Status], r.Importance, submit_time, start_time ,s.session_id FROM sys.dm_pdw_exec_sessions s
    JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
    WHERE s.login_name IN ('asa.sql.workload01','asa.sql.workload02') and Importance
    is not NULL AND r.[status] in ('Running','Suspended') and submit_time>dateadd(minute,-4,getdate())
    ORDER BY submit_time ,status
    ```

11. Select **Run** from the toolbar menu to execute the SQL command. You should see an output similar to the following:

    ![SQL query results.](media/sql-query-2-results.png "SQL script")

12. We will give our `asa.sql.workload01` user queries priority by implementing the **Workload Importance** feature. In the query window, replace the script with the following:

    ```sql
    IF EXISTS (SELECT * FROM sys.workload_management_workload_classifiers WHERE name = 'CEO')
    BEGIN
        DROP WORKLOAD CLASSIFIER CEO;
    END
    CREATE WORKLOAD CLASSIFIER CEO
      WITH (WORKLOAD_GROUP = 'largerc'
      ,MEMBERNAME = 'asa.sql.workload01',IMPORTANCE = High);
    ```

13. Select **Run** from the toolbar menu to execute the SQL command.

14. Let's flood the system again with queries and see what happens this time for `asa.sql.workload01` and `asa.sql.workload02` queries. To do this, we'll run an Azure Synapse Pipeline which triggers queries.

    - **Select** the `Orchestrate` Tab

    - **Run** the **Exercise 7 - Execute Data Analyst and CEO Queries** Pipeline, which will run / trigger the `asa.sql.workload01` and `asa.sql.workload02` queries.

15. In the query window, replace the script with the following to see what happens to the `asa.sql.workload01` queries this time:

    ```sql
    SELECT s.login_name, r.[Status], r.Importance, submit_time, start_time ,s.session_id FROM sys.dm_pdw_exec_sessions s
    JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
    WHERE s.login_name IN ('asa.sql.workload01','asa.sql.workload02') and Importance
    is not NULL AND r.[status] in ('Running','Suspended') and submit_time>dateadd(minute,-2,getdate())
    ORDER BY submit_time ,status desc
    ```

16. Select **Run** from the toolbar menu to execute the SQL command. You should see an output similar to the following that shows query executions for the `asa.sql.workload01` user having a **high** importance.

    ![SQL query results.](media/sql-query-4-results.png "SQL script")

17. Navigate to the **Monitor** hub, select **Pipeline runs**, and then select **Cancel recursive** for each running Exercise 7 pipelines. This will help speed up the remaining tasks.

    ![The cancel recursive option is shown.](media/cancel-recursive.png "Pipeline runs - Cancel recursive")

### Task 2: Workload Isolation

Workload isolation means resources are reserved, exclusively, for a workload group. Workload groups are containers for a set of requests and are the basis for how workload management, including workload isolation, is configured on a system. A simple workload management configuration can manage data loads and user queries.

In the absence of workload isolation, requests operate in the shared pool of resources. Access to resources in the shared pool is not guaranteed and is assigned on an importance basis.

Configuring workload isolation should be done with caution as the resources are allocated to the workload group even if there are no active requests in the workload group. Over-configuring isolation can lead to diminished overall system utilization.

Users should avoid a workload management solution that configures 100% workload isolation: 100% isolation is achieved when the sum of `min_percentage_resource` configured across all workload groups equals 100%. This type of configuration is overly restrictive and rigid, leaving little room for resource requests that are accidentally misclassified. There is a provision to allow one request to execute from workload groups not configured for isolation.

1. Navigate to the **Develop** hub.

    ![The Develop menu item is highlighted.](media/develop-hub.png "Develop hub")

2. From the **Develop** menu, select the + button and choose **SQL Script** from the context menu.

    ![The SQL script context menu item is highlighted.](media/synapse-studio-new-sql-script.png "New SQL script")

3. In the toolbar menu, connect to the **SQL Pool** database to execute the query.

    ![The connect to option is highlighted in the query toolbar.](media/synapse-studio-query-toolbar-connect.png "Query toolbar")

4. In the query window, replace the script with the following:

    ```sql
    IF NOT EXISTS (SELECT * FROM sys.workload_management_workload_groups where name = 'CEODemo')
    BEGIN
        Create WORKLOAD GROUP CEODemo WITH  
        ( MIN_PERCENTAGE_RESOURCE = 50        -- integer value
        ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 25 --  
        ,CAP_PERCENTAGE_RESOURCE = 100
        )
    END
    ```

    The code creates a workload group called `CEODemo` to reserve resources exclusively for the workload group. In this example, a workload group with a `MIN_PERCENTAGE_RESOURCE` set to 50% and `REQUEST_MIN_RESOURCE_GRANT_PERCENT` set to 25% is guaranteed 2 concurrency.

5. Select **Run** from the toolbar menu to execute the SQL command.

6. In the query window, replace the script with the following to create a workload Classifier called `CEODreamDemo` that assigns a workload group and importance to incoming requests:

    ```sql
    IF NOT EXISTS (SELECT * FROM sys.workload_management_workload_classifiers where  name = 'CEODreamDemo')
    BEGIN
        Create Workload Classifier CEODreamDemo with
        ( Workload_Group ='CEODemo',MemberName='asa.sql.workload02',IMPORTANCE = BELOW_NORMAL);
    END
    ```

7. Select **Run** from the toolbar menu to execute the SQL command.

8. In the query window, replace the script with the following to confirm that there are no active queries being run by `asa.sql.workload02`:

    ```sql
    SELECT s.login_name, r.[Status], r.Importance, submit_time,
    start_time ,s.session_id FROM sys.dm_pdw_exec_sessions s
    JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
    WHERE s.login_name IN ('asa.sql.workload02') and Importance
    is not NULL AND r.[status] in ('Running','Suspended')
    ORDER BY submit_time, status
    ```

9. Let's flood the system with queries and see what happens for `asa.sql.workload02`. To do this, we will run a Azure Synapse Pipeline which triggers queries. Select the `Orchestrate` Tab. **Run** the **Exercise 7 - Execute Business Analyst Queries** Pipeline, which will run / trigger  `asa.sql.workload02` queries.

10. In the query window, replace the script with the following to see what happened to all the `asa.sql.workload02` queries we just triggered as they flood the system:

    ```sql
    SELECT s.login_name, r.[Status], r.Importance, submit_time,
    start_time ,s.session_id FROM sys.dm_pdw_exec_sessions s
    JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
    WHERE s.login_name IN ('asa.sql.workload02') and Importance
    is not NULL AND r.[status] in ('Running','Suspended')
    ORDER BY submit_time, status
    ```

11. Select **Run** from the toolbar menu to execute the SQL command. You should see an output similar to the following that shows the importance for each session set to `below_normal`:

    ![The script results show that each session was executed with below normal importance.](media/sql-result-below-normal.png "SQL script")

12. Navigate to the **Monitor** hub, select **Pipeline runs**, and then select **Cancel recursive** for each running Exercise 7 pipelines. This will help speed up the remaining tasks.

    ![The cancel recursive option is shown.](media/cancel-recursive-ba.png "Pipeline runs - Cancel recursive")

13. In the query window, replace the script with the following to set 3.25% minimum resources per request:

    ```sql
    IF  EXISTS (SELECT * FROM sys.workload_management_workload_classifiers where group_name = 'CEODemo')
    BEGIN
        Drop Workload Classifier CEODreamDemo
        DROP WORKLOAD GROUP CEODemo
        --- Creates a workload group 'CEODemo'.
            Create  WORKLOAD GROUP CEODemo WITH  
        (MIN_PERCENTAGE_RESOURCE = 26 -- integer value
            ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed more than 4 concurrencies)
        ,CAP_PERCENTAGE_RESOURCE = 100
        )
        --- Creates a workload Classifier 'CEODreamDemo'.
        Create Workload Classifier CEODreamDemo with
        (Workload_Group ='CEODemo',MemberName='asa.sql.workload02',IMPORTANCE = BELOW_NORMAL);
    END
    ```

    > **Note**: Configuring workload containment implicitly defines a maximum level of concurrency. With a CAP_PERCENTAGE_RESOURCE set to 60% and a REQUEST_MIN_RESOURCE_GRANT_PERCENT set to 1%, up to a 60-concurrency level is allowed for the workload group. Consider the method included below for determining the maximum concurrency:
    > 
    > [Max Concurrency] = [CAP_PERCENTAGE_RESOURCE] / [REQUEST_MIN_RESOURCE_GRANT_PERCENT]

14. Let's flood the system again and see what happens for `asa.sql.workload02`. To do this, we will run an Azure Synapse Pipeline which triggers queries. Select the `Orchestrate` Tab. **Run** the **Exercise 7 - Execute Business Analyst Queries** Pipeline, which will run / trigger  `asa.sql.workload02` queries.

15. In the query window, replace the script with the following to see what happened to all of the `asa.sql.workload02` queries we just triggered as they flood the system:

    ```sql
    SELECT s.login_name, r.[Status], r.Importance, submit_time,
    start_time ,s.session_id FROM sys.dm_pdw_exec_sessions s
    JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
    WHERE s.login_name IN ('asa.sql.workload02') and Importance
    is  not NULL AND r.[status] in ('Running','Suspended')
    ORDER BY submit_time, status
    ```

16. Select **Run** from the toolbar menu to execute the SQL command.

### Task 3: Monitoring with Dynamic Management Views

For a programmatic experience when monitoring SQL Analytics via T-SQL, the service provides a set of Dynamic Management Views (DMVs). These views are useful when actively troubleshooting and identifying performance bottlenecks with your workload.

All logins to your data warehouse are logged to `sys.dm_pdw_exec_sessions`. This DMV contains the last 10,000 logins. The `session_id` is the primary key and is assigned sequentially for each new logon.

1. Navigate to the **Develop** hub.

    ![The Develop menu item is highlighted.](media/develop-hub.png "Develop hub")

2. From the **Develop** menu, select the + button and choose **SQL Script** from the context menu.

    ![The SQL script context menu item is highlighted.](media/synapse-studio-new-sql-script.png "New SQL script")

3. In the toolbar menu, connect to the **SQL Pool** database to execute the query.

    ![The connect to option is highlighted in the query toolbar.](media/synapse-studio-query-toolbar-connect.png "Query toolbar")

4. In the query window, replace the script with the following:

    ```sql
    SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
    ```

    All queries executed on SQL pool are logged to `sys.dm_pdw_exec_requests`. This DMV contains the last 10,000 queries executed. The `request_id` uniquely identifies each query and is the primary key for this DMV. The `request_id` is assigned sequentially for each new query and is prefixed with `QID`, which stands for query ID. Querying this DMV for a given `session_id` shows all queries for a given logon.

5. Select **Run** from the toolbar menu to execute the SQL command.

6. Let's flood the system with queries to create operations to monitor. To do this, we will run a Azure Synapse Pipeline which triggers queries. Select the `Orchestrate` Tab. **Run** the **Exercise 7 - Execute Business Analyst Queries** Pipeline, which will run / trigger  `asa.sql.workload02` queries.

7. In the query window, replace the script with the following:

    ```sql
    SELECT *
    FROM sys.dm_pdw_exec_requests
    WHERE status not in ('Completed','Failed','Cancelled')
      AND session_id <> session_id()
    ORDER BY submit_time DESC;
    ```

8. Select **Run** from the toolbar menu to execute the SQL command. You should see a list of sessions in the query results similar to the following. **Note the `Request_ID` of a query** in the results that you would like to investigate (*keep this value in a text editor for a later step*):

    ![Active query results.](media/query-active-requests-results.png "Query results")

9. As an alternative, you can execute the following SQL command to find the top 10 longest running queries.

    ```sql
    SELECT TOP 10 *
    FROM sys.dm_pdw_exec_requests
    ORDER BY total_elapsed_time DESC;
    ```

10. To simplify the lookup of a query in the `sys.dm_pdw_exec_requests` table, use `LABEL` to assign a comment to your query, which can be looked up in the `sys.dm_pdw_exec_requests` view. To test using the labels, replace the script in the query window with the following:

    ```sql
    SELECT *
    FROM sys.tables
    OPTION (LABEL = 'My Query');
    ```

11. Select **Run** from the toolbar menu to execute the SQL command.

12. In the query window, replace the script with the following to filter the results with the label, `My Query`.

    ```sql
    -- Find a query with the Label 'My Query'
    -- Use brackets when querying the label column, as it is a key word
    SELECT  *
    FROM sys.dm_pdw_exec_requests
    WHERE [label] = 'My Query';
    ```

13. Select **Run** from the toolbar menu to execute the SQL command. You should see the previously run query in the results view.

14. In the query window, replace the script with the following to retrieve the query's distributed SQL (DSQL) plan from `sys.dm_pdw_request_steps`. **Be sure to replace** the `QID#####` with the `Request_ID` you noted in Step 8:

    ```sql
    SELECT * FROM sys.dm_pdw_request_steps
    WHERE request_id = 'QID####'
    ORDER BY step_index;
    ```

15. Select **Run** from the toolbar menu to execute the SQL command. You should see results showing the distributed query plan steps for the specified request:

    ![The query results are displayed.](media/sql-dsql-plan-results.png "Query results")

    > When a DSQL plan is taking longer than expected, the cause can be a complex plan with many DSQL steps or just one step taking a long time. If the plan is many steps with several move operations, consider optimizing your table distributions to reduce data movement.

### Task 4: Orchestration Monitoring with the Monitor Hub

1. Let's run a pipeline to monitor its execution in the next step. To do this, select the `Orchestrate` Tab. **Run** the **Exercise 7 - Execute Business Analyst Queries** Pipeline.

    ![The add trigger and trigger now menu items are highlighted.](media/ex7-task4-01.png "Add trigger")

2. Navigate to the `Monitor` hub. Then select **Pipeline runs** to get a list of pipelines that ran during the last 24 hours. Observe the Pipeline status.

    ![The pipeline runs blade is displayed within the Monitor hub.](media/ex7-task4-02.png "Monitor - Pipeline runs")

3. Hover over the running pipeline and select **Cancel** to cancel the execution of the current instance of the pipeline.

    ![The Cancel option is highlighted.](media/ex7-task4-03.png "Cancel")

### Task 5: Monitoring SQL Requests with the Monitor Hub

1. Let's run a pipeline to monitor its execution in the next step. To do this, select the `Orchestrate` Tab. **Run** the **Exercise 7 - Execute Business Analyst Queries** Pipeline.

    ![The add trigger and trigger now menu items are highlighted.](media/ex7-task5-01.png "Add trigger")

2. Navigate to the `Monitor` hub. Then select **SQL requests** to get a list of SQL requests that ran during the last 24 hours.

3. Select the **Pool** filter and select your SQL Pool. Observe the `Request Submitter`, `Submit Time`, `Duration`, and `Queued Duration` values.

    ![The SQL requests blade is displayed within the Monitor hub.](media/ex7-task5-02.png "Monitor - SQL requests")

4. Hover onto a SQL Request log and select `Request Content` to access the actual T-SQL command executed as part of the SQL Request.

    ![The request content link is displayed over a SQL request.](media/ex7-task5-03.png "SQL requests")
  
## After the hands-on lab

**Duration**: 5 minutes

### Task 1: Delete the resource group

1. In the Azure Portal, open the resource group for this lab. Select **Delete** from the top toolbar menu.

2. Open the Cloud Shell and issue the following command to remove the lab files:

   ```PowerShell
   Remove-Item -Path .\Synapse-MCW -recurse -force
   Remove-Item -Path .\modelconversion -recurse -force
   ```

You should follow all steps provided *after* attending the Hands-on lab.
