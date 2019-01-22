Working with Google Cloud Dataprep
==================================


Overview
--------

Cloud Dataprep is Google's self-service data preparation tool built in collaboration with Trifacta. In this lab you will learn how to clean and enrich multiple datasets using Cloud Dataprep. The lab exercises are based on a mock use case scenario.

#### Use Case Scenario:

You work for a technical services company that sells three monthly subscription products:

-   Silver (price: $9.99/month)
-   Gold (price: $14.99/month)
-   Platinum (price: $29.99/month)

The company occasionally offers promotional discounts, so some product prices may be slightly lower than those listed above. Your overall goal is to provide an analysis of sales activity by zip code over the course of three years.

To do this you'll need to join your customer contact datasource (where the zip code information resides) with sales data from your purchases datasource. Once you've joined the data, you'll aggregate the results.

#### What you'll learn

-   Cleaning and profiling data with Cloud Dataprep

-   Combining multiple datasets using Cloud Dataprep

-   Computing the results of formulas in Cloud Dataprep

#### What you'll need

-   A Google Cloud Platform Project

-   Chrome internet browser - this lab can only successfully run in Chrome

Open Google Cloud Dataprep
--------------------------

In the Console, select Navigation menu > Dataprep:

![navigation.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/4cfa551be0670ddaa7890c1ec44bc2ff449a95056a4831703b52069f046645d7.png)

1.  To get into Dataprep, check that you agree to Google Dataprep Terms of Service, and then click Accept.

2.  Click the checkbox and then click Agree and Continuewhen prompted to share account information with Trifacta.

3.  Click Allow to give Trifacta access to your project:

4.  Click your Qwiklabs credentials to sign in:

![sign-in.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/5474c1d90395f0ccce2ff4fba5bd2b54dc8095ef47b572b95b7f916d92f5665e.png)

1.  Click Allow. Check the box and click Accept to agree to Trifacta Terms of Service.

2.  Click Continue to set the storage location and complete the project setup.

Retrieve dataset files
----------------------

In this section you add the sales activity files to a storage bucket that Dataprep created for you.

1.  Go back to the Google Cloud Console.

If you had closed the Google Cloud Console you can open it by clicking the GCP icon in the bottom left corner).

1.  Get your bucket name. Select Navigation menu > Storage > Browser.

Note the Dataprep bucket name to use in the next step.

1.  In the Cloud Shell command line, execute the following command, substituting `[YOUR-BUCKET-NAME]` with the Dataprep bucket name:

```
gsutil cp -r gs://spls/gsp050 gs://[YOUR-BUCKET-NAME]

```

You should receive a similar output:

```
Copying gs://spls/gsp050/lab_customers.csv [Content-Type=text/csv]...
\ [4 files][  8.5 MiB/  8.5 MiB]
Operation completed over 4 objects/8.5 MiB.

```

Create a Flow
-------------

Go back to the Cloud Dataprep tab. To wrangle your data, you need to create a Flow. A Flow is comprised of a set of related datasets and the connections between them.

1.  Click Create Flow in the right-hand corner:

![create-flow.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/43d8de8ccbd3a6b3da59957bef36f3967e8df6f6b215e1565786c3cd2047e3fb.png)

1.  Name your Flow `Dataprep Qwiklab`, leave the flow description blank, then click Create.

At this point your Flow is an empty container. You need to import data to Dataprep and then add the imported data to your Flow.

1.  Click Import & Add Datasets to browse for the data files you uploaded to Google Cloud Platform:

![import-datasets.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/d04ca9597e35bd78a40fe7b3d5c63799a02798d750daa537f9adec41920f5dd4.png)

1.  In the left-hand navigation, click GCS > dataprep-staging-xxx... > gsp050 to access the sample data we stored in the previous section:

![data.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/cf56eda42908b4758d720ab917f1a151d3aa83c93f3a461d5b70ffe2da2e9603.png)

1.  Click each + next to each file listed. When you click on a file, it will move to the right side of the screen. Click Import & Add to Flow to add the datasets to your Flow:

![img/import_and_add_to_flow.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/a7f165b2bed71608a846774b4a702c5b9e378e643d54b68a1eb74a612ea7bc73.png)

Cloud Dataprep brings you back to the Flow View page, which now contains your sample data.

Clean Customer Data
-------------------

Now that you have a Flow, design a data preparation recipe to clean the customers dataset.

1.  Click the icon for the `lab_customers.csv` then click Add new Recipe:

![clean-data.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/2054ae579dd989f5aa069901a60d9537b92a4b9ac765ae25fa79b502c987d741.png)

1.  Click Edit Recipe.

