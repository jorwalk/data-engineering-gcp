Simulating a Data Warehouse in the Cloud Using BigQuery and Dataflow
====================================================================

Overview
--------

In this advanced lab will learn to build several Data pipelines that ingest data from a fictional dataset and match customer orders to supplier parts in BigQuery, simulating a batch transformation

During the lab these components are used:

-   GCS - Google Cloud Storage
-   Dataflow - Google DataFlow
-   BigQuery - BigQuery tables

![1d087575954fe047.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/4374e42f0d944a19128c2bc1d283b041cdbd2ebf10d30360a3c86563e9253201.png)

Setup
-----

#### Before you click the Start Lab button

Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click Start Lab, shows how long Cloud resources will be made available to you.

This Qwiklabs hand-on lab lets you do the lab activities yourself in a real cloud environment, not in a simulation or demo environment. It does so by giving you new, temporary credentials that you use to sign in and access the Google Cloud Platform for the duration of the lab.

#### What you need

To complete this lab, you need:

-   Access to a standard internet browser (Chrome browser recommended).
-   Time to complete the lab.

*Note:* If you already have your own personal GCP account or project, do not use it for this lab.

#### How to start your lab and sign in to the Console

1.  Click the Start Lab button. If you need to pay for the lab, a pop-up opens for you to select your payment method. On the left, the Connection Details panel becomes populated with the temporary credentials that you must use for this lab.

    ![Open Google Console](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/77f1dfb0d993a3750cecf55a47a18f4b7a4be8475c91f9587f52f84c5fbf7f3f.png)

2.  Copy the username, and then click Open Google Console. The lab spins up resources, and then opens another tab that shows the Choose an account page.

    *Tip:* Open the tabs in separate windows, side-by-side.

3.  On the Choose an account page, click Use Another Account.

    ![Choose an account](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/790eb13e73e7d771a38893f74569475b088c8e1a281f14cdbf37e0d402f658fc.png)

4.  The Sign in page opens. Paste the username that you copied from the Connection Details panel. Then copy and paste the password.

    *Important:* You must use the credentials from the Connection Details panel. Do not use your Qwiklabs credentials. If you have your own GCP account, do not use it for this lab (avoids incurring charges).

5.  Click through the subsequent pages:

    -   Accept the terms and conditions.
    -   Do not add recovery options or two-factor authentication (because this is a temporary account).
    -   Do not sign up for free trials.

After a few moments, the GCP console opens in this tab.

Note: You can view the menu with a list of GCP Products and Services by clicking the Navigation menu at the top-left, next to "Google Cloud Platform". ![Cloud Console Menu](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/f6f4fbc4f971a0d3ff3ec2b427c8f464f141e079e7a5a2095420c1c9a06b4878.png)

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

In Cloud Shell, set a variable equal to your project id:

```
export PROJECT_ID=$(gcloud config get-value core/project)

```

```
gcloud config set project $PROJECT_ID

```

### Create Cloud Storage bucket

Use the make bucket command to create a new regional bucket in the us-central1 region within your project

```
gsutil mb -c regional -l us-central1 gs://$PROJECT_ID

```

### Set Compute Engine region

Set the compute region. This is the region where the Dataflow workers will run.

```
gcloud config set compute/region us-central1
```

### Clone the repository

Clone the professional services Github repository to the current directory in Cloud Shell:

```
git clone https://github.com/GoogleCloudPlatform/professional-services.git
```

Move to the data analytics subdirectory:

```
cd professional-services/examples/dataflow-python-examples/

```

Download the available schemas from the example storage bucket into the local directory named "schemas":

```
gsutil -m cp -r gs://python-dataflow-example/schemas/ /home/$USER/professional-services/examples/dataflow-python-examples/

```

Download the Python application code to benchmark the schemas:

```
gsutil cp gs://cloud-training/gsp291/python/data_generation_for_benchmarking.py /home/$USER/professional-services/examples/dataflow-python-examples/data_generation_for_benchmarking.py

```

### Create the BigQuery dataset

Create a dataset in BigQuery called "BigQueryFaker":

```
bq mk BigQueryFaker
```

This is where all of your tables will be loaded within BigQuery.

Note: To simplify the lab configuration, you will use pre-configured files available in a Google Cloud Storage bucket.

### Create the lineorders table

This command will create a table called "lineorders" in the BigQueryFaker dataset:

```
bq --location=US load --source_format=CSV --skip_leading_rows=1 BigQueryFaker.lineorders gs://cloud-training/gsp291/lineorders.csv /home/$USER/professional-services/examples/dataflow-python-examples/schemas/lineorder-schema.json

```

### Create the customers table

