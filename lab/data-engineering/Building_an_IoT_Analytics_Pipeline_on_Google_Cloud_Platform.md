Building an IoT Analytics Pipeline on Google Cloud Platform
===========================================================

Overview
--------

The term Internet of Things (IoT) refers to the interconnection of physical devices with the global Internet. These devices are equipped with sensors and networking hardware, and each is globally identifiable. Taken together, these capabilities afford rich data about items in the physical world.

Cloud IoT Core is a fully managed service that allows you to easily and securely connect, manage, and ingest data from millions of globally dispersed devices. The service connects IoT devices that use the standard `Message Queue Telemetry Transport (MQTT)`protocol to other Google Cloud Platform data services.

Cloud IoT Core has two main components:

-   A device manager for registering devices with the service, so you can then monitor and configure them.

-   A protocol bridge that supports MQTT, which devices can use to connect to the Google Cloud Platform.

### What You'll Learn

In this lab, you will dive into GCP's IoT services and complete the following tasks:

-   Connect and manage MQTT-based devices using Cloud IoT Core (we will use simulated devices.)
-   Ingest a stream of information from Cloud IoT Core using Cloud Pub/Sub.
-   Process the IoT data using Cloud Dataflow.
-   Analyze the IoT data using BigQuery.

### Prerequisites

This is an advanced level lab and having a baseline level of knowledge on the following services will help you better understand the steps and commands that you'll be writing:

-   BigQuery
-   Cloud Pub/Sub
-   Dataflow
-   IoT

If you want to get up to speed in any of the above services, check out the following Qwiklabs:

-   [BigQuery: Qwik Start - Web User Interface](https://qwiklabs.com/catalog_lab/685)
-   [Weather Data in BigQuery](https://qwiklabs.com/catalog_lab/476)
-   [Dataflow: Qwik Start - Templates](https://qwiklabs.com/catalog_lab/934)
-   [Internet of Things: Qwik Start](https://qwiklabs.com/catalog_lab/708)

Ready to dive into IoT and related GCP services? If so, let's get started!

Create a Cloud Pub/Sub Topic
----------------------------

`Cloud Pub/Sub` is an asynchronous global messaging service. By decoupling senders and receivers, it allows for secure and highly available communication between independently written applications. Cloud Pub/Sub delivers low-latency, durable messaging.

In Cloud Pub/Sub, publisher applications and subscriber applications connect with one another through the use of a shared string called a topic. A publisher application creates and sends messages to a topic. Subscriber applications create a subscription to a topic to receive messages from it.

In an IoT solution built with Cloud IoT Core, device telemetry data is forwarded to a Cloud Pub/Sub topic.

To define a new Cloud Pub/Sub topic:

1.  In the GCP Console, go to Navigation menu> Pub/Sub> Topics.
2.  Click Create Topic. The Create a topic dialog shows you a partial URL path, consisting of `projects/`followed by your project name and a trailing slash, then `topics/` . Confirm that the project name is the one you noted above.

> Note: If you see `qwiklabs-resources` as your project name, cancel the dialog and return to the Cloud Platform console. Use the menu to the right of the Google Cloud Platform logo to select the correct project. Then return to this step.

1.  Give this string as your topic name:

```
iotlab
```

1.  Then click Create.

1.  In the resulting list of topics, you will see a new topic whose partial URL ends in `iotlab`. Click the three-dot icon at the right edge of its row to open its context menu. Choose Permissions.

    ![ff815d634bdaa981.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/7c82bed09d14b3982d55ba54ca9599ea49dd865181e037c8838d09839e095dd1.png)

2.  In the Permissions dialogue, add this member:

```
cloud-iot@system.gserviceaccount.com

```

1.  Grant the new member the Pub/Sub > Pub/Sub Publisher role. Click Add.

Create a BigQuery dataset
-------------------------

`BigQuery` is a serverless data warehouse. Tables in BigQuery are organized into datasets. In this lab, messages published into Pub/Sub will be aggregated and stored in BigQuery.

To create a new BigQuery dataset:

1.  In the GCP Console, go to Navigation menu> BigQuery.

2.  In the left-hand side of the browser window, make sure your project isn't set to "Qwiklabs Resources". If it is, click on the blue arrow next to the name of your project and select Switch to project > your-qwiklabs-project.

3.  Click on the blue arrow next to the name of your project and select Create new dataset:

    ![2a59ba66b972a280.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/a617910212b9a12781780c5d2831b58578dc0bc00da2fe6e7263c949edac8d82.png)

4.  Give the new dataset the name iotlab and click OK.

1.  When the dataset is created, to the right of iotlab, click the Add table icon. The Create Table dialog opens.

    ![855496676eb58e93.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/e1999411fbdcf55233d2e955a4dacd1cb2299ff3e8f6e787c2d9bcef75ff357e.png)

2.  In the Source Data section, click Create empty table.

3.  In the Destination Table section's Table name field, enter sensordata.

4.  In the Schema section, enter timestamp for the field name. Set the field's Type to TIMESTAMP.

5.  Click the Add Field button.

6.  In the newly created line, enter device for the field name. Set the field's Type to STRING.

7.  Click the Add Field button.

8.  In the newly created line, enter temperature for the field name. Set the field's Type to FLOAT.

9.  Leave the other defaults unmodified. Click Create Table.

    ![4159ab1792faee34.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/75103e8c7fa7c59294626de557246b669102faeda1e2460d7217ca794d07f3c5.png)

Create a Cloud Storage Bucket
-----------------------------

Cloud Storage allows world-wide storage and retrieval of any amount of data at any time. You can use Cloud Storage for a range of scenarios including serving website content, storing data for archival and disaster recovery, or distributing large data objects to users via direct download. In this lab we will use Cloud Storage to provide working space for our Cloud Dataflow pipeline.

1.  In the GCP Console, go to Navigation menu > Storage.

2.  Click CREATE BUCKET.

3.  For Name, paste in your GCP project ID.

4.  For Default storage class, click Multi-regional if it is not already selected.

5.  For Location, choose the selection closest to you.

6.  Click Create.

Set up a Cloud Dataflow Pipeline
--------------------------------

Cloud Dataflow is a serverless way to carry out data analysis. In this lab, you will set up a streaming data pipeline to read sensor data from Pub/Sub, compute the maximum temperature within a time window, and write this out to BigQuery.

1.  In the GCP Console, go to Navigation menu > Dataflow.

2.  In the top menu bar, click CREATE JOB FROM TEMPLATE.

3.  In the job-creation dialog, for Job name, enter iotlab.

4.  For Cloud Dataflow template, choose Cloud PubSub to BigQuery. When you choose this template, the form updates to review new fields below.

5.  For Cloud Dataflow Regional Endpoint, choose the region as us-central1.

6.  For Cloud Pub/Sub input topic, enter `projects/`followed by your GCP project ID then add `/topics/iotlab` . The resulting string will look like this: `projects/qwiklabs-gcp-d2e509fed105b3ed/topics/iotlab`

7.  For BigQuery output table, enter your GCP project ID followed by `:iotlab.sensordata`. The resulting string will look like this: `qwiklabs-gcp-d2e509fed105b3ed:iotlab.sensordata`

8.  For Temporary location, enter `gs://` followed by your GCP project ID and then `/tmp/`. The resulting string will look like this: `gs://qwiklabs-gcp-d2e509fed105b3ed/tmp/`

9.  Click Optional parameters.

10. For Max workers, enter 2.

11. For Machine type, enter n1-standard-1.

12. Click Run Job. A new streaming job is started. You can now see a visual representation of the data pipeline.

Prepare Your Compute Engine VM
------------------------------

In your project, a pre-provisioned VM instance named `iot-device-simulator` will let you run instances of a Python script that emulate an MQTT-connected IoT device. Before you emulate the devices, you will also use this VM instance to populate your Cloud IoT Core device registry.

To connect to the `iot-device-simulator` VM instance:

1.  In the GCP Console, go to Navigation menu > Compute Engine> VM Instances. You'll see your VM instance listed as `iot-device-simulator`.

2.  To the right, click the SSH drop-down arrow and select Open in browser window.

Note: You might need to click Hide Info Panel to reveal the SSH drop-down arrow.

1.  In your SSH session on the iot-device-simulator VM instance, enter this command to remove the default Google Cloud Platform SDK installation. (In subsequent steps, you will install the latest version, including the beta component.)

```
sudo apt-get remove google-cloud-sdk -y
```

1.  Now install the latest version of the Google Cloud Platform SDK and accept all defaults:

```
curl https://sdk.cloud.google.com | bash
```

1.  End your ssh session on the iot-device-simulator VM instance:

```
exit
```

1.  Start another SSH session on the `iot-device-simulator` VM instance.

2.  Initialize the gcloud SDK.

```
gcloud init
```

If you get the error message "Command not found," you might have forgotten to exit your previous SSH session and start a new one.

If you are asked whether to authenticate with an @developer.gserviceaccount.com account or to log in with a new account, choose to log in with a new account.

If you are asked "Are you sure you want to authenticate with your personal account? Do you want to continue (Y/n)?" enter Y.

1.  Click on the URL shown to open a new browser window that displays a verification code.

2.  Use your browser to copy the verification code.

3.  Paste the verification code in response to the "Enter verification code:" prompt and press Enter.

4.  In response to "Pick cloud project to use," pick the GCP project that Qwiklabs created for you.

5.  Enter this command to make sure that the components of the SDK are up to date:

```
gcloud components update
```

1.  Enter this command to install the beta components:

```
gcloud components install beta
```

1.  Enter this command to update the system's information about Debian Linux package repositories:

```
sudo apt-get update
```

1.  Enter this command to make sure that various required software packages are installed:

```
sudo apt-get install python-pip openssl git -y
```

1.  Use pip to add needed Python components:

```
sudo pip install pyjwt paho-mqtt cryptography
```

1.  Enter this command to add data to analyze during this lab:

```
git clone http://github.com/GoogleCloudPlatform/training-data-analyst
```

Create a Registry for IoT Devices
---------------------------------

To register devices, you must create a registry for the devices. The registry is a point of control for devices.

To create the registry:

1.  In your SSH session on the iot-device-simulator VM instance, run the following, adding your project ID as the value for PROJECT_ID:

```
export PROJECT_ID=
```

Your completed command will look like this: `export PROJECT_ID=qwiklabs-gcp-d2e509fed105b3ed`

1.  You must choose a region for your IoT registry. At the present time, these regions are supported:

-   `us-central1`
-   `europe-west1`
-   `asia-east1`

Choose the region that is closest to you. To set an environment variable containing your preferred region, enter the command:

```
export MY_REGION=
```

followed by the region name. Your completed command will look like this: `export MY_REGION=us-central1`

1.  Enter this command to create the device registry:

```
gcloud beta iot registries create iotlab-registry\
   --project=$PROJECT_ID\
   --region=$MY_REGION\
   --event-notification-config=topic=projects/$PROJECT_ID/topics/iotlab
```

Create a Cryptographic Keypair
------------------------------

To allow IoT devices to connect securely to Cloud IoT Core, you must create a cryptographic keypair.

In your SSH session on the iot-device-simulator VM instance, enter these commands to create the keypair in the appropriate directory:

```
cd $HOME/training-data-analyst/quests/iotlab/
openssl req -x509 -newkey rsa:2048 -keyout rsa_private.pem\
    -nodes -out rsa_cert.pem -subj "/CN=unused"
```

This `openssl` command creates an RSA cryptographic keypair and writes it to a file called `rsa_private.pem`.

Add Simulated Devices to the Registry
-------------------------------------

For a device to be able to connect to Cloud IoT Core, it must first be added to the registry.

1.  In your SSH session on the iot-device-simulator VM instance, enter this command to create a device called `temp-sensor-buenos-aires`:

```
gcloud beta iot devices create temp-sensor-buenos-aires\
  --project=$PROJECT_ID\
  --region=$MY_REGION\
  --registry=iotlab-registry\
  --public-key path=rsa_cert.pem,type=rs256
```

1.  Enter this command to create a device called `temp-sensor-istanbul`:

```
gcloud beta iot devices create temp-sensor-istanbul\
  --project=$PROJECT_ID\
  --region=$MY_REGION\
  --registry=iotlab-registry\
  --public-key path=rsa_cert.pem,type=rs256
```

Run Simulated Devices
---------------------

1.  In your SSH session on the iot-device-simulator VM instance, enter these commands to download the CA root certificates from pki.google.com to the appropriate directory:

```
cd $HOME/training-data-analyst/quests/iotlab/
wget https://pki.google.com/roots.pem
```

1.  Enter this command to run the first simulated device:

```
python cloudiot_mqtt_example_json.py\
   --project_id=$PROJECT_ID\
   --cloud_region=$MY_REGION\
   --registry_id=iotlab-registry\
   --device_id=temp-sensor-buenos-aires\
   --private_key_file=rsa_private.pem\
   --message_type=event\
   --algorithm=RS256 > buenos-aires-log.txt 2>&1 &
```

It will continue to run in the background.

1.  Enter this command to run the second simulated device:

```
python cloudiot_mqtt_example_json.py\
   --project_id=$PROJECT_ID\
   --cloud_region=$MY_REGION\
   --registry_id=iotlab-registry\
   --device_id=temp-sensor-istanbul\
   --private_key_file=rsa_private.pem\
   --message_type=event\
   --algorithm=RS256
```

Telemetry data will flow from the simulated devices through Cloud IoT Core to your Cloud Pub/Sub topic. In turn, your Dataflow job will read messages from your Pub/Sub topic and write their contents to your BigQuery table.

Analyze the Sensor Data Using BigQuery
--------------------------------------

To analyze the data as it is streaming:

1.  In the GCP Console, on the Navigation menu> BigQuery.

2.  In the left-hand side of the browser window, click on Compose Query.

3.  Enter the following query:

```
#standardsql
SELECT timestamp, device, temperature from iotlab.sensordata
ORDER BY timestamp DESC
LIMIT 100
```

1.  Browse the data using the previous-page and next-page links provided. What is the temperature trend at each of the locations?