Cloud Dataprep opens the "Transformer Grid". This is a worksheet-like interface where you can design the steps in your data preparation recipe. When you open the Transformer Grid, Cloud Dataprep automatically profiles the contents of your dataset and generates column-level histograms and data quality indicators. This profile information can be used to guide your data preparation process.

1.  Scroll all the way to the right to the start_date column. Examine the horizontal bar at the top column:

![700ec78f4f165691.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/12d47d7702deae9ed1bee35c9767703005890b26d0de95f95770ffa88fcdaa6c.png)

This is the data quality bar. The green part represent valid values, the black represents missing or null values. Clicking on the sections of the data quality bar will generate suggestions that contain data quality conditionals. These conditionals test whether each record is valid, empty, or invalid, depending on the section of the bar that you clicked.

#### Apply a filter to remove contacts

Use start_date and end_date as a filter to remove invalid contacts.

1.  Apply a filter to remove contacts where the start_datecolumn is empty. Click on the black part of the data quality bar.

Cloud Dataprep generates a list of suggested transformations based on your selection.

1.  Click Add on the "Delete rows with missing values in `start_date`":

![631d899cc40069cb.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/305a6a3f39dc70410a6b2573fa71fd11215add5e4105de2b8dfa0b4d6e1a3586.png)

Cloud Dataprep updates the data to show you a preview of this transformation. The rows that were highlighted in red have been removed from your dataset.

#### Fill in missing values

Look at the end_date column. Based on the data quality bar, there are a large number of rows with missing values. To easily work with this column, you'll insert an empty value--January 01, 2050--in those empty rows.

1.  Click the black section of the data quality bar for the end_date column. This will generate another set of suggested transformations.

2.  Click the Set missing values to NULL() card, then click Edit:

![a9002a8e55742050.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/cecdda5281c0295f6cdae910fcc580acff1329702866f7292937919939b85ca0.png)

This opens the Add Step builder. Cloud Dataprep's suggested transformation has already been populated, but you can make adjustments to the code.

1.  Enter the following into the Formula box:

```
ifmissing($col, '2050/01/01')

```

1.  Click Add:

![formula.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/0fa25b87dd43894c73aac5d4fa3c6452a7a1fcfab7ae369145431bdc89bf3849.png)

Now the data quality problems in the lab_customersdataset have been addressed and the black part of the data quality bar is gone.

Union Multiple Transactions Datasets
------------------------------------

Now switch gears and work on the transactions datasets.

Click on the `DATAPREP QWIKLAB` flow name at the top of the screen:

![flow-name.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/79c45fd6ed98f370cb3378dea1f72c0e4daf14f1b2785ce615ba475d85923b4b.png)

This brings you back to the Flow view.

Create a single dataset that unions the transactions datasets from 2013, 2014, and 2015.

1.  Click on the `lab_2013_transactions` dataset. On the Details panel, click Add new Recipe.

2.  Click Add new Recipe once more.

Cloud Dataprep creates a new recipe and wrangled dataset named `lab_2013_transactions - 2`:

![transaction-recipe.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/3bf403c2c3079394cdb076d2c754aa489f0725623864d964eafa88bb25474ef2.png)

1.  Click on this new wrangled dataset, in the Details panel click on the three dots icon. From the drop-down menu, choose Edit name and description.

2.  Change the name to "Combined transactions" and click OK.

3.  From the Details panel, click Edit Recipe. This opens the new "Combined transactions" recipe in the Transformer Grid. Notice that the data in the grid is the structured data from the `lab_2013_transactions.csv` dataset.

### Combine multiple datasets with the same schema using a Union transform.

1.  Click the Recipe icon at the top.

![recipe_icon.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/d4b2dada66e1c7dc56e78a083c92145c36c2d1eeccf62f0ddce3d261468b24ae.png)

1.  Click Add New Step.

2.  Type in "Union dataset" in the search field then click on the result to get to the Union tool.

The Union Output field displays the output schema for your dataset. Each box represents a column. Cloud Dataprep bases the output schema on the schema of the dataset from which you initiated the union transform. In this case, the columns in the "Combined transactions" dataset determine the columns that will appear in the combined output.

1.  Click Add Data:

![add_data.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/8665a23c886ca9ed125bdeb1f904cfdc6de66819fb3cc920bcbe0e35b8ab8bdf.png)

1.  Check the `lab_2014_transactions` and `lab_2015_transactions` datasets. From the drop down select 'Align By Name' and click Apply:

![union.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/17bcbe51f5bf589e004f7030b423ee1b5c1e1495c09f237975f2767c9bb1445c.png)

1.  Examine the column-to-column mappings. Click Add to Recipe to combine all three datasets:

