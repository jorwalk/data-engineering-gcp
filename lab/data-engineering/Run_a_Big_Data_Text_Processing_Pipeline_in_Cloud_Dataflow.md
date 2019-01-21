Run a Big Data Text Processing Pipeline in Cloud Dataflow
=========================================================

Overview
--------

Dataflow is a unified programming model and a managed service for developing and executing a wide range of data processing patterns including ETL, batch computation, and continuous computation. Because Dataflow is a managed service, it can allocate resources on demand to minimize latency while maintaining high utilization efficiency.

The Dataflow model combines batch and stream processing so developers don't have to make tradeoffs between correctness, cost, and processing time. In this lab, you'll learn how to run a Dataflow pipeline that counts the occurrences of unique words in a text file.

#### What you'll learn

-   How to create a Maven project with the Cloud Dataflow SDK

-   Run an example pipeline using the Google Cloud Platform Console

-   How to delete the associated Cloud Storage bucket and its contents

### Enable the APIs

A number of APIs must be enabled for this lab to work. This section will show you how to check and if necessary, enable an API.

To check an API status, click Navigation menu > APIs & Services > Dashboard

![api_nav.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/f9d0c73a14be9e0c6d06dc5c831760e5da02640247c5d100a11a83671e90e5f1.png)

Start by checking the status of Compute Engine API. In Dashboard, look down the API list and see if Compute Engine API is listed.

![api_dash.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/97986f3c8e40d59c05e3b54f27ca4d6eda007b8f2ca05374ef10b613299d127a.png)

In the example above, Compute Engine API is listed, and since you have the option to disable it, it's enabled.

In most cases, all APIs required for the lab are enabled; if any are not, you must enable them.

One way to enable this API is, still in the Dashboard, click ENABLE APIS AND SERVICES.

![enable_api.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/e14b377ba30d49c5a1ce465c19240082a4b94a11952570d621d2a9a6422a7459.png)

Still using Compute Engine API as an example, type "compute engine" in the search dialog, then click on Compute Engine API.

![api_filtered.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/620e18629136aaf1eecf00b3e5c1ec08c14ef5b91d3ee374f0edafe9544793c9.png)

Click on Google Compute Engine API in search results. The API library will display details. The API is enabled. If you see a Manage button and an API enabled white check in a green circle.

If the API is not enabled, click the Enable button.

In the example below, the API is enabled.

![api_status.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/53de7e8b165e41d2bdf1ae78ab6a0640d7cea4e9d459574d01976e50165eba55.png)

Once it has enabled click the arrow to go back to the API Library.

Search for the following APIs and enable them as needed:

-   Dataflow API

-   Stackdriver Logging API

-   Google Cloud Storage

-   Google Cloud Storage JSON API

-   BigQuery API

-   Cloud Pub/Sub API

-   Cloud Datastore API

Create a new Cloud Storage bucket
---------------------------------

