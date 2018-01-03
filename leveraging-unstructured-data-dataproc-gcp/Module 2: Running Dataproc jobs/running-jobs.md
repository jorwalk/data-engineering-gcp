## Lab 2a: Running Pig and Spark programs

In this lab, you will run Pig and Spark programs on a Dataproc cluster.

## What you learn
In this lab, you:

* SSH into the cluster to run Pig and Spark job
* Create a Cloud Storage bucket to store job input files
* Work with HDFS

Begin the lab

https://codelabs.developers.google.com/codelabs/cpb102-running-pig-spark/

Google Cloud Dataproc supports running jobs written in Apache Pig, Apache Hive, Apache Spark, and other tools commonly used in the Apache Hadoop ecosystem.

For development purposes, you can SSH into the cluster master and execute jobs using the PySpark Read-Evaluate-Process-Loop (REPL) interpreter.

Let's take a look at how this works.

./replace_and_upload.sh gcp365-191023
gsutil -m cp gs://gcp365-191023/unstructured/pet-details.* .
