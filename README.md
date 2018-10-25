# Google Data Engineering

This repository contains a collection of resources that will help a developer prepare
for the Google Cloud Platform - Data Engineering Certification.

## Conceptual Knowledge Articles
- [Google Cloud Platform (GCP) services you can use to manage data throughout its entire lifecycle, from initial acquisition to final visualization.](articles/data-lifecycle-cloud-platform.md)


## Google Developers Codelabs
Provide a guided, tutorial, hands-on coding experience. Most codelabs will step you through the process of building a small application, or adding a new feature to an existing application. They cover a wide range of topics such as Android Wear, Google Compute Engine, Project Tango, and Google APIs on iOS.

[https://codelabs.developers.google.com/](https://codelabs.developers.google.com/)

### Labs and demos for courses for GCP Training
https://github.com/GoogleCloudPlatform/training-data-analyst

### In this lab you spin up a virtual machine, configure its security, and access it remotely.
https://codelabs.developers.google.com/codelabs/cpb100-compute-engine/

### In this lab you carry out the steps of an ingest-transform-and-publish data pipeline manually.
https://codelabs.developers.google.com/codelabs/cpb100-cloud-storage/

### Geographic data in Datalab
This notebook demonstrates how to use Datalab to display the earthquakes that have happened over the past 7 days. The data come from USGS, and we will use the Python module basemap to do the plotting.
https://github.com/GoogleCloudPlatform/datalab-samples/blob/master/basemap/earthquakes.ipynb

### Setup rentals data in Cloud SQL
https://codelabs.developers.google.com/codelabs/cpb100-cloud-sql/
* Create Cloud SQL instance
* Create database tables by importing .sql files from Cloud Storage
* Populate the tables by importing .csv files from Cloud Storage
* Allow access to Cloud SQL
* Explore the rentals data using SQL statements from CloudShell

### Setup rentals data in Cloud SQL
https://codelabs.developers.google.com/codelabs/cpb100-dataproc/
* Launch DataprocRun Spark
* ML jobs using Dataproc

### Create ML dataset with BigQuery
https://codelabs.developers.google.com/codelabs/cpb100-datalab
```
gcloud compute zones list
datalab connect mydatalabvm
datalab create mydatalabvm --zone us-central1-a
```
https://codelabs.developers.google.com/codelabs/cpb100-bigquery-dataset/
* Use BigQuery and Datalab to explore and visualize data
* Build a Pandas dataframe that will be used as the training dataset for machine learning using TensorFlow

https://codelabs.developers.google.com/codelabs/cpb100-tensorflow/
* Use TensorFlow to create a neural network model to forecast taxicab demand in NYC

Machine Learning APIs
* Learn how to invoke ML APIs from Python and use their results.
https://codelabs.developers.google.com/codelabs/cpb100-translate-api/

- Cloud Datastore: https://cloud.google.com/datastore/
- Cloud Bigtable: https://cloud.google.com/bigtable/
- Google BigQuery: https://cloud.google.com/bigquery/
- Cloud Datalab: https://cloud.google.com/datalab/
- TensorFlow: https://www.tensorflow.org/
- Cloud Machine Learning: https://cloud.google.com/ml/
- Vision API: https://cloud.google.com/vision/
- Translate API: https://cloud.google.com/translate/
- Speech API: https://cloud.google.com/speech/
