# Lab 6 – Automating with Synapse Pipelines

<img src="images/lab06/media/image1.png" style="width:3.9375in;height:0.63819in" />
<br/>
<img src="images/lab06/media/image2.png" style="width:3.48973in;height:1.20479in" />

#  

Contents

# 

[Lab Overview](#lab-overview)

[Introduction](#introduction)

[Objectives](#objectives)

[Exercise 1: Orchestrate the refresh and transformation of the customer
charges
file](#exercise-1-orchestrate-the-refresh-and-transformation-of-the-customer-charges-file)

- [Step 1: Open Synapse Studio in a web browser](#step-1-open-synapse-studio-in-a-web-browser)

- [Step 2: Download the sample charge detail file](#step-2-download-the-sample-charge-detail-file)

- [Step 3: Automate the data integration and transformation for customer charges](#step-3-automate-the-data-integration-and-transformation-for-customer-charges)

[Exercise 2: Automate the rerun of your machine learning
pipeline](#exercise-2-automate-the-rerun-of-your-machine-learning-pipeline)

- [Step 1: Setup a Synapse pipeline which runs your machine learning pipeline](#step-1-setup-a-synapse-pipeline-which-runs-your-machine-learning-pipeline)

- [Step 2: Schedule the Synapse pipeline](#step-2-schedule-the-synapse-pipeline)

- [Step 3: Schedule the data source refresh in Customer Insights](#step-3-schedule-the-data-source-refresh-in-customer-insights)

[Exercise 3: Cleanup Azure resources (optional)](#exercise-3-cleanup-azure-resources-optional)

[Summary](#summary)

# 

# Lab Overview

## Introduction

This hands on lab will automate the refresh and transformation of a key
customer charges file with Synapse pipelines and Mapping data flows. It
will then automate the execution of the Azure ML pipeline using Synapse
pipelines and how to automate the refresh of data into Customer
Insights.

## Objectives

The objectives of this exercise are to:

-   Learn how to utilize Synapse pipelines and Mapping data flows to do
    data integration and transformations with a drag-and-drop interface

-   Learn how Synapse pipelines can schedule a machine learning to rerun

-   Learn how to schedule the refresh of data into Customer Insights

The estimated time for this lab is 45 minutes

# Exercise 1: Orchestrate the refresh and transformation of the customer charges file

In this section you will perform some data integration and data
transformation tasks to refresh the customer charges summary file from a
more detailed file. You will use Synapse pipelines and Mapping data
flows to accomplish this.

## Step 1: Open Synapse Studio in a web browser

Go to <https://portal.azure.com> and sign in with your organizational
account.

In the search box at the top of the portal, search for “asaworkspace”
and click on the Synapse workspace (not the SQL Server) which appears
under the Resources section.

<img src="images/lab06/media/image3.png" style="width:4.90693in;height:4.15683in" alt="Graphical user interface, text, application, email Description automatically generated" />

On the Overview blade and the Essentials section, click the Workspace
web URL link to open Synapse Studio.

## Step 2: Download the sample charge detail file

1.  Right click on this
    [CustomerChargesDetail.csv](https://raw.githubusercontent.com/ArtisConsulting/customer-insights-azure-data-workshop/main/SampleData/CustomerChargesDetail.csv)
    link and choose “Save link as…” and name the file
    **CustomerChargesDetail.csv** (not CustomerChargesDetail.txt) on your workstation. We will now upload this
    file to Azure Data Lake Storage Gen2 (ADLS).

1.  Click on the Data pane on the left. It is the
    <img src="images/lab06/media/image4.png" style="width:0.28129in;height:0.36463in" />
    icon. Then go to the Linked tab. Expand Azure Data Lake Storage Gen2 and
    click on the default storage account (asaworkspace\<suffix>) and click
    on the staging container.

    <img src="images/lab06/media/image5.png" style="width:3.50049in;height:3.6776in" alt="Graphical user interface, text, application Description automatically generated" />

1.  Click the “+ New folder” button and create a new folder called
    **ChargesDetail**.

    <img src="images/lab06/media/image6.png" style="width:7.5in;height:2.05208in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Double click the new ChargesDetail folder. Then click the Upload button:

    <img src="images/lab06/media/image7.png"  />

1.  Choose the CustomerChargesDetail.csv file you previously downloaded and
    click Upload.

    <img src="images/lab06/media/image8.png" style="width:6.3238in;height:3.06293in" alt="Graphical user interface, text, application, email Description automatically generated" />

    For sake of this hands-on-lab, we will pretend that this
    CustomerChargesDetail.csv file is being updated and uploaded to this
    location daily by your accounting system.

## Step 3: Automate the data integration and transformation for customer charges

Let’s create a Synapse pipeline which takes the
CustomerChargesDetail.csv file and transforms it using a Mapping data
flow into the CustomerCharges.csv file we use in other labs. Mapping
data flows are a visually-designed data transformations. A mapping data
flow is executed under the covers on highly scalable Spark compute.

1.  Go to the Integrate tab (the pipeline icon) in Synapse Studio:

    <img src="images/lab06/media/image9.png" style="width:3.89638in;height:3.75052in" alt="Graphical user interface, application, Word Description automatically generated" />

1.  Click the + sign and choose Pipeline to create a new Synapse pipeline.

    <img src="images/lab06/media/image10.png" style="width:4.79234in;height:2.36491in" alt="Graphical user interface, text, application, Word Description automatically generated" />

1.  In the Properties pane on the right, rename the pipeline and provide a
    description:

    <img src="images/lab06/media/image11.png" style="width:3.03167in;height:2.36491in" alt="Graphical user interface, text, application Description automatically generated" />

1.  In the Activities pane search for “data flow” and drag the
    **Data flow** module onto the canvas.

    <img src="images/lab06/media/image12.png" style="width:7.31352in;height:2.48993in" alt="Graphical user interface, application Description automatically generated" />

1.  With the Data flow module selected, view the General tab at the bottom
    of the screen and rename the module:

    <img src="images/lab06/media/image13.png" style="width:5.82373in;height:1.85443in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  On the Settings tab, click the +New button to create a new Mapping data
    flow:

    <img src="images/lab06/media/image14.png" style="width:5.66746in;height:2.16697in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  In the properties pane rename to “dfRefreshCustomerCharges” (“df” stands
    for data flow):

    <img src="images/lab06/media/image15.png" style="width:3.06293in;height:2.48993in" alt="Graphical user interface, text, application Description automatically generated" />

1.  Click the “Add source” box:

    <img src="images/lab06/media/image16.png" style="width:3.93805in;height:1.97944in" alt="Application Description automatically generated with medium confidence" />

1.  In the “Source settings” tab at the bottom name it
    **CustomerChargesDetail**, change to an **inline** source type, choose a
    **DelimitedText** dataset type, and choose the
    “**asaworkspace\<suffix>-WorkspaceDefaultStorage**” linked service
    (which was created automatically when the Synapse workspace was
    created).

    <img src="images/lab06/media/image17.png" style="width:7.5in;height:3.22153in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  On the “Source options” tab click the Browse button on the File path:

    <img src="images/lab06/media/image18.png" style="width:7.5in;height:1.67292in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Browse to staging/ChargesDetail and select CustomerChargesDetail.csv and
    click OK:

    <img src="images/lab06/media/image19.png" style="width:3.27129in;height:1.68774in" alt="Graphical user interface, application Description automatically generated" />

1.  Check the “first row as header” checkbox:

    <img src="images/lab06/media/image20.png" style="width:7.5in;height:4.96111in" alt="Graphical user interface, application Description automatically generated" />

1.  On the Projection tab, notice the “Import schema” button is grayed out.
    Click the “Data flow debug” toggle at the top of the mapping data flow
    in order to spin up debug compute to assist in developing and debugging
    this mapping data flow.

    <img src="images/lab06/media/image21.png" style="width:6.69885in;height:3.77136in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Accept the defaults and click OK. The debug time to live of 1 hour means
    that the compute will auto-pause after 1 hour of inactivity in order to
    reduce your Azure cost.

    <img src="images/lab06/media/image22.png" style="width:3.07335in;height:3.50049in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Wait about 2-5 minutes until the data flow debug compute spins up. When
    complete you will get a notification in the top right and there will be
    a green checkmark next to the Data flow debug toggle. If you step away
    for more than an hour, the toggle debug compute will auto-pause and you
    will have to click the toggle again manually.

    <img src="images/lab06/media/image23.png" style="width:3.63592in;height:0.80219in" alt="A picture containing text Description automatically generated" />

    <img src="images/lab06/media/image24.png" style="width:1.86484in;height:0.30213in" />

1.  Click the Import schema button (which was grayed out in step 13) now. Accepting the default formats is
    adequate for our file. Click the Import button at the bottom to accept the defaults:

    <img src="images/lab06/media/image25.png" style="width:6.40714in;height:5.01112in" alt="Table Description automatically generated" />

1.  Wait a few seconds for the schema to be loaded. Now the schema for this CustomerChargesDetail source is populated.

    <img src="images/lab06/media/image26.png" style="width:7.5in;height:2.86389in" alt="Graphical user interface, application Description automatically generated" />

1.  Go to the **Data preview** tab and click the **Refresh** button to see a
    preview of a few rows of this dataset.

    <img src="images/lab06/media/image27.png" style="width:7.5in;height:2.24792in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  This file has one row per service per customer per month. We want to
    perform two data transformation tasks. First, we want to sum together
    the Charge and Tax columns. Second, we want to summarize all the
    services together into a single total charge per customer per
    ChargeDate.

    Click the + sign next to the CustomerChargesDetail source and choose
    Derived Column

    <img src="images/lab06/media/image28.png" style="width:5.0007in;height:5.90707in" alt="Graphical user interface, application Description automatically generated" />

1.  Describe what you are doing in the Output stream name, set the column
    name to TotalCharges, and set the expression to Charge + Tax (you can
    click the “Open expression builder” button for help with building more
    complex expressions.

    <img src="images/lab06/media/image29.png" style="width:7.5in;height:2.08056in" alt="Graphical user interface, text, application Description automatically generated" />

1.  Click the + sign next to the SumChargesAndTax transform and add
    Aggregate:

    <img src="images/lab06/media/image30.png" style="width:7.5in;height:4.85347in" alt="Graphical user interface Description automatically generated with medium confidence" />

1.  Name the output stream AggregateByCustomerMonth (there is only one
    ChargeDate per month in the file), add customerID and ChargeDate
    (clicking the + sign next to customerID to add the second column to the
    Group by section:

    <img src="images/lab06/media/image31.png" style="width:7.5in;height:5.4875in" alt="Graphical user interface Description automatically generated" />

1.  Then click the “Aggregates” selector and add a TotalCharges column with
    expression sum(TotalCharges) which will aggregate the TotalCharges
    derived column to the per customer/ChargeDate grain.

    <img src="images/lab06/media/image32.png" style="width:7.5in;height:3.18681in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Click the + sign next to the AggregateByCustomerMonth and choose
    **Select**:

    <img src="images/lab06/media/image33.png" style="width:5.19864in;height:4.6569in" alt="Graphical user interface, application Description automatically generated" />

1.  The column order looks like this to begin with:

    <img src="images/lab06/media/image34.png" style="width:7.5in;height:1.19931in" alt="Graphical user interface, application Description automatically generated" />

1.  Rename the TotalCharges column to just Charge and mouse over that row
    until you see the six dots to the left of the row and drag it to reorder
    the columns. The columns should be in the order customerID, Charge,
    ChargeDate since this is the file format expected downstream.

    <img src="images/lab06/media/image35.png" style="width:7.5in;height:1.11319in" alt="Graphical user interface, application Description automatically generated" />

1.  Go to the Data preview tab on the Select1 module and click Refresh.
    Validate that the columns are in the order matching this screenshot and
    the column names match exactly:

    <img src="images/lab06/media/image36.png" style="width:7.34477in;height:3.7922in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Now let’s write out the results. Click the + icon next to the Select1
    module and search for “sink” and choose Sink:

    <img src="images/lab06/media/image37.png" style="width:4.93819in;height:2.13572in" alt="A picture containing funnel chart Description automatically generated" />

1.  Name the output stream CustomerCharges, choose an Inline sink type,
    choose a DelimitedText dataset type, choose the
    “asaworkspace\<suffix>-WorkspaceDefaultStorage” linked service:

    <img src="images/lab06/media/image38.png" style="width:7.5in;height:3.20972in" alt="Graphical user interface, text, application Description automatically generated" />

1.  Choose the staging container and the Charges folder. Check the “first
    row as header” checkbox and the “Clear the folder” checkbox:

    <img src="images/lab06/media/image39.png" style="width:6.31338in;height:5.51119in" alt="Graphical user interface, application Description automatically generated" />

1.  On the Optimize tab of the CustomerCharges sink you can keep the current
    “use current partitioning” setting as this will output multiple files in
    a highly parallel manner and generally perform better. The downstream
    Synapse Serverless SQL view called dbo.CustomerChurnCharges has been
    coded to read all files in the folder rather than looking for a specific
    filename. (If your scenario requires a single filename then choose “Single
    partition” but realize that will hurt performance.)

    <img src="images/lab06/media/image40.png" style="width:6.49049in;height:1.05223in" alt="Graphical user interface, text, application, Word Description automatically generated" />

1.  Flip back to the RefreshCustomerCharges pipeline tab, click the Debug
    dropdown and choose “Use data flow debug session”:

    <img src="images/lab06/media/image41.png" style="width:7.5in;height:2.41181in" alt="Graphical user interface, text, application, chat or text message Description automatically generated" />

1.  Go to the Data left nav, the Linked tab, choose the primary storage
    account, choose the staging container, browse to the Charges folder and
    you should see about 200 CSV files which are about 38KB each (compared
    to the original 7MB CustomerCharges.csv file):

    <img src="images/lab06/media/image42.png" style="width:7.5in;height:2.71875in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Click the Publish all button to save the pipeline and mapping data flow
    and make them available to schedule or for other users to see.

    <img src="images/lab06/media/image43.png" style="width:4.1985in;height:0.38547in" />

1.  Confirm the list of objects to publish and click the Publish button at
    the bottom:

    <img src="images/lab06/media/image44.png" style="width:5.55286in;height:2.77122in" alt="Graphical user interface, text, application, email Description automatically generated" />

# Exercise 2: Automate the rerun of your machine learning pipeline

In this section you will automate the execution of your machine learning
pipeline. There are several ways to automate and operationalize your
model. One option is to publish a batch inference pipeline which reuses
a trained model to make more predictions (inferences). A second option is
to automate the execution of the entire pipeline which retrains models
and scores the dataset with each execution. We will use this
second option in this lab.

## Step 1: Setup a Synapse pipeline which runs your machine learning pipeline

1.  On the Integrate left nav, open the RefreshCustomerChargesAndChurnML
    pipeline, search for “machine” in the Activities panel, and drag
    **Machine Learning Execute Pipeline** onto the canvas:

    <img src="images/lab06/media/image45.png" style="width:7.5in;height:2.02222in" alt="Graphical user interface Description automatically generated" />

1.  Drag the green square from the Refresh Customer Charges data flow module
    to the Machine learning module. This sets a dependency to ensure the
    refresh of the charges file completes successfully before the machine
    learning pipeline is run:

    <img src="images/lab06/media/image46.png" style="width:4.92777in;height:1.10432in" alt="A picture containing waterfall chart Description automatically generated" />

1.  Click on the ML module and rename it Execute Churn ML Pipeline:

    <img src="images/lab06/media/image47.png" style="width:5.91749in;height:1.12516in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  On the Settings tab for the ML module, click the new button to create a
    new Azure ML linked service:

    <img src="images/lab06/media/image48.png" style="width:5.95916in;height:1.03139in" alt="Graphical user interface, text, application Description automatically generated" />

1.  Name the linked service “lsAzureML” (the “ls” prefix stands for Linked
    Service), ensure the authentication is the (Synapse) Managed Identity
    (which was granted RBAC Contributor role on the Azure ML workspace in
    the ARM template), then choose the right Azure subscription and the
    “amlworkspace\<suffix>” Azure ML workspace.

    <img src="images/lab06/media/image49.png" style="width:6.12586in;height:5.68829in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Click “Test connection” and you should see a “Connection successful”
    message. Click Create.

    <img src="images/lab06/media/image50.png" style="width:6.59467in;height:0.84387in" alt="A picture containing graphical user interface Description automatically generated" />

1.  Change the Pipeline ID type to “Pipeline endpoint ID”, choose the
    CustomerChurnEP endpoint, choose the pipeline version you want (possibly
    version 0), and set the Experiment name to SynapseAutomatedChurn:

    <img src="images/lab06/media/image51.png" style="width:7.5in;height:3.54375in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Click the Debug button at the top of the
    RefreshCustomerChargesAndChurnML pipeline. After 15 minutes or so the
    debug run should complete successfully. (Tip: If you don’t see the
    Output tab, click on the canvas background rather than on a module in
    the pipeline.) Mouse over the Execute Churn ML activity row and click
    the eyeglasses icon to see more details about that execution. Click the
    link to see details from the Azure ML pipeline execution.

    <img src="images/lab06/media/image52.png" style="width:7.5in;height:3.33472in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Click Publish all:

    <img src="images/lab06/media/image53.png" style="width:4.13599in;height:0.30213in" />

1.  Confirm the two objects which are changed:

    <img src="images/lab06/media/image54.png" style="width:6.34464in;height:2.80247in" alt="Graphical user interface, text, application, email Description automatically generated" />

## Step 2: Schedule the Synapse pipeline

1.  Click the Add trigger button at the top of the
    RefreshCustomerChargesAndChurnML Synapse pipeline and choose New/Edit:

    <img src="images/lab06/media/image55.png" style="width:7.5in;height:2.19306in" alt="Graphical user interface, text, application Description automatically generated" />

1.  On the Add triggers pane, choose +New from the dropdown:

    <img src="images/lab06/media/image56.png" style="width:6.34464in;height:1.64606in" alt="Graphical user interface, application Description automatically generated" />

1.  Name the trigger Daily3am. Set the Start date to 3:00AM tomorrow. Choose
    your time zone. Change the recurrence to every 1 day. Then click OK two
    times.

    <img src="images/lab06/media/image57.png" style="width:6.45923in;height:9.1992in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  Click Publish all and confirm the trigger to be published. Click
    Publish.

    <img src="images/lab06/media/image58.png" style="width:5.6362in;height:2.35449in" alt="Graphical user interface, text, application, email Description automatically generated" />

1.  If you ever want to adjust the trigger, go to the Manage left nav,
    Triggers left nav. Mouse over the Daily3am trigger and click the start
    or stop icon to toggle whether this trigger is active. (If there was an
    error activating the trigger during publishing, you can retry
    starting/activating the trigger on this screen.)

    <img src="images/lab06/media/image59.png" style="width:7.5in;height:3.56042in" alt="Graphical user interface, text, application Description automatically generated" />

## Step 3: Schedule the data source refresh in Customer Insights

1.  At <https://home.ci.ai.dynamics.com/> on the Data… Data sources left
    nav, click the … next to AzureMlResults and Edit refresh settings.

    <img src="images/lab06/media/image60.png" style="width:7.5in;height:3.71181in" alt="Graphical user interface, text, application Description automatically generated" />

1.  Choose “Refresh automatically” and set it to refresh on specific days
    and times, choose Daily, and choose a time which is 30 minutes after the
    Synapse pipeline is scheduled to run. Pay attention to the time zone.
    Click Create.

    <img src="images/lab06/media/image61.png" style="width:7.5in;height:2.79653in" alt="Graphical user interface, text, application Description automatically generated" />

# Exercise 3: Cleanup Azure resources (optional)

When you are **COMPLETELY DONE** with all your work, you can optionally
delete these Azure resources. Go to the Overview blade of the
**customer-insights-workshop-rg** in the Azure portal and click the
**Delete resource group** button. Your Azure resources and all the data
in them will be **deleted and unrecoverable**.

<img src="images/lab06/media/image62.png" style="width:7.29268in;height:2.3024in" alt="Graphical user interface, application Description automatically generated" />

# Summary

In this lab, you learned how to automate the refresh and transformation
of a key customer charges file with Synapse pipelines and Mapping data
flows. Then you learned how to automate the execution of the Azure ML
pipeline using Synapse pipelines and how to automate the refresh of data
into Customer Insights.
