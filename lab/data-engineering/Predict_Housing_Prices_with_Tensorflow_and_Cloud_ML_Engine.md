Predict Housing Prices with Tensorflow and Cloud ML Engine
==========================================================


Overview
--------

In this lab you will build an end to end machine learning solution using Tensorflow + Cloud ML Engine and leverage the cloud for distributed training and online prediction.

[tf.estimator](https://www.tensorflow.org/api_docs/python/tf/estimator)---a high-level TensorFlow API that greatly simplifies machine learning programming. Estimators encapsulate the following actions:

-   training
-   evaluation
-   prediction
-   export for serving

This lab is focused on interacting with Datalab and Cloud ML Engine. Non-relevant concepts and code blocks are glossed over and are provided for you to execute in your Datalab notebook.

Datalab setup
-------------

Datalab is set up on a GCE VM. For that we need to specify the project and the zone where the VM is created. Typically Datalab is set up from your client machine (desktop/laptop) with Cloud SDK installed. Here we are going to use Cloud Shell as the client to run the installation commands.

Copy the project id from the left pane using the icon next to the text in Qwiklabs and paste it in place of *`PROJECT_ID`* :

```
gcloud config set core/project PROJECT_ID
```

You can use the specified zone `us-central1-f`. If you don't specify one, the subsequent command will provide a list and prompt you to pick one.

```
gcloud config set compute/zone us-central1-f
```

`gsutil` is a utility to interact with Google Cloud Storage and is packaged with `gcloud`. Use it to create a storage bucket for your code packages to deploy to Cloud ML engine, replacing [bucket_name] with a [universally unique](https://cloud.google.com/storage/docs/naming) name you create:

```
gsutil mb -c multi_regional -l us gs://[bucket_name]
```

Now create a Datalab VM instance by running the following:

```
datalab create my-datalab --machine-type n1-standard-4
```

When asked to continue, type y.

When you are prompted for a password for the SSH key, press Enter twice to leave it blank.

This process will take a few minutes to complete.

You're ready to proceed when you see following text:

![108a49671fd84563.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/c498abe42284379d88c856974ae8988fff3f7974c9c74b4c948f70d7a8b79bbe.png)

In your browser open the Cloud Datalab notebook by selecting Cloud Shell Web preview→Change port:

![9c8bbf6d12e3a0c4.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/481aba73bfe1f58ee109a0b9b20e2270893f2ecb13ad3b1e318d743dd2fab69a.png)

Then type in "8081" then click Change and Preview:

![9f5f95b5b31c241f.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/5b58e5dcd6542530224db51f2735508cde041e8d981bd510c4e245bff22c6f78.png)

You may need to wait a minute and refresh, as the Datalab process is still booting.

Download lab notebook
---------------------

Next you'll open a new notebook and download some examples into it.

Open a new notebook by clicking the "+ Notebook" button (top left of screen)

Add this code into the notebook cell, then click the Run button or press `Shift + Enter` to execute.

```
!git clone https://github.com/vijaykyr/tensorflow_teaching_examples.git housing_prices

```

You should see the following output.

![ea4ea3742231c6fc.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/725fc9490233b39fcf557edfef3bdf76b097370f33583170f341b0396f84a268.png)

Open and execute the housing prices notebook
--------------------------------------------

From within the Google Cloud Datalab console, switch back to the datalab home page and select datalab→housing_prices→housing_prices→cloud-ml-housing-prices.ipynb to begin the lab. Now you're ready to start!

In the top ribbon, click Clear > Clear all Cells:

![3bbea01e88a74a6f.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/742f0208ca5d95f3fa07cad8056116a064afcbb2ddde9e5502b0ad3a5df3e796.png)

From here, read the instructions in the notebook to complete the lab.

Execute the cells one by one and observe the results. A convenient way to progress through the cells is by clicking in a cell, then click `Shift + Enter`, waiting for each cell to complete before progressing. Code cell completion is indicated by a blue bar on the left of the cell. Any output is printed below.

Example of executed cells:

![cec879ac414fe272.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/ad146b98a7b35ca53420a5e026d705b39f3e18b89b7b8fd3d2e552015b948f94.png)

Read the instructions and the comments in the code blocks carefully. You will be asked to edit some of the code blocks before running them. For example, you will be setting environment variables in the notebook, make sure to add your bucket name and project ID before running the cell.