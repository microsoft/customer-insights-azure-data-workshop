# Lab 2 - Deploying Azure Services

![](images/lab02/media/image1.png)

![](images/lab02/media/image2.png)

#  

Contents

# 

[Lab Overview](#lab-overview)

[Introduction](#introduction)

[Objectives](#objectives)

[Exercise 1: Deploy ARM Template](#exercise-1-deploy-arm-template)

- [Step 1: Deploy the ARM template](#step-2-deploy-the-arm-template)

- [Step 2: Grant permissions to the team](#step-3-grant-permissions-to-the-team)

- [Step 3: Cleanup Azure resources (at conclusion of workshop)](#step-4-cleanup-azure-resources-at-conclusion-of-workshop)

[Summary](#summary)

# 

# Lab Overview

## Introduction

This hands on lab will deploy the necessary Azure services to support
subsequent labs. The services that are deployed are Azure Synapse
Analytics, Azure Machine Learning, Azure Data Lake Storage Gen2, and a
few supporting services like Azure Key Vault and Azure Application
Insights.

## Objectives

The objectives of this exercise are to:

-   Learn how to deploy Azure services with an Azure Resource Manager
    template

-   Learn how to use Role Based Access Control (RBAC) to grant access to
    Azure resources

The estimated time for this lab is 30 minutes

# Exercise 1: Deploy ARM Template

In this section you will deploy the necessary Azure services into your
Azure subscription using a parameterized Azure Resource Manager (ARM)
template.


## Step 1: Deploy the ARM template

1.  Go to the Access control (IAM) left nav of the resource group **customer-insights-workshop-rg**. Click the “View my access” button. Ensure
    that you are assigned the Owner role for this resource group (or the
    Owner role is inherited from the subscription or management group). If
    you are not an Owner, then ask your Azure administrator to make you an
    Owner on this resource group. Or ask your Azure administrator to perform
    this deployment for you. Owner permissions will also be required for some steps in Lab 3.

    _Note: RBAC permissions changes can take up to **15 minutes** to propogate._

    ![](images/lab02/media/image6.png)

1.  CTRL+click (on Windows) or CMD+click (on MacOS) the **Deploy to Azure** button below which will prompt you to
    enter several parameters before deploying Azure resources:

     [![Button to deploy the resource template to Azure.](https://aka.ms/deploytoazurebutton "Deploy the Contoso HA resources to Azure")](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FArtisConsulting%2Fcustomer-insights-azure-data-workshop%2Fmain%2FAzureSetup%2FArmTemplate.json)

1.  On the Custom deployment form fill in the choose the right Azure
    subscription and choose the **customer-insights-workshop-rg** resource group.

    Then fill in the following “Instance details”:

    -   **Region** – This should default to the region your resource group
        is in and shouldn’t be changed

    -   **Unique Suffix** – Enter **<inject key="Deployment ID" />**

    -   **SQL Administrator Login Password** – Enter **<inject key="LABVM Admin Password" />**

1.  Review the requirements for the password again carefully. Click the
    **Review + create** button.

    If you get any validation failures, click the arrow to investigate
    the problems and resolve them by clicking the Previous button,
    changing the parameters, then trying again:

    ![](images/lab02/media/image8.png)

1.  You should see a “Validation passed” message. Click **Create**.

    ![](images/lab02/media/image9.png)

1.  The deployment should take around 5-10 minutes. If you get any
    errors, press the “Redeploy” button at the top and correct the
    parameters appropriately and deploy again using the same suffix as
    your previous attempt. If all else fails, delete all the resources
    from the resource group and try the deployment again with a
    **different** suffix (since the deleted key vault has the [soft
    delete feature
    enabled](https://docs.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)).

    Once the deployment completes you should have the following
    resources:

    -   **amlworkspace<inject key="Deployment ID" enableCopy="false" />** - An Azure Machine Learning workspace you
        will use in a later lab to build machine learning models to enrich
        your customer data

    -   **asaappinsights<inject key="Deployment ID" enableCopy="false" />** - The Azure Application Insights which
        is tied to your Azure Machine Learning workspace

    -   **asadatalake<inject key="Deployment ID" enableCopy="false" />** - The Azure Data Lake Store Gen2 storage
        account which will be the primary storage for Synapse and for export
        from Customer Insights

    -   **asakeyvault<inject key="Deployment ID" enableCopy="false" />** - The Azure Key Vault tied to your Azure
        Machine Learning workspace for storage of secrets

    -   **asastore<inject key="Deployment ID" enableCopy="false" />** - The Azure blob storage account tied to your
        Azure Machine Learning workspace. (Storage accounts with
        hierarchical namespace enabled like the asadatalake storage account
        are not supported by Azure Machine Learning at the time of this
        writing, so a separate blob storage account was created for Azure ML
        internal storage.)

    -   **asaworkspace<inject key="Deployment ID" enableCopy="false" />** - The Azure Synapse Analytics workspace.
        The Serverless SQL pool is part of this workspace. You can use
        Synapse Studio to interact with this workspace.

## Step 2: Grant permissions to the team

Any other developers on the team who will need to create and monitor
this solution should be given permissions to it. The user who will setup
the export from Customer Insights to Azure and the remaining labs should
have permission to this Azure resource group. Permissions are maintained
using Role Based Access Control (RBAC). While it’s a best practice to
use groups instead of assigning permissions to individual users, for the
purposes of this lab feel free to assign permissions to individual users
if you don’t have an appropriate group handy.

1.  On the **Access control (IAM)** left nav of the
    **customer-insights-workshop-rg** click **+Add** and **Add role
    assignment**.

    ![](images/lab02/media/image10.png)

1.  One at a time, grant each developer the Contributor role by choosing
    role **Contributor** and searching for their name, selecting their name, and
    clicking Save.

    ![](images/lab02/media/image11.png)
    
1.  Repeat the process for the Storage Blob Data Contributor role. One at a
    time, grant each developer the role by choosing Role=Storage Blob Data
    Contributor and searching for their name, selecting their name, and
    clicking Save. Be sure to assign yourself **Storage Blob Data
    Contributor** role, too!

    ![](images/lab02/media/image12.png)

1.  In the search box at the top of the Azure portal,
    search for “asaworkspace” and click on the Synapse workspace (not the
    SQL Server) which appears under the Resources section.

    ![](images/lab02/media/image13.png)

1.  On the Overview blade and the Essentials section, click the Workspace
    web URL link to open Synapse Studio.

    Go to the Manage left nav and the Access control left nav. Make sure that user is assigned with **Synapse Administrator** role.

    ![](images/lab02/media/lab2-synapse.png)


# Summary

In this lab, you learned how to create an Azure resource group,
provision Azure resources with an Azure Resource Manager template, and
grant access to those resources.

Next, go to [Lab 3](lab03.md).
