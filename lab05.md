# Lab 5 - Import Machine Learning Predictions into Customer Insights

![](images/lab05/media/image1.png)

![](images/lab05/media/image2.png)


# Lab Overview

## Introduction

This final lab will close the loop by importing back to Customer
Insights the results of your machine learning model which predicts which
customers are likely to churn.

## Objectives

The objectives of this exercise are to:

-   Learn how to import the results from your machine learning model
    back into Customer Insights

The estimated time for this lab is 45 minutes

# Exercise 1: Import Machine Learning Output to Customer Insights

In this section you will learn how to import the results of your machine
learning models into Customer Insights.

## Step 1: Copy the storage account key for your Azure ML storage account

1.  Browse to <https://portal.azure.com> and sign in with your
    organizational account.

    Search for **asastore** in the search box at the top:

    ![](images/lab05/media/image3.png)

1.  On the Security + networking… Access keys blade, click the **Show** button.
    button:

    ![](images/lab05/media/asadatalake1.png)

1.  Click the Copy to clipboard button on the right side of the Key textbox
    under the key1 section:

    ![](images/lab05/media/image5.png)

## Step 2: Import Machine Learning Output to Customer Insights

1.  Browse to <https://home.ci.ai.dynamics.com/> and sign in with your
    organizational account. On the Data… Data sources left nav, click the **+ Add data source**.
    button:

    ![](images/lab05/media/image6.png)

1.  Choose Microsoft Power Query and give it the name **AzureMlResults** and click Next.

    ![](images/lab05/media/AzureMlResults.png)

1.  Click the Azure tab in the Power Query Online window and click the **Azure
    Blobs** option:

    ![](images/lab05/media/image8.png)

1.  In the **Account name or URL** type in the name of your storage account
    for Azure ML (**asastore<inject key="Deployment ID" enableCopy="false" />**). Change the
    Authentication kind to **Account key**. Paste in the storage account key for
    your **asastore<inject key="Deployment ID" enableCopy="false" />** storage account. Click Next.

    ![](images/lab05/media/image9.png)

1.  Click the checkbox on the container called **azureml-blobstore-\<guid>**
    and click the Transform data button.

    ![](images/lab05/media/image10.png)

1.  On the Name column click the dropdown and choose **Text filters**… **Equals**:

    ![](images/lab05/media/image11.png)

1.  Paste **azureml/Decision_Tree_Results/dtresults/data.csv** into the Filter
    rows dialog and click Ok.

    ![](images/lab05/media/image12.png)

1.  Power Query Online should now show one row with the CSV file which is
    the output of your ML model. Click the **\[Binary\]** link in the Content
    column for that one row:

    ![](images/lab05/media/image13.png)

1.  The columns in your ML results file should show on the screen. On the
    Transform tab, click the **Use first row as headers** button:

    ![](images/lab05/media/image14.png)

1.  The first row has now been promoted to column headers. Now click the ABC
    button on the Scored Probabilities column and choose **Decimal number** to
    convert that column to a decimal.

    ![](images/lab05/media/image15.png)

1.  Rename this query to **PredictedCustomerChurn**.

    ![](images/lab05/media/image16.png)

1.  On the **Add column** tab click the **Conditional column** button:

    ![](images/lab05/media/image17.png)

1.  Name the new column **ChurnRisk** and setup the conditions as follows. If
    the model predicts the customer will not churn (Scored Labels=No),
    return Low. If the model predicts they have a 50% or greater probability
    of churning (because if it got to this condition, Scored Labels must
    equal Yes, and Scored Probabilities \> .5) return High. Otherwise return
    Medium. Then click OK and click Next.

    ![](images/lab05/media/image18.png)

1.  Accept manually should be selected. Click **Save**:

    ![](images/lab05/media/image19.png)

1.  Click the radio button to the left of the AzureMlResults data source and
    click the Refresh button at the top of the screen.

    ![](images/lab05/media/image20.png)

1.  On the Data… Unify left nav, click the **Edit** button.

    ![](images/lab05/media/unify1.png)
    
1. On the Unify tab, click on **Select entities and fields**

    ![](images/lab05/media/unify2.png)

1.  Click the checkmark next to the **PredictedCustomerChurn** entity and click
    **Apply**:

    ![](images/lab05/media/unify3.png)

1.  Click the PredictedCustomerChurn entity and set the primary key to
    customerID and click **Next**.

    ![](images/lab05/media/image23.png)