To create a new Cloud Storage bucket, In the [Google Cloud Platform Console](https://console.cloud.google.com/), click Navigation menu > Storage.

![create_bucket.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/6bf395b6f270d152076a23d350ca0ff2584021936082ba44eaa0ad563ecdfc0d.png)

Click Create bucket to create a Cloud Storage Bucket.

Enter a unique name, and click Create.

![bucket_conf.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/796d0c77d4a256150b53e5cd297b0c068fadba92435a0244b37f772c5e1e0280.png)

After successfully creating a bucket, you are taken to your new, empty, bucket in Browser:

![storage_browser.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/fcda94b021b78a4a7015bd572b781a863be7027bc6d4520029034392d92525d1.png)

Create a Maven project
----------------------

In this section, you'll create a Maven project that contains the Cloud Dataflow SDK for Java.

Before you begin, in the Cloud Shell command line, enter the ls command to see what's what.

```
ls
```

You'll see one directory, `README-cloudshell.txt`.

Create the Maven project, enter the `mvn archetype:generate` command:

```
mvn archetype:generate\
      -DarchetypeGroupId=org.apache.beam\
      -DarchetypeArtifactId=beam-sdks-java-maven-archetypes-examples\
      -DarchetypeVersion=2.8.0\
      -DgroupId=org.example\
      -DartifactId=first-dataflow\
      -Dversion="0.1"\
      -Dpackage=org.apache.beam.examples\
      -DinteractiveMode=false

```

Now you should see a new directory called `first-dataflow` under your current directory. `first-dataflow` contains a Maven project that includes the Cloud Dataflow SDK for Java and example pipelines. You'll see the following output when the build is finished:

![build_status.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/4a2639707cef579a937b8c7c36345710ab49e2c67c5ed48d36055925c2d73d8e.png)

Check out your directory again:

```
ls

```

You'll see a new directory, `first-dataflow`.

Run a text processing pipeline on Cloud Dataflow
------------------------------------------------

Save your project ID and Cloud Storage bucket names as environment variables. Be sure to replace `<your_project_id>` with your GCP Project ID, found in the Connection Details section of the lab.

```
export PROJECT_ID=<your_project_id>

```

Note: You can retrieve the Project ID with the `gcloud` command and with using ENV variable:

-   `gcloud` command: `gcloud config get-value project`
-   ENV Variable: `echo $DEVSHELL_PROJECT_ID`

If you use mentioned ways then your final command will be like:

-   `export PROJECT_ID=$DEVSHELL_PROJECT_ID`
-   `export PROJECT_ID=$(gcloud config get-value project)`

Now do the same for the Cloud Storage bucket. Remember to replace `<your_bucket_name>` with the bucket name you created earlier.

```
export BUCKET_NAME=<your_bucket_name>

```

Change to the `first-dataflow/` directory.

```
cd first-dataflow

```

We're going to run a pipeline called WordCount, which reads text, tokenizes the text lines into individual words, and performs a frequency count on each of those words. While the pipeline is running, we'll take a look at what's happening in each step.

Start the pipeline by running `mvn -Pdataflow-runner compile exec:java`. For the `--project, --stagingLocation,` and `--output`arguments, the command below references the environment variables you just set up.

```
 mvn -Pdataflow-runner compile exec:java\
      -Dexec.mainClass=org.apache.beam.examples.WordCount\
      -Dexec.args="--project=${PROJECT_ID}\
      --stagingLocation=gs://${BUCKET_NAME}/staging/\
      --output=gs://${BUCKET_NAME}/output\
      --runner=DataflowRunner"

```

While the job is running, let's find the job in the job list in the Dataflow page in the console.

Click the Navigation menu > Dataflow to open the Dataflow page.

You should see your wordcount job with a status of Running:

![dataflow_console.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/901dd84ed9b57910391eb32773e0e0558d2d020749e0f535c56fee4d8d35f277.png)

Let's look at the pipeline parameters. Start by clicking on the name of your job:

![job_status.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/3b98ebd9683aca00b467b694f6f7b76d8561fa85c77ded8ad276b0994a0ad770.png)

When you select a job, you'll see the execution graph. A pipeline's execution graph represents each transform in the pipeline as a box that contains the transform name and some status information. You can click on the carat in the top right corner of each step to see more details:

![job_view.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/dbad462bab1ac1fad44edec0948e64c9f8f2a59cf64f90235c5d3627f3ad9896.png)

Let's see how the pipeline transforms the data at each step:

-   **Read:** The pipeline reads from an input source. In this case, it's a text file from Cloud Storage with the entire text of the Shakespeare play *King Lear*. Our pipeline reads the file line by line and outputs each a `PCollection`, where each line in the text file is an element in the collection.

-   **CountWords:** The `CountWords` step has two parts. First, it uses a parallel do function (ParDo) named `ExtractWords` to tokenize each line into individual words. The output of `ExtractWords` is a new PCollection where each element is a word. The next step, `count`, utilizes a transform provided by the Dataflow SDK which returns key, value pairs where the key is a unique word and the value is the number of times it occurs. Here's the method implementing `CountWords`. You can check out the full `WordCount.java` file on [GitHub](https://github.com/GoogleCloudPlatform/DataflowSDK-examples/blob/1af22edf182b86a6d1331f7589966841d6e09f82/java/examples-java8/src/main/java/com/google/cloud/dataflow/examples/WordCount.java):

```
  /**
   * A PTransform that converts a PCollection containing lines of text
   * into a PCollection of formatted word counts.
   */
  public static class CountWords extends PTransform<PCollection<String>,
      PCollection<KV<String, Long>>> {
    @Override
    public PCollection<KV<String, Long>> apply(PCollection<String> lines) {

      // Convert lines of text into individual words.
      PCollection<String> words = lines.apply(
          ParDo.of(new ExtractWordsFn()));

      // Count the number of times each word occurs.
      PCollection<KV<String, Long>> wordCounts =
          words.apply(Count.<String>perElement());

      return wordCounts;
    }
  }

```

-   **FormatAsText:** This is a function that formats each key, value pair into a printable string. Here's the `FormatAsText` transform to implement this:

```
  /** A SimpleFunction that converts a Word and Count into a printable string. */
  public static class FormatAsTextFn extends SimpleFunction<KV<String, Long>, String> {
    @Override
    public String apply(KV<String, Long> input) {
      return input.getKey() + ": " + input.getValue();
    }
  }
```

-   **WriteCounts:** The printable strings are written into multiple sharded text files.

We'll take a look at the resulting output from the pipeline in a few minutes.

Now take a look at the Job Summary section to the right of the execution graph, which includes Pipeline options that were included in the `mvn -Pdataflow-runner compile exec:java` command:

![job_detailed_status.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/0ae233aa694c5a30a9887431dc7415cb8589882f8f191274ed22d66072dccc18.png)

You can also see Custom counters for the pipeline, which in this case shows how many empty lines have been encountered so far during execution. Add new counters to your pipeline to track application-specific metrics.

Click the Logs icon to see specific error messages.

![wordcount_log.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/dae0fd308d22bb92afdaab619aa9bfd0a35e8ec3f931ee05ea256825dfed5281.png)

Filter the messages that appear in the Job Log tab by using the Minimum Severity drop-down menu.

![job_logs.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/267a5dd70633d73878d29234adfe937bb12e818156dcc356d07a5a815cd401df.png)

Click the Stackdriver link in the JOB LOGS tab to see the worker logs for the Compute Engine instances that run your pipeline. Worker Logs consist of log lines generated by your code and the Dataflow generated code running it.

If you are trying to debug a failure in the pipeline, oftentimes there will be additional logging in the Worker Logs to help solve the problem. These logs are aggregated across all workers and can be filtered and searched.

![detailed_logs.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/e411b4e7ab4197d2507d3040c4b3ba12735f935ae65a8ddd1f9c0e98b35696e5.png)

Now we'll check that your job succeeded.

Check that your job succeeded
-----------------------------

In the console, click Navigation menu > Dataflow to open the Dataflowpage and monitor you job.

You'll see your wordcount job with a status of Running at first, and then Succeeded:

![job_success.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/df1431fd17ef55ae0bac5df43cebd5af3421e04ff9faa40267939c0ce5eee001.png)

The job will take approximately 3-4 minutes to run.

Remember when you ran the pipeline and specified an output bucket? Let's take a look at the result (because don't you want to see how many times each word in *King Lear* occurred?!).

In the Google Cloud Platform Console, click Navigation menu > Storage, then Browser. In your bucket you should see the output text files and staging folder that your job created:

![storage_view.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/1e39c67de32c740213e61c2fa8d309ffb0a9898f72f81382d3a3712776ae29f7.png)

Open a text file to see some results.