![4b940533eb65fbfd.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/f7b526f24d484fa269dbfd2a04cb8797c925d7f466337b87c25acfc24bc02da0.png)

After adding the union to your script, look at the `transaction_date` column:

![5bb5d080581c2f17.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/3b7b291a8d9b80aec36094db2f785d80f24e695e7a1f325c5743dd7b555dc69e.png)

This dataset now includes records from January 2013 through December 2015.

1.  Click on the `DATAPREP QWIKLAB` flow name to return to the Flow View.

The flow visualization is updated to show how the three transactions datasets combine to form the Combined Transactions dataset:

![updated-flow.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/55edea6a6cea8bd475cbe4c53d63dc1ca48ab1aa9d111c1b29e624d754bee6f1.png)

Join Transactions Data to Customers Data
----------------------------------------

Now that the datasets are combined, you will enrich the transactions data with information about where each purchase was made. To do this, join the customer data to the transactions data. When performing a join, treat the larger dataset as the master dataset, or the "left side" of the join. The smaller dataset should be the detail dataset, or the "right side" of the join. In Cloud Dataprep, the dataset from which you initiate a join automatically becomes the master dataset.

1.  You should already be on the Combined Transactionswrangled dataset. In the Details panel, click Edit Recipe.

2.  Click on Recipe, then New Step:

![new-step.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/8d76d9242706c9f7bf5aaa9dad5732c5b6710090a51f6d5d402a21e83664fa45.png)

1.  Search for 'Join' and click 'Join Datasets'.

2.  Check next to the `lab_customers` dataset, then click Accept.

3.  Cloud Dataprep tries to automatically infer the correct join keys. For these datasets, Cloud Dataprep chose an inside join for customer_id.

This means the output dataset will be those records that have the same customer_id.

1.  Click Next.

2.  In the Output Columns panel, click on the customer_id (current) field to remove that column from the output dataset, and then click on the following fields to add those columns to the Join:

-   `customer_id (current)`
-   `transaction_date`
-   `ticket_price`
-   `product`
-   `address_state`
-   `address_zip`
-   `region`
-   `start_date`
-   `end_date`

Your results will look like this:

![transactions.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/a11371cfaeba0d7fa93544691d4b64e981ea0ca964d267daf9e311d3473173f8.png)

1.  Click Review to preview the result of your join in the Transformer Grid.

2.  Click Add to Recipe.

Compute Aggregate Sales Figures
-------------------------------

As a final step, aggregate the transactions by product, year, and state.

1.  Create a new column that contains the year of each transaction.

2.  Click the drop-down arrow next to transaction_date > Extract > Datetime > Year (YYYY).

New columns are created for the year of each transaction.

1.  Click Add:

![add-it-up.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/eaa1cdf6f829bedb5a560819af55b214290552deb0ff69d3db13b96cd23171b0.png)

Make a pivot table
------------------

1.  Click New Step in the top right corner and select Pivot tablefrom the drop down list.

2.  In Row labels, add the following:

-   `product`

-   `address_state`

-   `transaction_date`

1.  In the values box, add the following:

-   `MIN(ticket_price)`
-   `MAX(ticket_price)`
-   `AVERAGE(ticket_price)`

![pivot.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/ded410aa3474cea48878210fc3e2ff91333b78d33808ab373c4d73c77100e27b.png)

1.  Click Add.

Your pivot table opens.

Create a Results File on Google Cloud Storage
---------------------------------------------

You just finished preparing your data and you're ready to produce a results file in Google Cloud Storage. Cloud Dataprep executes your data transformation recipe to produce your output file using the Dataflow engine.

Click Run Job in the top right of the Transformer Grid.

In the Jobs panel, you can configure your output settings. By default, Cloud Dataprep will create a CSV file on GCS.

Now click Run Job in the bottom left corner to kick off your Dataflow job. This will take a few minutes. You can see the job processing on the Dataprep "Jobs" page. When it completes, you'll see a success message that resembles the following:

![job-done.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/cf490dab3a91e4d1e3e3a3fde30c013bc7e1fb8c422e4bc19efdb07378965713.png)

Hover over the finished job and click View Results to see your data organized. It should resemble the following:

![output.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/8187df2b75e6f7a5dc4e7dfc6e9a9cfbfca33842b2326b8a421f9ec52de3c5ef.png)

Your results are stored in the Cloud Storage automatically: go to Storage > Browser > dataprep-staging-xxx... > Your Qwiklabs Account > jobrun to see the folder that contains it.

Cloud Dataprep is that simple! It's easy to cleanse and enrich multiple data sources using an intuitive, visual interface.