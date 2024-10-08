# Lab-04 : Create and use a Dataflow (Gen2) in Microsoft Fabric 

## Overview 
Microsoft Fabric offers a unified solution for data engineering, integration, and analytics. A crucial step in end-to-end analytics is data ingestion. Dataflows (Gen2) are used to ingest and transform data from multiple sources, and then land the cleansed data to another destination. They can be incorporated into data pipelines for additional data movement, and used as a data source in Power BI.

In our scenario, you need to develop a data model that can standardize the data and provide access to the business. By using Dataflows (Gen2), you can connect to the various data sources, and then prep and transform the data. To allow access, you can land the data directly into your Lakehouse or use a data pipeline for other destinations.

## _Architecture Diagram_

![Architecture Diagram](./Images/Create-and-use-a-Dataflow(Gen2).png)

## Ingest Data with Dataflows Gen2 in Microsoft Fabric

In Microsoft Fabric, Dataflows (Gen2) connect to various data sources and perform transformations in Power Query Online. They can then be used in Data Pipelines to ingest data into a lakehouse or other analytical store, or to define a dataset for a Power BI report.

This lab is designed to introduce the different elements of Dataflows (Gen2), and not create a complex solution that may exist in an enterprise. This lab takes **approximately 30 minutes** to complete.

> **Note**: You'll need a Microsoft Fabric license to complete this exercise. Complete the previous task to proceed further.

## Task 1: Create a lakehouse

Now that you have a workspace, it's time to switch to the **Data Engineering** experience in the portal and create a data lakehouse into which you'll ingest data.

1. At the bottom left of the Power BI portal, select the **Data Engineering** icon and click to the **Data Engineering** experience.

2. In the **Data engineering** home page, create a new **Lakehouse** with a name of **dp_lakehouse**.

    After a minute or so, a new empty lakehouse will be created.

   ![New lakehouse.](./Images/m6-fabric-1.png)

## Task 2: Create a Dataflow (Gen2) to ingest data

Now that you have a lakehouse, you need to ingest some data into it. One way to do this is to define a dataflow that encapsulates an *extract, transform, and load* (ETL) process.

1. In the home page for your workspace, select **New Dataflow Gen2**. After a few seconds, the Power Query editor for your new dataflow opens as shown here.

   ![New dataflow.](./Images/powermodel7.png)

2. Select **Import from a Text/CSV file**, and create a new data source with the following settings:
 - **Link to file**: *Selected*
 - **File path or URL**: `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv`
 - **Connection**: Create new connection
 - **data gateway**: (none)
 - **Authentication kind**: Anonymous

   ![New dataflow.](./Images/powermodel8.png)

   ![New dataflow.](./Images/powermodel9.png)

3. Select **Next** to preview the file data, and then **Create** the data source. The Power Query editor shows the data source and an initial set of query steps to format the data, as shown here:

   ![Query in the Power Query editor.](./Images/m6-fabric-3.png)

4. On the toolbar ribbon, select the **Add column** tab. Then select **Custom column**, create a new column named **MonthNo** that contains a number based on the formula `Date.Month([OrderDate])`and  click on **OK**. The step to add the custom column is added to the query and the resulting column is displayed in the data pane - as shown here:

   ![New dataflow.](./Images/powermodel10.png)

   ![New dataflow.](./Images/powermodel11.png)

   ![Query with a custom column step.](./Images/custom-column-added1.png)

> **Tip:** In the Query Settings pane on the right side, notice the **Applied Steps** include each transformation step. At the bottom, you can also toggle the **Diagram flow** button to turn on the Visual Diagram of the steps.
>
> Steps can be moved up or down, edited by selecting the gear icon, and you can select each step to see the transformations apply in the preview pane.

## Task 3: Add data destination for Dataflow

1. On the toolbar ribbon, select the **Home** tab. Then in the **Add data destination** drop-down menu, select **Lakehouse**.

   ![New dataflow.](./Images/powermodel12.png)

   > **Note:** If this option is grayed out, you may already have a data destination set. Check the data destination at the bottom of the Query settings pane on the right side of the Power Query editor. If a destination is already set, you can change it using the gear.