This command will create a table called "customers" in the BigQueryFaker dataset:

```
bq --location=US load --source_format=CSV --skip_leading_rows=1 --allow_jagged_rows --allow_quoted_newlines BigQueryFaker.customers gs://cloud-training/gsp291/customers.csv /home/$USER/professional-services/examples/dataflow-python-examples/schemas/customer-schema.json

```

As the source data contains null values, two new command line parameters are required (i.e. `--allow_jagged_rows`and `--allow_quoted_newlines`).

Build a batch (bound) Dataflow pipeline
---------------------------------------

In this section you will create an append-only Dataflow which will ingest data into the BigQuery table. You can use the built in code editor which will allow you to view and edit the code in the GCP console.

![3b1ddc75045cae6b.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/c6ed9934e099d0881d94335d3eb98937f0c8a5d1623d53c24acb4718e6077833.png)

1.  Run the following to configure the Python environment and add the software components necessary for the lab:

```
{ sudo pip install --upgrade virtualenv; sudo pip install -U pip; virtualenv -p `which python2.7` dataflow-env; source dataflow-env/bin/activate; pip install -r /home/$USER/professional-services/examples/dataflow-python-examples/requirements.txt; } > gsp291.out

```

Batch updating the data warehouse
---------------------------------

Next you'll go back to Cloud Shell to generate more data for the tables.

1.  Generate data for the Parts table:

```
nohup python /home/$USER/professional-services/examples/dataflow-python-examples/data_generation_for_benchmarking.py\
--num_records=1000\
--schema_file=/home/$USER/professional-services/examples/dataflow-python-examples/schemas/part-schema.json\
--p_null=0.05\
--output_bq_table=BigQueryFaker.parts\
--project=$PROJECT_ID\
--requirements_file=/home/$USER/professional-services/examples/dataflow-python-examples/requirements.txt\
--disk_size_gb=30\
--worker_machine_type=n1-highcpu-2\
--runner=DataflowRunner\
--staging_location=gs://$PROJECT_ID/test\
--temp_location=gs://$PROJECT_ID/temp\
--save_main_session > parts-schema.log &

```

1.  Generate data for a Supplier table:

```
nohup python /home/$USER/professional-services/examples/dataflow-python-examples/data_generation_for_benchmarking.py\
--num_records=500\
--n_keys=500\
--schema_file=/home/$USER/professional-services/examples/dataflow-python-examples/schemas/supplier-schema.json\
--p_null=0.05\
--output_bq_table=BigQueryFaker.suppliers\
--project=$PROJECT_ID\
--requirements_file=/home/$USER/professional-services/examples/dataflow-python-examples/requirements.txt\
--disk_size_gb=30\
--worker_machine_type=n1-highcpu-2\
--runner=DataflowRunner\
--staging_location=gs://$PROJECT_ID/test\
--temp_location=gs://$PROJECT_ID/temp\
--save_main_session > supplier-schema.log &

```

These processes will run in the background. Monitor the job progress in Dataflow. You may need to wait a couple of minutes to see the jobs appear on the Dataflow page. You can refresh the view by clicking on Dataflow in the upper left corner to see the most accurate information.

At this point there are two Dataflow batch jobs being processed.

![db88da8b552a103d.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/a1a80c78aa331c51dcbd0f05d278a8f3d5caac84588dcda80b3d043e69d61161.png)

Click on each job to monitor its progress if you want. It will take 5-10 minutes for each of them to complete.

After the Dataflow jobs show a Successful status, in the Cloud Shell confirm there are four new tables in BiqQuery for customers, lineorders, parts, and suppliers by running the following:

```
bq ls BigQueryFaker

```

![f2ad57444c720416.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/c14ae1f995b33e8b806af05e96d8b835352bba8e835964376e8d65e4ca70bf34.png)

1.  Now navigate to BigQuery. In the BigQuery console, click on the project name and then Create Dataset.

2.  Call the new dataset "BusinessViews" and set the data location to the US. This dataset enables business users to have read only access to the data. The new dataset should look like the illustration below:

![e1c26ac3655e94d9.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/817effd977804aa31322ce1294a93b8f28ca1220dfce1b079985ac4dae00014b.png)

Then click Create Dataset button at the bottom.

1.  Add the following query into the Query editor console, replacing [PROJECT_ID] with your Project ID (in four places):

