Analyzing Natality Data Using Datalab and BigQuery
==================================================

Overview
--------

In this lab you analyze a large (137 million rows) natality dataset using Google BigQuery and Cloud Datalab.

If you are not yet familiar with Datalab, here is a graphical cheat sheet for the main Datalab functionality:

![369bf7e045b084ed.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/0341c149ceaa889ea07f0c50f2c5a25a4b93e36c5900fbf672b8369393ea06ff.png)

### What you learn

In this lab, you:

-   Launch Cloud Datalab
-   Invoke a BigQuery query
-   Create charts in Datalab
-   Export data for machine learning

This lab illustrates how you can carry out data exploration of large datasets, but continue to use familiar tools like Pandas and Juypter. The trick is to do the first part of your aggregation in BigQuery, get back a Pandas DataFrame, then work with the smaller Pandas DataFrame locally. Datalab provides a managed Jupyter experience, so you don't need to run notebook servers yourself.

### Activate Google Cloud Shell

Google Cloud Shell is a virtual machine that is loaded with development tools. It offers a persistent 5GB home directory and runs on the Google Cloud. Google Cloud Shell provides command-line access to your GCP resources.

1.  In GCP console, on the top right toolbar, click the Open Cloud Shell button.

    ![Cloud Shell icon](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/bdd6397bf6a7f59197c396bf64c6f56a0a578511a8cec39a747511711f2d8404.png)

2.  In the dialog box that opens, click START CLOUD SHELL:

    ![Start Cloud Shell](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/2b3aa073e2de48f786ec9497068b23b4bb0479e055c72b245c68c629a3eb6bb5.png)

    You can click "START CLOUD SHELL" immediately when the dialog box opens.

It takes a few moments to provision and connect to the environment. When you are connected, you are already authenticated, and the project is set to your *PROJECT_ID*. For example:

![Cloud Shell Terminal](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/86630ad16e354f193edb46d0cae0cff60eb4bc27416a3212fb9da22367f86d89.png)

gcloud is the command-line tool for Google Cloud Platform. It comes pre-installed on Cloud Shell and supports tab-completion.

You can list the active account name with this command:

```
gcloud auth list

```

Output:

```
Credentialed accounts:
 - <myaccount>@<mydomain>.com (active)
```

Example output:

```
Credentialed accounts:
 - google1623327_student@qwiklabs.net
```

You can list the project ID with this command:

```
gcloud config list project

```

Output:

```
[core]
project = <project_ID>
```

Example output:

```
[core]
project = qwiklabs-gcp-44776a13dea667a6
```