1.  Click the **+Add rule** button under the AzureMlResults :
    PredictedCustomerChurn entity:

    ![](images/lab05/media/image25.png)

1.  Choose the **CustomerDemographics** entity and the **customerID** fields on both
    entities. Name this rule and click Done.

    ![](images/lab05/media/image26.png)

1.  Click on **Save and close**
     
    ![](images/lab05/media/unify4.png)
     
1. Click **Run matching conditions only** at the top of the screen:

    ![](images/lab05/media/unify5.png)

1.  Once the run is successful, click on the **Edit** button under **unified customer fields**.

    ![](images/lab05/media/unify6.png)

1.  On the customer fields tab, click the **Excluded fields** link:

    ![](images/lab05/media/image29.png)

1.  Click the arrow next to Scored Labels, Scored Probabilities, and
    ChurnRisk fields to include those fields in the merging. Click the **Done**
    button.

    ![](images/lab05/media/image30.png)

1.  Scroll down to the bottom of the field list, mouse over the Scored
    Labels field and click the Rename button. Rename Scored Labels to
    **PredictedChurn**. Rename Scored Probabilities to
    **PredictedChurnProbability**.

    ![](images/lab05/media/image31.png)

    ![](images/lab05/media/image32.png)
    
    ![](images/lab05/media/image33.png)

1.  Click **Save** and click on **unify customer profiles** at the top of the screen.

    ![](images/lab05/media/unify7.png)

1.  Once the run is successful, go to the Data… Entities left nav, click on
    the Customer entity, go to the Data tab and scroll to the right. You
    should see that the PredictedChurn and PredictedChurnProbability are now
    included as fields in the customer entity. For clarity, customer
    0423-UDIJQ has not yet churned (Churn=No) but has a 52% probability of
    churning (PredictedChurn=Yes, PredictedChurnProbability=0.52) at some
    point soon per the ML model. Customer 2480-JZOSN has not yet churned
    (Churn=No) and the model predicts with a probability of 36% that they
    will not churn (PredictedChurn=No, PredictedChurnProbability=0.36). In
    other words, a high (close to 1.0) PredictedChurnProbability doesn’t
    alone mean they are predicted to churn. The probability is the
    probability associated to the PredictedChurn field. The customers with a
    blank PredictedChurn were part of the training set in Azure Machine
    Learning so no prediction was made.

    ![](images/lab05/media/image35.png)

## Step 3: Segment your customers at highest risk of churning

1.  Go to the Customers left nav and click **Search & filter index**. Let’s
    find any customers which have a high probability of churning.

    ![](images/lab05/media/image36.png)

1.  Click the **+Add** button to add a field to the index:

    ![](images/lab05/media/image37.png)

1.  Check the Churn, PredictedChurn and ChurnRisk fields (decimal fields are
    not currently supported) and click **Add**.

    ![](images/lab05/media/image38.png)
    
1.  Mouse over the **ChurnRisk** field and click the **funnel icon** to
    enable filtering on this field. Do the same for the **Churn** field.

    ![](images/lab05/media/image39.png)

1.  When prompted after clicking the funnel icon, accept the default options
    and click **OK**.

    ![](images/lab05/media/image40.png)

1.  Then click the **Save** and **Run** buttons. When refreshing completes,
    click the **Back to Customers** button.

    In the right side Filter customers pane, filter to Churn=No (not
    currently churned) and ChurnRisk=High (high predicted risk of churning
    soon). Now you have filtered your 7,000 customers down to a list of 478
    customers at highest risk of churn. Let’s take action on this list of
    customers.

    ![](images/lab05/media/image41.png)

1.  Click the **Save filters as segment** link and enter a name for this new
    segment of customers. Click **Save**.

    ![](images/lab05/media/image42.png)

1.  Click **Activate**.

    ![](images/lab05/media/image43.png)

1.  Go to the Segments left nav, select the Customers at High Risk of Churn
    segment, wait for it to finish refreshing, then click the **Download**
    button. You can send this file to your customer care department and ask
    them to come up with strategies to boost customer satisfaction and
    improve retention for these high risk customers.

    ![](images/lab05/media/image44.png)

# Summary

In this lab, you imported the results of your machine learning model
back into Customer Insights and merged this information into the
Customer entity. Now you are able to take action from Customer Insights
on customers who are predicted to churn.

Next, go to [Lab 6](lab06.md).
