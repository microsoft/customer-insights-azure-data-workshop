# Lab 1 - Customer Insights Data Sources, Unification and Enrichment

# <img src="images/lab01/media/image1.png" style="width:3.9375in;height:0.63819in" />

# <img src="images/lab01/media/image2.png" style="width:3.48973in;height:1.20479in" alt="Text Description automatically generated with medium confidence" />

#  

Contents

# 

[Lab Overview](#lab-overview)

[Introduction](#introduction)

[Objectives](#objectives)

[Exercise 1: Import customer
datasets](#exercise-1-import-customer-datasets)

- [Step 1: Sign into Dynamics 365 Customer Insights](#step-1-sign-into-dynamics-365-customer-insights)

- [Step 2: Load CustomerDemographics sample dataset to Customer Insights](#step-2-load-customerdemographics-sample-dataset-to-customer-insights)

- [Step 3: Load CustomerServiceAttributes sample dataset to Customer Insights](#step-3-load-customerserviceattributes-sample-dataset-to-customer-insights)

- [Step 4: Ensure entities are loaded](#step-4-ensure-entities-are-loaded)

[Exercise 2: Unify customer
entities](#exercise-2-unify-customer-entities)

- [Step 1: Map fields in your customer entities](#step-1-map-fields-in-your-customer-entities)

- [Step 2: Match your customer entities on customerID](#step-2-match-your-customer-entities-on-customerid)

- [Step 3: Merge your customer entities](#step-3-merge-your-customer-entities)

[Exercise 3: Enrich customer
entities](#exercise-3-enrich-customer-entities)

- [Step 1: Enrich with Interest affinities](#step-1-enrich-with-interest-affinities)

[Summary](#summary)

# 

# Lab Overview

## Introduction

This hands on lab will provide an overview of key functionality in
Dynamics 365 Customer Insights with a sample dataset. You will load
sample customer data sources and ensure they have been properly mapped,
merged, and enriched. Whether you walk through this lab with sample data
or use your own customer datasets for your business, the steps in this
lab are prerequisites for the later hands on labs.

Prior to this workshop you should have already provisioned a Dynamics
365 Customer Insights environment, granted appropriate access to your
team and loaded appropriate customer datasets relevant to your business.
If you have completed these steps, skim the instructions below to ensure
you have completed each type of activity as they are prerequisites for
later labs.

## Objectives

The objectives of this exercise are to:

-   Import customer datasets into your Dynamics 365 Customer Insights
    environment

-   Map the fields in your customer entities

-   Merge multiple entities on customer key columns

-   Perform enrichment of your data using external industry sources and
    services

The estimated time for this lab is 90 minutes

# Exercise 1: Import customer datasets

In this section you will import sample customer datasets to your
Dynamics 365 Customer Insights environment. The steps below will show
how to load a demo dataset which will be used for illustrative purposes
in this hands on lab. They can be skipped if your real datasets are
already loaded, unified and enriched appropriately.

## Step 1: Sign into Dynamics 365 Customer Insights

Browse to <https://home.ci.ai.dynamics.com/> and sign in with your
organizational account.

If you don’t have a Customer Insights environment provisioned already
and wish take advantage of a trial environment, visit
<https://aka.ms/tryci>

## Step 2: Load CustomerDemographics sample dataset to Customer Insights

Go to the Data… Data sources tab and click “Add data source”

<img src="images/lab01/media/image3.png" style="width:7.5in;height:4.46597in" alt="Graphical user interface, application, Teams Description automatically generated" />

Choose “Import data” and enter a dataset name “CustomerDemographics”

<img src="images/lab01/media/image4.png" style="width:7.5in;height:4.73333in" alt="Graphical user interface, text, application, email Description automatically generated" />

Search for “Web” and choose “Web API” which is a quick way to import a
dataset from a public website. (Your actual business data could come
from any other type of data source which Power Query Online supports.)

<img src="images/lab01/media/image5.png" style="width:7.5in;height:2.39375in" alt="Graphical user interface, text, application, email Description automatically generated" />

Paste in the following URL and click Next:

<https://raw.githubusercontent.com/ArtisConsulting/customer-insights-azure-data-workshop/main/SampleData/CustomerDemographics.csv>

<img src="images/lab01/media/image6.png" style="width:7.5in;height:4.43889in" alt="Graphical user interface, application Description automatically generated" />

Choose “Transform data”

<img src="images/lab01/media/image7.png" style="width:7.5in;height:4.54444in" alt="Graphical user interface, application Description automatically generated" />

Click the Transform tab at the top of the Power Query screen and then
click “Use first row as headers”

<img src="images/lab01/media/image8.png" style="width:7.5in;height:4.44375in" alt="Graphical user interface, application Description automatically generated" />

Confirm the column names read customerID, gender, etc. instead of
Column1, Column2, etc.

<img src="images/lab01/media/image9.png" style="width:7.5in;height:4.39444in" alt="Graphical user interface, application Description automatically generated" />

Change the query name to CustomerDemographics. Click the Next button at
the bottom of the screen.

<img src="images/lab01/media/image10.png" style="width:7.5in;height:3.9875in" alt="Graphical user interface, application, table Description automatically generated" />

Choose to refresh manually (since this is a static sample dataset that
won’t change) and click Save.

<img src="images/lab01/media/image11.png" style="width:7.5in;height:4.53125in" alt="Graphical user interface, text, application Description automatically generated" />

While you wait for the CustomerDemographics data source to refresh,
continue onto the next step.

<img src="images/lab01/media/image12.png" style="width:7.5in;height:2.20347in" alt="Graphical user interface, text, application Description automatically generated" />

## Step 3: Load CustomerServiceAttributes sample dataset to Customer Insights

Click “Add data source”, Import data, and name it
CustomerServiceAttributes:

<img src="images/lab01/media/image13.png" style="width:7.5in;height:4.67986in" alt="Graphical user interface, text, application, email Description automatically generated" />

Choose Web API like on the previous data source and then paste in the
following URL:

<https://raw.githubusercontent.com/ArtisConsulting/customer-insights-azure-data-workshop/main/SampleData/CustomerServiceAttributes.csv>

Accept the existing connection since this data source is from the same
website.

<img src="images/lab01/media/image14.png" style="width:7.5in;height:4.80278in" alt="Graphical user interface, text, application Description automatically generated" />

Click “Transform data”:

<img src="images/lab01/media/image15.png" style="width:7.5in;height:4.78681in" alt="Graphical user interface, application Description automatically generated" />

On the Transform tab click “User first row as headers”:

<img src="images/lab01/media/image16.png" style="width:7.5in;height:4.79236in" alt="Graphical user interface, application Description automatically generated" />

Confirm the column headers now say customerID, tenure, PhoneService,
etc.

<img src="images/lab01/media/image17.png" style="width:7.5in;height:2.05625in" alt="Graphical user interface Description automatically generated" />

Left click on the ABC icon next to the tenure column header and choose
“Whole number” to convert this column to an integer data type.

<img src="images/lab01/media/image18.png" style="width:6.69885in;height:3.13585in" alt="Graphical user interface, application Description automatically generated" />

Rename the query to CustomerServiceAttributes and then click click the
Next button at the bottom of the screen.

<img src="images/lab01/media/image19.png" style="width:7.5in;height:3.98681in" alt="Graphical user interface, application, table Description automatically generated" />

Choose to refresh manually and click Save.

<img src="images/lab01/media/image20.png" style="width:7.5in;height:3.60486in" />

## Step 4: Ensure entities are loaded

Once the data sources are done refreshing (it takes approximately 5
minutes), go to the Entities tab and click each entity:

<img src="images/lab01/media/image21.png" style="width:7.5in;height:2.27431in" alt="Graphical user interface, text, application Description automatically generated" />

On the Data tab for each entity, ensure that rows have been loaded and
are visible. If no rows are shown, go back to the “Data sources” left
nav and refresh the data sources again.

<img src="images/lab01/media/image22.png" style="width:7.5in;height:3.59444in" alt="Graphical user interface Description automatically generated" />

# Exercise 2: Unify customer entities

In this section you will map, match and merge your sample customer
entities in your Dynamics 365 Customer Insights environment. This step
is a prerequisite for later steps and labs.

## Step 1: Map fields in your customer entities

On the Data… Unify left nav, click “+ Select entities” on the Map tab:

<img src="images/lab01/media/image23.png" style="width:7.5in;height:3.47361in" alt="Graphical user interface, application, Teams Description automatically generated" />

Click the checkboxes next to the two entities you with to map then click
Apply:

<img src="images/lab01/media/image24.png" style="width:3.46923in;height:4.83401in" alt="Graphical user interface, text, application Description automatically generated" />

Click the CustomerDemographics entity. Because “intelligent mapping” is
selected, it will have already set the Type on the Country, customerID,
gender and zip columns. Set the primary key to customerID.

<img src="images/lab01/media/image25.png" style="width:7.5in;height:5.59444in" alt="Graphical user interface, application, email Description automatically generated" />

Click on the CustomerServiceAttributes entity. Set the primary key to
customerID. Notice the intelligent mapping has incorrectly categorized
the PhoneService column as this column indicates whether this telco
customer has phone service or not. Dropdown the Type dropdown next to
PhoneService and choose the blank value to blank out the Type on this
column. Then click the Save button at the top of the page.

<img src="images/lab01/media/image26.png" style="width:7.5in;height:4.6625in" alt="Graphical user interface, application Description automatically generated" />

## Step 2: Match your customer entities on customerID

Go to the Match tab and click the “+Set order” button.

<img src="images/lab01/media/image27.png" style="width:7.5in;height:4.6in" alt="Graphical user interface, application Description automatically generated" />

Choose CustomerDemographics to the first or primary entity. Choose
CustomerServiceAttributes to be the second entity. Click Done.

<img src="images/lab01/media/image28.png" style="width:6.06335in;height:4.75066in" alt="Graphical user interface, text, application, email Description automatically generated" />

Click the Add Rule button:

<img src="images/lab01/media/image29.png" style="width:7.5in;height:2.56389in" alt="Graphical user interface, text, application, email Description automatically generated" />

Select customerID from both “Select field” dropdowns. Name this rule and
click Done.

<img src="images/lab01/media/image30.png" style="width:6.04251in;height:6.9593in" alt="Graphical user interface, text, application, email Description automatically generated" />

Click the **Save** button at the top and then click the **Run** button
at the top.

<img src="images/lab01/media/image31.png" style="width:7.5in;height:3.12431in" alt="Graphical user interface, text, application, email Description automatically generated" />

Wait while the matching runs. It takes approximately 5 minutes.

<img src="images/lab01/media/image32.png" style="width:7.5in;height:5.22292in" alt="Graphical user interface, text, application, email Description automatically generated" />

When the matching is complete, validate that you have the proper matched
record counts:

<img src="images/lab01/media/image33.png" style="width:7.5in;height:4.25556in" alt="Graphical user interface, text, application, email Description automatically generated" />

## Step 3: Merge your customer entities

On the Merge tab, if you need to combine any fields, ignore or rename
any fields do that. For the sample dataset, nothing needs to be done
except click **Save** and click **Run… Run only Merge**.

<img src="images/lab01/media/image34.png" style="width:7.5in;height:3.57222in" alt="Graphical user interface, text, application Description automatically generated" />

When merging is complete the screen will update with the current matched
records count:

<img src="images/lab01/media/image35.png" style="width:7.5in;height:2.77431in" alt="Graphical user interface, text, application Description automatically generated" />

# Exercise 3: Enrich customer entities

In this section you will enrich your customer entities in your Dynamics
365 Customer Insights environment using external sources and services.

## Step 1: Enrich with Interest affinities

On the Enrichment left nav click “Enrich my data” on the “Interests”
tile to enrich your data with interest affinities from people in a
similar demographic to your customers.

<img src="images/lab01/media/image36.png" style="width:6.61551in;height:6.02167in" alt="Graphical user interface, application Description automatically generated" />

On the overview tab, click Next

<img src="images/lab01/media/image37.png" style="width:7.5in;height:3.90069in" alt="Graphical user interface, text, application Description automatically generated" />

On the Interests tab choose “Internet & Telecom” from the dropdown (or
whatever industry makes sense for your business).

<img src="images/lab01/media/image38.png" style="width:7.5in;height:5.28889in" alt="Graphical user interface, application Description automatically generated" />

Click the + icon next to Search Engines, Internet Service Plans,
Teleconferencing, and Cable Services, then click Next.

<img src="images/lab01/media/image39.png" style="width:5.17781in;height:3.28171in" alt="Graphical user interface, application, Teams Description automatically generated" />

On the Preferences tab, review the settings and click Next.

<img src="images/lab01/media/image40.png" style="width:7.5in;height:6.86806in" alt="Graphical user interface, text, application Description automatically generated" />

On the Required data tab choose the Customer dataset and click Next:

<img src="images/lab01/media/image41.png" style="width:7.5in;height:4.51875in" alt="Graphical user interface, text, application, email Description automatically generated" />

On the Attribute mapping tab, follow the instructions. For the sample
dataset, ensure the Gender, Country/Region, and Postal code fields are
mapped to columns in your dataset. For you own dataset, follow the
documentation on the [required
fields](https://docs.microsoft.com/en-us/dynamics365/customer-insights/audience-insights/enrichment-microsoft#map-your-fields).
Click Next.

<img src="images/lab01/media/image42.png" style="width:7.5in;height:7.50972in" alt="Graphical user interface, application, email Description automatically generated" />

On the Review and run tab, review the settings, name your enrichment and
click “**Save enrichment**”.

<img src="images/lab01/media/image43.png" style="width:7.5in;height:7.63542in" alt="Graphical user interface, text, application Description automatically generated" />

Then click Run:

<img src="images/lab01/media/image44.png" style="width:7.5in;height:4.62917in" alt="Graphical user interface, text, application, email, Teams Description automatically generated" />

# Summary

In this lab, you loaded, mapped, merged and enriched customer entities
in Dynamics 365 Customer Insights. Later labs build on this work using
key Azure data services to add value to your customer data.
