Launching Dataproc Jobs with Cloud Composer
===========================================

Overview
--------

In this lab you'll use Google Cloud Composer to automate the transform and load steps of an ETL data pipeline. The pipeline will create a Dataproc cluster, perform transformations on extracted data (via a Dataproc PySpark job), and then upload the results to BigQuery. You'll then trigger this pipeline using either:

-   HTTP POST request to a Cloud Composer endpoint
-   Recurring schedule (similar to cron job)

Cloud Composer workflows are comprised of [DAGs (Directed Acyclic Graphs)](https://airflow.incubator.apache.org/concepts.html#dags). You will create your own DAG, including design considerations, as well as implementation details, to ensure that your prototype meets the requirements.

### What you will build

You're going to build an Apache Airflow DAG that will:

-   Begin running when triggered from an on-prem POST request
-   Spin up a Dataproc cluster
-   Run a Pyspark job on cluster
-   Tear down cluster when job completes
-   Upload Pyspark output to BigQuery
-   Remove any remaining intermediate files

![78cbe88a24d8f0b0.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/56058a36ddf8a382b0d436b7e980e1788b51546b5091bbbc88dc5f92ef6698b2.png)

### Project Setup

Copy and paste the following into Cloud Shell to check out the source code from [our professional services github](https://github.com/GoogleCloudPlatform/professional-services/blob/master/data-analytics/dataflow-python-examples/README.md):

```
git clone https://github.com/GoogleCloudPlatform/professional-services.git ~/professional-services
```

Set a variable equal to your project id, replacing [PROJECT_ID] with the GCP Project ID for your lab :

```
export PROJECT=[PROJECT-ID]
```

```
gcloud config set project $PROJECT

```

Looking for your `PROJECT_ID`? Check the Project Info the Console dashboard.

### Create Cloud Storage Bucket

Use the make bucket command to create a new regional bucket in us-central1 within your project

```
gsutil mb -c regional -l us-central1 gs://$PROJECT
```

### Create the BigQuery Dataset

In Cloud Shell create a dataset in BigQuery:

```
bq mk ComposerDemo
```

This is where all of your tables will be loaded within BigQuery.

Export the Data
---------------

Typically, data would be uploaded to Google Cloud from an on-prem location. This lab will use the [New York City Yellow Cab](https://bigquery.cloud.google.com/table/nyc-tlc:yellow.trips?pli=1) BigQuery public table dump to simulate that. Follow the link to take a look at the data.

![e8791fbe430f4098.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/08b96916561f82d98951867ab6347949d144a7de14705b977a0bb0332a6a4126.png)

To export a small sample of the taxi data, execute the following steps in Cloud Shell.

First, you'll select 1 day of records from the public table to add into a temp table with the following `gcloud` command:

```
bq query --destination_table=ComposerDemo.nyc_tlc_yellow_nyd_2015 --use_legacy_sql=false --allow_large_results "SELECT * FROM \`nyc-tlc.yellow.trips\` WHERE EXTRACT(DATE FROM dropoff_datetime) = '2015-01-01'"

```

Then export the temp table to a timestamped Google Cloud Storage path:

```
export EXPORT_TS=`date "+%Y-%m-%dT%H%M%S"` && bq extract --destination_format=NEWLINE_DELIMITED_JSON\
ComposerDemo.nyc_tlc_yellow_nyd_2015\
gs://$PROJECT/cloud-composer-lab/raw-$EXPORT_TS/nyc-tlc-yellow-*.json

```

Now delete the temp table:

```
bq rm -f -t ComposerDemo.nyc_tlc_yellow_nyd_2015

```

> Note: You will timestamp the data to avoid collision. The `dag_trigger.py`, used later in this lab, is dependent on the `$EXPORT_TS` bash environment variable.

Create a Composer Environment
-----------------------------


Next you will create a Cloud Composer Environment for this DAG. Cloud Composer will spin up the necessary compute resources to host your DAG and install the necessary software.

![7d78e9a6c6a8233d.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/94dd70cae57d444c8628621dd263dae2ca6a6989df657b05799a0470e9f4f2f1.png)

Now you'll create a Composer environment using `gcloud` commands.

In Cloud Shell create the environment by running:

```
gcloud composer environments create demo-ephemeral-dataproc\
--location us-central1\
--zone us-central1-b\
--machine-type n1-standard-2\
--disk-size 20

```

This will take about 15 minutes to provision your Composer Environment. You can monitor the progress in the Console. In the Navigation menu, click on Composer:

![6d5cc1e126272384.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/88e73ca76e28d018015f77615bce17dfb60db7b7bdb74061700ab6cb821781f2.png)

Note: Cloud Composer exposes the Airflow web UI on a public IP address. For many organizations this is a security concern. While you're waiting for your environment to build, you can familiarize yourself with [Identity Aware Proxy](https://cloud.google.com/iap/docs/). This is a method of authenticating with Google Cloud services and how your access to the Airflow UI on Cloud Composer is secured.

You can also read ahead on the [Airflow Variables](https://airflow.apache.org/concepts.html#variables) - you'll be creating them next.

Once the environment is created, set Airflow Variables in the Composer Environment you just created:

```
gcloud composer environments run demo-ephemeral-dataproc\
--location=us-central1 variables --\
--set gcp_project $PROJECT

```

```
gcloud composer environments run demo-ephemeral-dataproc\
--location=us-central1 variables --\
--set gce_zone us-central1-b

```

```
gcloud composer environments run demo-ephemeral-dataproc\
--location=us-central1 variables --\
--set gcs_bucket $PROJECT

```

```
gcloud composer environments run demo-ephemeral-dataproc\
--location=us-central1 variables --\
--set bq_output_table $PROJECT:ComposerDemo.nyc_tlc_yellow_trips

```

### Copy the Python requirements for making IAP requests

Back in Cloud Shell, run these commands to install the necessary requirements for making [Identity Aware Proxy](https://cloud.google.com/iap/docs/) requests with `dag_trigger.py`. This includes grabbing the latest version of the `make_iap_request.py` script from the `python-docs-samples` repo.

```
(curl https://raw.githubusercontent.com/GoogleCloudPlatform/python-docs-samples/master/iap/requirements.txt; echo 'tzlocal>=1.5.1') >> ~/professional-services/examples/cloud-composer-examples/composer_http_post_example/iap_requirements.txt
```

```
curl https://raw.githubusercontent.com/GoogleCloudPlatform/python-docs-samples/master/iap/make_iap_request.py >> ~/professional-services/examples/cloud-composer-examples/composer_http_post_example/make_iap_request.py
```

Next, copy the prepared schema file for your enhanced data from this public Cloud Storage bucket:

```
gsutil cp gs://python-dataflow-example/schemas/nyc-tlc-yellow.json gs://$PROJECT/schemas/nyc-tlc-yellow.json
```

You can take a look at this file by running:

```
gsutil cat gs://$PROJECT/schemas/nyc-tlc-yellow.json
```

Notice that it is identical to the schema of the `nyc-tlc:yellow.trips` source table with 1 additional column named "average_speed": a FLOAT which will house the average speed calculated by your Spark job. This file will serve as the schema for your final output table.

### Copy the Spark job and DAG to Google Cloud Storage

On the Composer page, click the name of your Composer environment:

![12a1087dbf356979.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/d7b430ddf4668ebfe01176ea06f5843db4dc364e76bef5ca69f85bd9977e3ff6.png)

On the Environments details page you'll find the web server URL, the DAG folder path, and the bucket name.

Run the following, updating with the name of your DAGs folder:

```
gsutil cp ~/professional-services/examples/cloud-composer-examples/composer_http_post_example/ephemeral_dataproc_spark_dag.py gs://<dag-folder>/dags

```

```
gsutil cp ~/professional-services/examples/cloud-composer-examples/composer_http_post_example/spark_avg_speed.py gs://$PROJECT/spark-jobs/

```

Preparing IAP Authentication
----------------------------

In the event of Spark jobs that need to be run at an irregular cadence based on some on-prem business logic, you will set up the DAG to be triggered only via a POST to an endpoint. This endpoint will sit behind an Identity Aware Proxy and will need to be called using a service account.

### Create a Service Account for POST Trigger

You need to create a service account to facilitate triggering your DAG by a POST to an endpoint.

![79bfd1a0d18e630e.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/330274b0324a6133072e2da62a75384fd7af9cf7da2a464b259593c2f3e73c3c.png)

Create a service account to facilitate triggering your DAG by a POST to an endpoint. To do this the Service Account will need [Service Account Token Creator](https://cloud.google.com/iam/docs/service-accounts#the_service_account_token_creator_role), [Service Account Actor](https://cloud.google.com/iam/docs/service-accounts#the_service_account_token_actor_role) permissions and [Cloud Composer User](https://cloud.google.com/iam/docs/understanding-roles#cloud_composer_roles) permissions.

This Service Account can be created with the following series of commands in the Cloud Shell:

```
gcloud iam service-accounts create dag-trigger
```

```
gcloud projects add-iam-policy-binding $PROJECT\
--member serviceAccount:dag-trigger@$PROJECT.iam.gserviceaccount.com\
--role roles/iam.serviceAccountTokenCreator
```

```
gcloud projects add-iam-policy-binding $PROJECT\
--member serviceAccount:dag-trigger@$PROJECT.iam.gserviceaccount.com\
--role roles/iam.serviceAccountActor
```

```
gcloud projects add-iam-policy-binding $PROJECT\
--member serviceAccount:dag-trigger@$PROJECT.iam.gserviceaccount.com\
--role roles/composer.user
```

You will need a Service Account json key file in order to use the Service Account you just created.

This json key file can be downloaded and incorporated into your environment with the following commands:

```
gcloud iam service-accounts keys create ~/$PROJECT-dag-trigger-key.json\
--iam-account=dag-trigger@$PROJECT.iam.gserviceaccount.com
```

```
export GOOGLE_APPLICATION_CREDENTIALS=~/$PROJECT-dag-trigger-key.json
```

Triggering the DAG
------------------

### Python setup

First, do a bit of setup for the required Python libraries. These commands are setting up the Python environment:

```
cd ~/professional-services/examples/cloud-composer-examples/composer_http_post_example/
pip install --upgrade virtualenv
pip install -U pip
virtualenv composer-env
source composer-env/bin/activate
```

By default one of Airflow's dependencies installs a [General Public License](https://www.gnu.org/licenses/gpl-3.0.en.html) (GPL) dependency (unidecode). To avoid this dependency set `SLUGIFY_USES_TEXT_UNIDECODE=yes` in your environment when you install or upgrade Airflow:

```
export SLUGIFY_USES_TEXT_UNIDECODE=yes
```

Install requirements for using the `dag_trigger.py` script:

```
pip install -r iap_requirements.txt
```

### Getting Airflow web server URL

From the Environment details page, copy the URL of the web server and save it to your computer. You'll be using it in a command shortly.

![1c54cb71905da97c.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/f087e42bad6eb7ec0c02cbc952a5e41d97757883307ea407c153ad9f7454d6ba.png)

### Getting Airflow Client ID

Now paste the Airflow web URL into a new incognito window and press Enter.

DO NOT LOG IN. Just look at the URL.

The first landing page for IAP Auth has the Client ID in the URL. Copy the ID after "client_id" stopping right before "googleusercontent.com" and save it. You'll be using it in a command shortly.

### Construct the Endpoint URL and trigger the DAG

The endpoint triggering the DAG has the following structure: `https://<airflow web server url>/api/experimental/dags/<dag-id>/dag_runs`. In this case your dag-id is `average-speed`.

Construct your endpoint by running the following in Cloud Shell, replacing and updating the with the information you saved:

```
python dag_trigger.py\
--url=https://<airflow-url>/api/experimental/dags/average-speed/dag_runs\
--iapClientId=<client id>.apps.googleusercontent.com\
--raw_path=gs://$PROJECT/cloud-composer-lab/raw-$EXPORT_TS/

```

Reviewing the DAG
-----------------

While the DAG is running you can review the code for this DAG in the Cloud Shell's Code Editor or a text editor of your choosing (ie. vim, emacs, nano).

### Open Code Editor

Now view the source code for this lab. Click on the Code Editor icon in Cloud Shell.

![ba731110a97f468f.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/3b5a520a96927baa799f138c9a3420dd5b1fd18c076aa5b66d3e7a84ce88ca6d.png)

The file structure in `professional-services/examples/cloud-composer-examples` is as follows:

-   README.md: a markdown file detailing how to use this lab

-   requirements.txt: a text file defining the python dependencies for this lab

-   tests: a directory containing the necessary unit tests for the spark job

    -   *Test_spark_avg_speed.py*: a python unit test file
-   composer_http_post_example: a directory containing the majority of the source code

    -   *spark_avg_speed.py*: python file defining the PySpark job for enhancing the yellow cab trips data with an average speed field
    -   *dag_trigger.py*: a python file for triggering the dag in the cloud composer environment from any computer.
    -   *ephemeral_dataproc_spark_dag.py*: the configuration as code python file defining our Airflow DAG
    -   composer-env: a directory containing the virtual environment that you created earlier in this lab

#### Review DAG and Spark Python Code

Navigate to the `spark_avg_speed.py` (Spark job) and `ephemeral_dataproc_spark_dag.py` (Airflow DAG) files and open them. Read through the comments which explain what the code is doing.

This "configuration as code" script will orchestrate:

-   spinning up the Dataproc cluster
-   submitting the Pyspark job
-   tearing down the Dataproc cluster
-   writing the enhanced data to BigQuery

You can close the Cloud Shell Editor.

Observe Airflow do it's thing
-----------------------------

On the Environments page, click the Airflow webserver link to open Airflow. Log in with your lab credentials, or you can finish logging into the Airflow tab you opened earlier.

On the DAGs page click on Graph View icon:

![16ca138a63ca7162.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/d094f32549bbb342919ea5357e199b5d2850cce0d53fac46331ec8b8c7e77bbe.png)

Here you can view the structure of your DAG:

![7c7028644dbdce15.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/242615182852d264080cfbebf57259ac8ab4076c4b03541f8da2d89219e17b6d.png)

Occasionally refresh the graph with the button to the right to see the most up to date information. Don't wait for the process to complete - keep going with the lab!

Now navigate to Browse > Task Instances:

![84a9648163630fad.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/54eb169517575365ccf8e4d693e153cc3650296eac88df5eb3be60c11f7376da.png)

Here you'll see a history of the Task Instances that have run in this environment. By clicking in a task instance you can dig into the associated metadata, rendered templates, XComs and logs recorded during the execution of this Task Instance. This is useful when debugging a DAG. Next you'll see what's going on in the GCP Console.

Monitor your GCP resources from the Console
-------------------------------------------

Before the `run_dataproc-pyspark` task completes, go the GCP Console. It is more than mildly entertaining to sit back and watch these resources get created and destroyed while the DAG is executing. From the Console Navigation menu go to:

-   Dataproc > Clusters and watch the dataproc cluster get created. (90s)
-   Dataproc > Jobs to see the DAG submit the PySpark job to add an `average_speed` field to the data and convert to CSV. By clicking on your job your can monitor the logs. (5 mins)
-   Cloud Storage: Click on your project name then choose the cloud-composerfolder to see the enhanced data accumulated in a timestamped GCS folder.
-   Finally, observe that the DAG cleans up the enhanced files in GCS. (10 seconds)
-   BigQuery and see that the enhanced data was written to a BigQuery table.
-   Navigate back to Dataproc > Clusters and note that the cluster has been torn down. (2 min)

Go back to Airflow, refresh the browser, and see how the graph displays your job.