Full documentation of gcloud is available on [Google Cloud gcloud Overview](https://cloud.google.com/sdk/gcloud).

Launch Cloud Datalab
--------------------

To launch Datalab, you first create a VM instance and SSH into it. In the interest of time, we'll give you the code to do this.

In the Cloud Shell command line, type the following command to create a Datalab:

```
datalab create babyweight --zone us-central1-a --no-create-repository

```

The Datalab takes about 5 minutes to start.

Click Y to continue, and Enter through the passphrase questions.

Move on to the next step while Datalab is launching.

Invoke BigQuery
---------------

### Open BigQuery Console

In the Google Cloud Console, select Navigation menu > BigQuery:

![BigQuery_menu.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/430517e9a9607573e021386a065801dab9ea1a4a1051400d925df189c0f4eae7.png)

When the console opens, change to the Classic UI.

Click on Go to Classic UI.

![BigQuery_classicUI.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/8297f980d0f0604aaac740e81c1dd0fd438e2ec5306f6e783f65a112cefffc4f.png)

The BigQuery console opens in a new browser tab. Select your Qwiklabs project by clicking the Project Down arrow, selecting Switch to Project > Your Qwiklab Project:

![BigQuery_resources_chooseProj.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/7a3a1ad9deb7c6a8d1868d222f7b84b815879b63b1cb63fe460f570b69c551e0.png)

The BigQuery console is ready to go! ![BigQuery_console.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/3af06038df9ab6190fbb9d5d54b23d44ab1bb9791691d7823c8180709d89b4bb.png)

In the BigQuery Console, click on Compose Query.

![7f2d6198bc3ccbf0.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/25ea979b35171b40ee314f262dc755b221c026862e7d43b0240063103b2422ef.png)

Click on Show Options and uncheck Legacy SQL (you will be using Standard SQL).

In the query textbox, enter:

```
SELECT
  plurality,
  COUNT(1) AS num_babies,
  AVG(weight_pounds) AS ave_weight
FROM
  `bigquery-public-data.samples.natality`
WHERE
  year > 2000 AND year < 2005
GROUP BY
  plurality
```

Now click Run Query.

Review the result. How many triplets were born in the US between 2000 and 2005?

Visualize data in Cloud Datalab
-------------------------------

Switch back to your Cloud Shell window. If necessary, wait for Datalab to finish launching. Datalab is ready when you see a message prompting you to use "Web Preview".

You should also have earned a 5/10 score. You'll see this under the timer in the lab:

![score5.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/33155a1d31bd095d9b3931f2abc08213286df4bdf3400e5234b4c6648086eec5.png)

Click on the Web Preview icon on the top-left corner of the Cloud Shell ribbon and click on "Change Port".

Note: If you cannot see the Web Preview button at the top of Cloud Shell, click on Navigation menu (hamburger) in the upper left corner.

![9c8bbf6d12e3a0c4.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/481aba73bfe1f58ee109a0b9b20e2270893f2ecb13ad3b1e318d743dd2fab69a.png)

Type in 8081.

![9f5f95b5b31c241f.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/5b58e5dcd6542530224db51f2735508cde041e8d981bd510c4e245bff22c6f78.png)

Click "Change and Preview".

This opens Datalab in a new browser window. If the page comes up blank (a white screen) reload the page.

In Datalab, start a new notebook by clicking on the +Notebookicon.

Start by updating to the latest version of the BigQuery Python Client Library. Enter the following into the first cell of the notebook, and click the Run button at the top of the screen.

```
!pip install --upgrade google-cloud-bigquery

```

This cell produces quite a bit of text output, which you can hide by collapsing the cell:

![datalab-collapse_code.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/158ec57293c6bf7911603a9f1513ee95f067cacfaf6347d1f7019f26c1abb437.png)

Next, insert the following code to import the BigQuery Python Client Library and initialize a client. The BigQuery client will be used to send and receive messages from the BigQuery API.

```
from google.cloud import bigquery

client = bigquery.Client()
```

Run the cell with Shift + Enter.

Add the following into the next cell of the notebook to run a query on the BigQuery natality public dataset, which describes all United States births registered from 1969 to 2008. This query returns the annual count of plural births by plurality (2 for twins, 3 for triplets, etc.).

```
sql = """
  SELECT
    plurality,
    COUNT(1) AS count,
    year
  FROM
    `bigquery-public-data.samples.natality`
  WHERE
    NOT IS_NAN(plurality) AND plurality > 1
  GROUP BY
    plurality, year
  ORDER BY
    count DESC
"""
df = client.query(sql).to_dataframe()
df.head()
```

Run the cell with Shift + Enter.

You just ran a query in the cloud! The head of the DataFrame (the first 5 rows) is displayed below the code cell. Full results are available for further analysis in a Pandas DataFrame.

Insert the following code into the next cell to pivot the data and create a stacked bar chart of the count of plural births over time.

```
pivot_table = df.pivot(index='year', columns='plurality', values='count')
pivot_table.plot(kind='bar', stacked=True, figsize=(15,7));
```

Next, take a look at baby weight by gender. In the next cell, enter the following, then run the cell:

```
sql = """
  SELECT
    is_male,
    AVG(weight_pounds) AS ave_weight
  FROM
    `bigquery-public-data.samples.natality`
  GROUP BY
    is_male
"""
df = client.query(sql).to_dataframe()
df.plot(x='is_male', y='ave_weight', kind='bar');
```

Are male babies heavier or lighter than female babies? Does this align with your expectations?

For your last visualization, see how the baby's weight fluctuates according to the number of gestation weeks. Enter the following into the next cell and run it:

```
sql = """
  SELECT
    gestation_weeks,
    AVG(weight_pounds) AS ave_weight
  FROM
    `bigquery-public-data.samples.natality`
  WHERE
    NOT IS_NAN(gestation_weeks) AND gestation_weeks <> 99
  GROUP BY
    gestation_weeks
  ORDER BY
    gestation_weeks
"""
df = client.query(sql).to_dataframe()
df.plot(x='gestation_weeks', y='ave_weight', kind='bar');
```

Note: Because the gestation_weeks field allows null values and stores unknown values as 99, this query excludes records where gestation_weeks is null or 99.

Now you have a chart that shows how the weight of the baby relates to the number of weeks of gestation.

Your score should now be 10/10. If not, click the green Score Box and click Run Step to check the progress of each step.

![RunStep.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/67d8178baef61bce8ab106370e1d56ce00a38de789ebe118a6d8d417236d54fe.png)


Waiting for SSH key to propagate.

Error: Could not connect to Cloud Shell on port 8081.
Ensure your server is listening on port 8081 and try again.