```
#standardSQL
SELECT
  *
FROM (
  SELECT
    *
  FROM
    `[PROJECT_ID].BigQueryFaker.lineorders` lo
  FULL OUTER JOIN
    `[PROJECT_ID].BigQueryFaker.parts` p
  ON
    lo.lo_part_key=p.p_part_key
  FULL OUTER JOIN
    `[PROJECT_ID].BigQueryFaker.customers` c
  ON
    lo.lo_cust_key = c.c_cust_key
  FULL OUTER JOIN
    `[PROJECT_ID].BigQueryFaker.suppliers` s
  ON
    lo.lo_supp_key = s.s_supp_key )
LIMIT 100
```

1.  Now click Save View. Name the View "v_orderset", and change the Destination name to your newly created BusinessViews dataset. Click Save.

2.  Query the view created for the customers (`p_name`) who ordered the most parts (`count_part`) since 2010, replacing the [PROJECT_ID] with your Project ID, then click Run query:

```
SELECT
  p_name,
  COUNT(lo_part_key) AS count_part
FROM
  `[PROJECT_ID].BusinessViews.v_orderset`
WHERE
  lo_orderdate >= '2010-01-01' AND
  p_name <> 'null'
GROUP BY
  p_name
ORDER BY
  count_part DESC
LIMIT 100
```

Example output from running the above query:

![cc8eb29276a44741.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/efa578ac0a1bb27ee7f36171eab919e0c0c3179fa6ef3647f11bcffda8a78351.png)we

A batch Dataflow pipeline has been successfully deployed to create a data warehouse in BiqQuery.

Build a streaming (unbound) Dataflow pipeline
---------------------------------------------

In this section you create a streaming Dataflow which ingests random data generated by [Faker](https://github.com/joke2k/faker) and updates a new BigQuery table.

![fe7a7d13f4b3c409.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/c41dfc034b62861e782f8c51d223a20e93ffde0e19206288e6e0d5ae0213dc49.png)

Dataflow provides a number of templates to simplify the process for exchanging data between services such as PubSub and BigQuery. In this example, you use an existing template to connect between the two services and perform processing.

### Create a Pub/Sub topic

1.  In the Console, go to Pub/Sub > Topics. Click on Create a Topic.

2.  Create a new topic named quotes in Pub/Sub to integrate with BigQuery via Dataflow. You'll just type "quotes" at the end of the topic name. The created topic should look similar to the illustration below:

![a40d28b0f953871c.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/39a8524bc095ee2c26bcce395fe6141365044980141a5968a8a60003540f76ad.png)

Now go to Storage and add a folder to your existing storage bucket and call it "quotes". You'll reference the destination, gs://<bucket_name/quotes, it in the next command.

Go back to Dataflow and click Create job from template. This will enable a Pub/Sub to BigQuery data exchange.

Create your template with the following:

-   Job name: quotations
-   Cloud Dataflow template: PubSub to BigQuery
-   Cloud PubSub input topic: projects/PROJECT_ID/topics/quotes
-   BigQuery output table: PROJECT_ID:BigQueryFaker.quotations
-   Temporary Location: gs://<bucket_name>/quotes

A datasource to supply realtime information to the Data Warehouse is created as part of this project setup.

An example of a completed template form is displayed below:

![1140b3b3f36eda37.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/9b7e279b4b53633bcf48adc65cc94c976e2b1afadeee93807f76318ab421128c.png)

Once the Dataflow template is complete, select Run Job to start the service.

To test the streaming of data, a new schema needs to be defined. You will next create the following schema in BigQuery:

![62c52293ccbda3c6.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/762eddc29a4c52f23ceee13475bec4af0e0afa3b25c9d3c3970346e6171c2a57.png)

In the BigQuery console, click on the BigQueryFaker dataset, click Create Table. Name the table "quotations". Use the +Add field button to add all of the fields (i.e. email, job, country and phone) illustrated above.

Click Create table when you're done.

1.  Back in Cloud Shell, download the quotes.py application to the current directory :

```
gsutil cp gs://cloud-training/gsp291/quotes.py quotes.py

```

Note: The Python code will populate the quotations table with fake data (i.e. email, job, country and phone number). Entering pseudo data in this way enables the process to be quickly verified.

1.  From Cloud Shell install the necessary Faker package:

```
pip install Faker

```

Now run the program to generate data for the quotation table:

```
python quotes.py

```

The command will send a stream of randomly generated quotes to BigQuery. You will see a series of message IDs displayed in Cloud Shell.

1.  Go to BigQuery and run this query to display the updated quotations table, replacing PROJECT_ID with your Project ID:

```
SELECT * FROM `PROJECT_ID.BigQueryFaker.quotations` LIMIT 100

```

Click Run query.

This will confirm that the quotation table has been updated with information based on the fields created previously.

![fe93ea7650785288.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/b3bd1c4cfb046ef74bd73b2dbddc0a0d0b0106c283f3214eed6b7a92d65ebd55.png)