2. In the **Connect to data destination** dialog box, edit the connection and sign in using your Power BI organizational account to set the identity that the dataflow uses to access the lakehouse.

   ![New dataflow.](./Images/powermodel13.png)

   ![Data destination configuration page.](./Images/dataflow-connection1.pn)

3. Select **Next** and in the list of available workspaces, find your workspace and select the lakehouse **df_datahouse**. Then specify a new table named **orders**:

   ![New dataflow.](./Images/powermodel14.png)

   > **Note:** On the **Destination settings** page, notice how MonthNo are not selected in the Column mapping and there is an informational message: *Change to date/time*.

    ![Data destination settings page.](./Images/destination-settings1.png)

4. Cancel this action, then go back to OrderDate and MonthNo columns in Power Query online. Right-click on the column header and **Change Type**.

    - OrderDate = Date/Time
    - MonthNo = Whole number

   ![New dataflow.](./Images/powermodel15.png)

   ![New dataflow.](./Images/powermodel16.png)

6. Now repeat the process outlined earlier to add a lakehouse destination.

7. On the **Destination settings** page, select **Append**, and then save the settings.  The **Lakehouse** destination is indicated as an icon in the query in the Power Query editor.

   ![New dataflow.](./Images/powermodel17.png)

8. Select **Publish** to publish the dataflow. Then wait for the **Dataflow 1** dataflow to be created in your workspace.

9. Once published, you can right-click on the dataflow in your workspace, select **Properties**, and rename your dataflow.

## Task 4: Add a dataflow to a pipeline

You can include a dataflow as an activity in a pipeline. Pipelines are used to orchestrate data ingestion and processing activities, enabling you to combine dataflows with other kinds of operation in a single, scheduled process. Pipelines can be created in a few different experiences, including Data Factory experience.

1. From your Fabric-enabled workspace, make sure you're still in the **Data Engineering** experience. Select **New**, **Data pipeline**, then when prompted, create a new pipeline named **Load data**.

   The pipeline editor opens.

   ![New dataflow.](./Images/powermodel18.png)

   ![New dataflow.](./Images/powermodel19.png)


   > **Tip**: If the Copy Data wizard opens automatically, close it!

2. Select **pipeline activity**, and add a **Dataflow** activity to the pipeline.

   ![New dataflow.](./Images/powermodel20.png)

   ![New dataflow.](./Images/powermodel21.png)


3. With the new **Dataflow1** activity selected, on the **Settings** tab, in the **Dataflow** drop-down list, select **Dataflow 1** (the data flow you created previously)

   ![New dataflow.](./Images/powermodel22.png)


4. On the **Home** tab, save the pipeline using the **&#128427;** (*Save*) icon. provide name to your dataflow and click on **Save.**

   ![New dataflow.](./Images/powermodel23.png)

5. Use the **&#9655; Run** button to run the pipeline, and wait for it to complete. It may take a few minutes.

   ![Pipeline with a dataflow that has completed successfully.](./Images/dataflow-pipeline-succeeded1.png)

6. In the menu bar on the left edge, select your lakehouse.

   ![New dataflow.](./Images/powermodel24.png)

7. In the **...** menu for **Tables**, select **refresh**. Then expand **Tables** and select the **orders** table, which has been created by your dataflow.

   ![Table loaded by a dataflow.](./Images/loaded-table1.png)

> **Tip**: Use the Power BI Desktop *Dataflows connector* to connect directly to the data transformations done with your dataflow.
>
> You can also make additional transformations, publish as a new dataset, and distribute with intended audience for specialized datasets.
>
>![Power BI data source connectors](Images/pbid-dataflow-connectors1.png)

## Clean up resources

If you've finished exploring dataflows in Microsoft Fabric, you can delete the workspace you created for this exercise.

1. Navigate to Microsoft Fabric in your browser.
1. In the bar on the left, select the icon for your workspace to view all of the items it contains.
1. In the **...** menu on the toolbar, select **Workspace settings**.
1. In the **Other** section, select **Remove this workspace**.
1. Don't save the changes to Power BI Desktop, or delete the .pbix file if already saved.

## **Congratulations! you have successfully completed this lab**
