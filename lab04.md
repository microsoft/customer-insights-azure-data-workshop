# Lab 4 - Predicting Customer Churn in Azure Machine Learning

<img src="images/lab04/media/image1.png" style="width:3.9375in;height:0.63819in" />
<br/>
<img src="images/lab04/media/image2.png" style="width:3.48973in;height:1.20479in" alt="Microsoft partner logo" />

# Contents

# 

[Introduction](#introduction)

[Objectives](#objectives)

[Step 1: Sign into Azure ML Studio and choose your workspace](#step-1-sign-into-azure-ml-studio-and-choose-your-workspace)

[Step 2: Create Data Connection and Dataset](#step-2-create-data-connection-and-dataset)

[Step 3: Experiment Design](#step-3-experiment-design)

[Step 4: Training models](#step-4-training-models)

[Step 5: Output Model](#step-5-output-model)

# 

# Lab Overview

## Introduction

The purpose of this lab is to allow you to get hands-on experience
creating an Azure Machine Learning model to predict which
telecommunications customers are likely to churn (i.e. to stop using the
service or move to a competitor). The output of this model will allow a
targeted intervention to try to retain these customers since retaining
existing customers is more cost effective than gaining new customers.

## Objectives

The objectives of this exercise are to:

-   Sign into Azure ML

-   Create a datastore connected to Azure Synapse Serverless SQL

-   Create a dataset from the datastore

-   Learn how to prepare the data for machine learning

-   Learn how to perform classification to predict whether a subscriber
    will churn.

-   Learn how to train and evaluate a model

-   Learn how to compare the accuracy of several models.

-   Export the results of the models to Blob Storage

Estimated time for this lab is 75 minutes


# Exercise 1: Creating a Telco Churn Model

This exercise uses an anonymized telecommunications dataset about
customers to predict which customers will cancel their subscription. In
the telecommunications industry, a subscriber cancelling is called
“churn.”

## Step 1: Sign into Azure ML Studio and choose your workspace

In this step, you will sign into Azure ML Studio and choose which
workspace you will use for creating experiments.

1.  Navigate to [ml.azure.com](https://ml.azure.com/) in your browser.
    If you’re already signed in, you will be directed to the main Azure
    ML screen. If not, you will be asked to sign in.

    <img src="images/lab04/media/image3.png" style="width:3.43333in;height:3.19582in" /><img src="images/lab04/media/image4.png" style="width:3.43085in;height:3.18256in" alt="Graphical user interface, application Description automatically generated" />

2.  Select your “amlworkspace\<suffix>” Azure ML workspace you created
    in Lab 2 from the drop-down menu. Click the “Get started” button.
    You will be directed to the Azure ML home screen.

    <img src="images/lab04/media/image5.png" style="width:5.62565in;height:3.34167in" alt="Graphical user interface, application, email Description automatically generated" />

3.  In the top right, there is a directory and workspace selection.
    Ensure you are in the desired directory, subscription and workspace.

    <img src="images/lab04/media/image6.png" style="width:3.23938in;height:3.79583in" alt="Graphical user interface, application Description automatically generated" />

## Step 2: Create Data Connection and Dataset

In this step, you connect to Azure Synapse Serverless SQL using the
Azure SQL database datastore type and perform a query to extract the
data.

1.  To connect to the data. Click the **Datastores** option on the left
    pane.

    <img src="images/lab04/media/image7.png" style="width:1.24167in;height:3.51806in" alt="Graphical user interface, application Description automatically generated" />

2.  Choose **+ New Datastore**

    <img src="images/lab04/media/image8.png" style="width:7.15341in;height:1.8875in" alt="Graphical user interface, text, application Description automatically generated" />

3.  Enter the details of the new datastore. Name the Datastore
    “**telcochurn**” and select “**Azure SQL Database**” as the
    Datastore type. The server name will be
    “**asaworkspace\<suffix>-ondemand**”. The Database name is “CI”.
    Select the proper Subscription ID and Resource Group assigned to
    your project. The User ID will be “**asa.sql.admin**”. Enter the
    password you created in lab 2. Select “No” on using the workspace
    managed identity for data preview. Once done, click “**Create**” at
    the bottom. *Note: Even though the documented connection string for
    Synapse Serverless SQL is
    asaworkspace\<suffix>-ondemand.**sql.azuresynapse.net**,
    asaworkspace\<suffix>-ondemand.**database.windows.net** also works.*


    <img src="images/lab04/media/image9.png" style="width:3.71875in;height:5in" />

1.  Upon creation, “telcochurn” will appear in the datastore
    selection, click on it to create a new dataset.

    <img src="images/lab04/media/image10.png" style="width:5.44253in;height:2.07917in" alt="Graphical user interface, text, application, email Description automatically generated" />

2.  Select **Create Dataset** from the Overview screen

    <img src="images/lab04/media/image11.png" style="width:4.22441in;height:1.74583in" alt="Graphical user interface, text, application, email Description automatically generated" />

3.  Give the dataset the name **TelcoChurn** and select Dataset Type
    as **Tabular**. Click **Next**.

    <img src="images/lab04/media/image12.png" style="width:3.63743in;height:1.6833in" alt="Graphical user interface, text, application, email Description automatically generated" />

4.  Enter the following SQL Query into the space provided:
    **select \* from dbo.CustomerChurn**. Leave Skip Data Validation
    **UNCHECKED** then click Next. This will connect to the data and
    pop up with a data preview screen. After the preview has loaded,
    click **Next**. *Note: we could’ve also created the Datastore on
    this screen.*

    <img src="images/lab04/media/image13.png" style="width:3.52049in;height:2.48333in" alt="Table Description automatically generated" />

5.  The next options are to set the Schema. Here we can set the data
    types of each column. Azure ML will automatically detect what it
    perceives the data type to be. We can accept the defaults and
    click **Next**.

    <img src="images/lab04/media/image14.png" style="width:6.16101in;height:1.85833in" alt="Graphical user interface, application Description automatically generated" />

6.  The final step for creating the dataset is to Confirm the
    details. Review the screen and click **Create** at the bottom of
    the page.

    <img src="images/lab04/media/image15.png" style="width:6.90813in;height:1.19167in" alt="Table Description automatically generated" />

7.  Once complete, there will be a success notification.

    <img src="images/lab04/media/image16.png" style="width:6.38861in;height:1.27917in" alt="Graphical user interface, text, application Description automatically generated" />

## Step 3: Experiment Design

In this step, you will add a dataset from the datastore and begin
designing the experiment.

1.  In the selection pane on the left, select the **Designer** module.

    <img src="images/lab04/media/image17.png" style="width:1.38661in;height:2.61667in" alt="Graphical user interface, application Description automatically generated" />

2.  Select the New Pipeline option **Easy-to-use-prebuilt modules**

    <img src="images/lab04/media/image18.png" style="width:5.10077in;height:2.675in" alt="Graphical user interface, application Description automatically generated" />

3.  In the Settings pane that pops up on the right, we need to specify a
    compute cluster as well as pipeline settings. If you already have a
    compute cluster initiated, please skip down to step 8. *Note: if the
    settings pane does not pop up automatically, select the gear icon
    next to the Pipeline name.*

    <img src="images/lab04/media/image19.png" style="width:3.88065in;height:4.075in" alt="Graphical user interface, text, application, email Description automatically generated" />

4.  Select **Compute Cluster** for the compute type and then click
    **Create Azure ML Compute Cluster**.

    <img src="images/lab04/media/image20.png" style="width:4.63869in;height:2.05in" alt="Graphical user interface, text, application, email Description automatically generated" />

5.  In the Compute Cluster screen, we are going to select the default
    options of Dedicated Priority, CPU Virtual Machine, and
    Standard_DS3_v2. If Standard_DS3_v2 is not an option, click the
    ‘Select from all options’ and locate it then. The Location should be
    defaulted to your region but you can change that if necessary. Click
    **Next** when complete.

    <img src="images/lab04/media/image21.png" style="width:6.79426in;height:3.34167in" alt="Graphical user interface, text, application, email Description automatically generated" />

6.  In the Advanced Settings, you will see your selections from the
    prior screen at the top. Name the compute **CustomerChurnCC**. Set
    the Minimum number of nodes at 0 and Maximum at 2. Set the Idle
    seconds before scale down to 60. Click Create when complete.
    **Important: This will be mentioned later on as well, but we need to
    make sure and scale down our clusters when we’re done to avoid
    incurring charges.**

    <img src="images/lab04/media/image22.png" style="width:6.14467in;height:2.1375in" alt="Graphical user interface, text, application Description automatically generated" />

7.  After creation, you will be redirected back to the Setting pane of
    the pipeline, select the newly created Compute Cluster from the
    options. If you have trouble selecting the compute cluster from the
    dropdown, wait until you receive a message saying “Compute
    ‘CustomerChurnCC’ provisioning succeeded” then try to select it
    again. Name the draft “**CustomerChurn**”

    <img src="images/lab04/media/image23.png" style="width:3.77361in;height:3.82278in" alt="Graphical user interface, text, application, email Description automatically generated" />

8.  We are going to begin the process of dragging what are called assets
    or modules on the page to build a pipeline starting with data
    ingestion and ending in exporting the model results to the
    datastore.

    *There are multiple ways to accomplish this task, we are going to
    build a basic pipeline for demonstration. In a real-world scenario,
    we would train multiple models and perform hyperparameter tuning to
    find the optimal model.*

9.  Let’s begin with bringing in the data set we created earlier. Open
    the **Datasets** module and drag the **TelcoChurn** dataset onto the
    designer page.

    <img src="images/lab04/media/image24.png" style="width:1.81921in;height:1.69271in" alt="Graphical user interface, text, application, email Description automatically generated" /><img src="images/lab04/media/image25.png" style="width:3.46533in;height:1.37951in" alt="Diagram Description automatically generated with medium confidence" />

10. If you select the **TelcoChurn** asset on the designer page, you can
    see the details of it. Every asset will work similarly when
    selected. The only difference will be that some will require
    parameters to be set (We will cover this shortly).

    <img src="images/lab04/media/image26.png" style="width:2.76597in;height:3.30568in" alt="Graphical user interface, text, application Description automatically generated" />

11. Next, we know that TotalCharges has a few blank values (we could
    have tested this via using the Summarize Data asset and evaluating
    each column distribution but for the sake of this lab we’re going to
    move on to handling them). Drag **Clean Missing Data** onto the page
    and connect TelcoChurn to the new asset.

    <img src="images/lab04/media/image27.png" style="width:2.70438in;height:1.87083in" alt="A picture containing chart Description automatically generated" />

12. In the properties pane for Clean Missing Data. Select TotalCharges
    as the Column and set the parameters below. *Note: If the goal of
    the lab was to build the best model we would have run some form of
    regression or sub-sample from the complete rows to find a predicted
    value for this.*

    <img src="images/lab04/media/image28.png" style="width:2.25828in;height:3.25417in" alt="Graphical user interface, text, application Description automatically generated" />

13. Next, drag **Split** **Data** onto the page. Connect the left node
    of Clean Missing data to the top of Split Data as shown below. Set
    the parameters to match the following settings. To train a machine
    learning model, we need to provide data for the model to train on.
    Generally, we provide 25% of the data to train the model and then
    test on the remaining 75%. This ensures that we have a large enough
    dataset to test our proposed model on. You can set the Random seed
    to any number you’d like, or leave it blank. The seed is used for
    re-producibility of results.

    <img src="images/lab04/media/image29.png" style="width:2.10868in;height:2.65182in" />
    <img src="images/lab04/media/image30.png" style="width:3.27426in;height:2.63264in" alt="Graphical user interface, text, application, email Description automatically generated" />

14. From the Split Data asset, we are going to connect the left node
    (training) to 1 new asset and the right node (testing) to 3 new
    assets. The left node is connected to **Select Columns in Dataset**
    (drag over from the module options). In the Select Columns options,
    remove CustomerID from data. If we were to leave the ID in, the
    models would treat that as a feature which would cause errors down
    the line.

    <img src="images/lab04/media/image31.png" style="width:3.11667in;height:1.72958in" alt="Diagram Description automatically generated" /><img src="images/lab04/media/image32.png" style="width:3.57729in;height:1.70347in" alt="Graphical user interface, text, application Description automatically generated" />

15. The right node of the Split Data asset is connected to another
    instance of **Select Columns in Dataset**. This extracts the
    Customer ID’s of the testing data. We are going to join this back in
    to the results dataset after the models have run.

    <img src="images/lab04/media/image33.png" style="width:3.04853in;height:1.57265in" alt="Diagram Description automatically generated" /><img src="images/lab04/media/image34.png" style="width:3.46568in;height:1.52663in" />

16. We’re going to create two models. The first one is a Two-Class
    Boosted Decision Tree with the following parameters. Search for and
    drag Two-Class Boosted Decision Tree from the module selection over
    to the page. *Yours will not turn green until after we have
    successfully run the pipeline*

    <img src="images/lab04/media/image35.png" style="width:2.76436in;height:1.1664in" alt="A picture containing diagram Description automatically generated" /><img src="images/lab04/media/image36.png" style="width:2.82917in;height:3.04193in" alt="Graphical user interface, text, application, email Description automatically generated" />

17. The second is a Two-Class Decision Forest with the following
    parameters. Drag this on to the page.

    <img src="images/lab04/media/image37.png" style="width:2.40751in;height:0.74845in" /><img src="images/lab04/media/image38.png" style="width:3.35852in;height:2.70052in" />

18. **Checkpoint**. Below is how our designer page looks for reference.
    Yours will not show the green Completed indicator on the models just
    yet. Those appear after we’ve ran the pipeline.

    <img src="images/lab04/media/image39.png" style="width:6.59064in;height:3.11505in" />

## Step 4: Training models

Now that we’ve setup the design of our experiment, we can begin to train
the models and evaluate the outputs.

1.  You will first train a **Two-Class Boosted Decision Tree**. Setup
    your page similarly to the one below. You should already have Split
    Data, Select Columns and the Decision Tree setup. Add a **Train
    Model** and **Score Model** step. Connect the Two-Class Boosted
    Decision Tree to the left node on Train Model and then the Select
    Columns in Dataset to the right node on Train Model. Connect the
    single output node of Train Model to the left node on Score Model
    and then the right output node of Split Data to the right input node
    on Score Model.

    <img src="images/lab04/media/image40.png" style="width:4.77415in;height:4.69792in" />

2.  In the properties pane of the **Train Model** module, choose the
    **Churn** column since that is the column we are trying to predict.

    <img src="images/lab04/media/image41.png" style="width:4.09375in;height:1.64577in" />

3.  Now add a **Two-Class Decision Forest**, along with another **Train
    Model** and **Score Model**. Connect the Two-Class Decision Forest
    output node to the left input node on Train Model and then the
    Select Columns in Dataset output node to the right input node on the
    Train Model. Finally add an **Evaluate Model** to the bottom and
    connect both Score Models to it.

    <img src="images/lab04/media/image42.png" style="width:7in;height:3.10341in" />

4.  Review the screenshot below to make sure all of the connections are
    made. Your page should look similar to this. Press the Submit button
    at the top right of the screen.

    <img src="images/lab04/media/image43.png" style="width:5in;height:4.22917in" />

5.  On the Set up pipeline run popup, create a new experiment and name
    it. Clicking **Submit** will kick off the run so we can evaluate the
    two models.

    <img src="images/lab04/media/image44.png" style="width:3.61799in;height:4.03125in" alt="Graphical user interface, text, application, email Description automatically generated" />

6.  The pipeline run will take about 10-15 minutes on the sample data.
    Once complete, click on the **Evaluate Model** step and then the
    Output + Logs option. Then click the Evaluation results option.

    <img src="images/lab04/media/image45.png" style="width:4.65116in;height:1.92462in" />

7.  From here, we can see the results of both models side by side to
    compare them. The **Decision Tree** was the left port and the
    **Decision Forest** was the right port. Without getting too thick
    into the weeds, we can see that the left port had a higher accuracy,
    precision, and AUC at the 0.5 threshold. Sliding the threshold back
    and forth changes the values. The right port received higher marks
    for Recall and F1 Score.

    <img src="images/lab04/media/image46.png" style="width:3.40152in;height:2.44583in" alt="Diagram Description automatically generated" /><img src="images/lab04/media/image47.png" style="width:3.33582in;height:2.47014in" alt="Diagram Description automatically generated" />

8.  Naturally, we could go back to the parameters and introduce
    parameter sweeps, hyper-tuning, and feature selection to try and
    boost these scores. For the purposes of this lab, we are going to
    proceed with the baseline models.

##  Step 5: Output Model

In this step, we’re going to output the results of these models to blob
storage for the next labs.

1.  **CHECK.** Your page should look similar to this at this point in
    the lab.

    <img src="images/lab04/media/image48.png" style="width:7.14059in;height:5.97916in" alt="Diagram Description automatically generated" />

2.  Drag the following modules below the evaluate model step. Connect
    the Score Model for the Decision Tree to the left column and then
    the Score Model for the Decision Forest to the right column.

    <img src="images/lab04/media/image49.png" style="width:4.05011in;height:3.72083in" alt="Diagram Description automatically generated" />

3.  In the parameters for both Select Columns in Dataset, set the
    following. We only want to keep the Predicted Labels and
    Probabilities.

    <img src="images/lab04/media/image50.png" style="width:4.0378in;height:1.925in" alt="Graphical user interface, text, application Description automatically generated" />

4.  At the Add Columns step, we’re going to add back in the Customer
    ID’s that we extracted earlier from the testing dataset.

    <img src="images/lab04/media/image51.png" style="width:5.16386in;height:5.275in" alt="Diagram Description automatically generated" />

5.  At the Convert to CSV stage, we are going to send the model results
    to Blob Storage as CSV’s. On the left side, change the output
    settings to the following with the **Path on datastore** as:
    **azureml/Decision_Tree_Results/dtresults**

    <img src="images/lab04/media/image52.png" style="width:3.47208in;height:3.6125in" alt="Graphical user interface, text, application Description automatically generated" />

6.  On the right side set the Path to datastore as:
    **azureml/Decision_Forest_Results/dfresults**

7.  The final designer should resemble this:

    <img src="images/lab04/media/image53.png" style="width:6.24893in;height:7in" alt="Diagram, engineering drawing Description automatically generated" />

8.  Once complete, hit **Submit** in the top right to begin the pipeline
    run. This should take approximately 3 to 5 minutes.

9.  After completion, select one of the **Convert to CSV** modules and
    then **Access Data**

    <img src="images/lab04/media/image54.png" style="width:4.42295in;height:1.93333in" alt="Graphical user interface, application Description automatically generated" />

10. From here, you can see the data outputs. **data.csv** is the output
    file we’ll need in the next section. The URL for the file can be
    accessed by clicking on the file.

    <img src="images/lab04/media/image55.png" style="width:5.54033in;height:1.85833in" alt="Graphical user interface, text, application Description automatically generated" />

11. If you would like to view the data, you can select the **Edit**
    button to preview it.

12. Refer back to the **Clean Missing Data** module. In the Output
    Settings Option, we are going to select **Regenerate Output.** This
    will ensure that every time we execute the pipeline, we are
    re-executing the SQL query to pull the latest data and handling the
    missing values appropriately and not using cached results from a
    previous execution.

    <img src="images/lab04/media/image56.png" style="width:4.88542in;height:1.85417in" />

13. The final step is to **Publish** the pipeline. Click the **Publish**
    button in the top right next to the Submit button. Select **Create
    New** and call it **CustomerChurnEP**. Click **Publish**.

    <img src="images/lab04/media/image57.png" style="width:3.61437in;height:4.64583in" alt="Graphical user interface, text, application Description automatically generated" />

14. Click on the **Pipelines** tab to see the newly published pipeline.

    <img src="images/lab04/media/image58.png" style="width:6.0625in;height:2.51341in" />

    After approximately 60 seconds, the cluster will scale down (to ensure you do not incur any unnecessary costs) until another experiment is kicked
    off.

# Summary

In this lab, you signed into Azure Machine Learning Studio, created a
datastore connected to a SQL database, prepared the data, trained two
models, evaluated the accuracy of the models, and sent the output of
those models to Blob Storage to be accessed by other Microsoft
resources. Classification tasks can easily be performed with Azure ML
using Two-Class Boosted Decision Trees or Two-Class Decision Forests.

Next, go to [Lab 5](lab05.md).