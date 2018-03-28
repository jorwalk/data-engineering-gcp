Streaming Data Pipelines
In this lab you will use Dataflow to collect traffic events from simulated traffic sensor data made available through Google Cloud PubSub, process them into an actionable average, and store the raw data in BigQuery for later analysis. You will learn how to start a Dataflow pipeline, monitor it, and, lastly, optimize it.

What you need
To complete this lab, you need:

To have set up the Pub/Sub topic and subscription. If you have not done so already, please complete the previous lab.
To have created a Cloud Storage bucket in a previous lab.
All other resources necessary for completing the lab you will create during the lab.

What you learn
In this lab, you will learn how to:

Launch Dataflow and run a Dataflow job
Understand how data elements flow through the transformations of a Dataflow pipeline
Connect Dataflow to Pub/Sub and BigQuery
Observe and understand how Dataflow autoscaling adjusts compute resources to process input data optimally
Learn where to find logging information created by Dataflow
Explore metrics and create alerts and dashboards with Stackdriver Monitoring
Begin the lab
https://codelabs.developers.google.com/codelabs/cpb104-dataflow-streaming-pipeline/

# Lab Review
So let's go ahead and run the code to create streaming data pipelines. So first thing to do is to set up the BitQuery data set. The reason we need it is that our pipeline is going to be streaming

into the sync, and the sync is going to be BitQuery. So let's go to the cloud console, find BitQuery, and this opens up the BitQuery console. And in the BitQuery console, let's go ahead and create a new data set. So my name of my project is cloud training demos. I'm going to go there and create a new data set. I will call that demos, it doesn't expire and I say OK.

And I already have a bucket, so I'll leave that as is. And now we're ready to simulate our traffic sensor data into pub sub. This is exactly what we did in the previous exercise. So let's just go ahead and restart that simulation, so there's stuff being written out. And last time we did the G Cloud alt login, but let's go ahead and do it without and see what happens, courses, streaming, publish. And in there, we will send sensor data with a speed factor of 60, so 60 times real time speeds.

And we basically see that we get a permission denied. And the reason that we're getting a permission denied here is that the user is not authorized. The user, and that's tricky to think about, but the user here is the API. Right, so we're calling a program, send_sensor_data.py. It is that program and that API, right? So we need to basically have this program run as me, as with my user credentials. And the way we're going to do that is to do gcloud auth application- default login. So the application is going to run with my credentials. And in order to do that, so we would take the string, put it into the browser. And the browser window is going to ask me to provide permission, so I'll say yes. As myself, go ahead and allow the Google Auth Library, which is what the Python API is using, wants to basically view and manage your data on GCP. And the reason it needs to do that is because it needs to publish it to a topic on pub sub. So we'll go ahead and say, yes, we'll allow it. And to prove that we have allowed it, we'll go back to the console and provide that. And at this point, now, within this cloud shell window, applications at the launch, I'm launching with my credentials. So we'll go ahead and do that. And then, we will want to send the sensor data, and then now this time, it should be fine. And it'll start sending messages into this topic that we created in the previous lab.

The next thing that we want to do is that we want to go into the process, right? So in order to do that, I'll open up a new tab. And in this tab, I can go into training data analyst, courses, streaming. But now this is the process part of it, and there is a directory called sandiego, which contains all of the Dataflow code. So let's go ahead and look at what that code looks like. So we can actually go into sandiego, and in there is a run_oncloud.sh. And again, I could look at it by using Nano, which is a quick editor, run_oncloud.sh, and we could look at it. But let's instead use the code editor, so I'm clicking on this icon here to launch the code editor. Don't worry if your user interface looks slightly different. But there should be an icon for launching a code editor. So go ahead and do that. And at that point,

We should be able to go into the repository into streaming > process > sandiego. And now look at run_oncloud.sh, and what this is doing is that it's using three parameters. The first parameter is a project, second parameter is a bucket, and the third parameter is the name of the Java class. And then it's using the Maven command to execute this Java class. Again, very similar to the way we did it in the server less data analysis course, when we introduced Dataflow.

So we can be in here and we could also, let's go ahead and look at the code for averagespeeds.java. So that's in the source. So let me just follow the Java packages, and in here, is the averagespeeds.java. So this is the code that we're going to be running, and what this code is going to do is what we talked about in the slides. It's going to set options streaming to be true, creating the pipeline, and then looking at the topic called sandiego. And it's going to be writing out to this data set that we just created, called demos. It's going to be creating a new table called average speeds. In order to do that, it's basically going to be getting messages from pub sub, extracting them into the information of the lane, applying a sliding time window over the averaging interval every so often, that's the averaging frequency. Grouping them by key, and having done that, computing the mean per key.

And then, it's going to take those means, those average speeds, now we're not writing out all of the data, we're just writing out the averages that they're computing over the stream. And we are writing out the lane information. And we're also adding, we are basically changing the speed there to be the average value. So we're writing out for that lane what the average speed is at that sensor. And then we're writing it out to the average speed table. So this is what this code does. So let's go ahead and run it. So to do that, I will do ./run_oncloud.sh. And the project name, my project name happens to be cloud-training-demos.

And my bucket name also happens to be cloud-training-demos conveniently. And the name of the Java class, I'm just going to say AverageSpeeds here because what run_oncloud did, I don't know if you remember what it did, but, Let's just go ahead and close all of these.

And I can show you what run, there it is, run_oncloud.sh. And what it did was, it adds the package name to it. So that's why I'm not providing the full package name, I'm just saying AverageSpeeds. So I'll go ahead and run it. And what this does is that it launches this program onto the cloud.

So it's downloading all of the necessary JAR files, and it's packaging it up. And then it launch it to the cloud. So let's wait for it to launch, it's now still compiling.

And it's averaging it, and then,

All right, so at this point, a Dataflow job has been launched. We can go to the cloud console. Let's go to Dataflow, and when we look in Dataflow, we see that there is an average speed that just got launched. 29 seconds have elapsed, I can click on that, and this is the pipeline. And you can compare this pipeline, get messages, extract data, time window, by sensor, average by sensor, etc, to this code that we want it to do. All right, so we could go in here, I closed it, okay. So let's go ahead and go to sandiego here and do a source > main > java.

And average speeds for Java, and we don't need all these subdirectories, so we can just close all of the tabs for those.

So it helps us go back conveniently.

All right, so there is the AverageSpeeds.java, and as you can see, there is the transformation for get messages, the transformation for extract data, the transformation for the time window, the transformation for bi-sensor. These are just names I gave it. Transformation to compute the average bi-sensor, convert it to a BQ table row, BigQuery table row, and finally write it all out. And that's the pipeline that we are seeing, right? So that's a pipeline that's currently running. And as it's running, it's basically processing messages that are streaming in from the pub sub pipeline. So we can go ahead and look at what it is that we are having in the pub sub, all right? Okay, so let's go to the, Here, and notice that it's going to be, once it starts, here's my job. And we can look in here, and we can see that it's talking about the watermark, right? This is essentially the heuristic that DataFlow is developing over what messages that we published can be expected to have been received. And, of course, at this point, there is very little latency, because I'm running them in the same project. So the time now is 1:45, and that's essentially what the watermark also is, there's not much of a latency. But if there were, you would basically see that difference, that network latency being learned. The system lag here is just four seconds. And we can also see the number of elements that are being processed. So 15,000 elements, 15,000 sensor readings have been processed so far. And they're all getting transmitted to the next step. But then if we go down here where we are computing the average, there are 30,000 sensors readings that come in. But what goes out is only 3,000 of them. So essentially, there is an average being computed. And it's the average that is getting written out to the BitQuery row, so about 3,000 rows so far essentially have all been written out, right? So this is all in the streaming buffer in BitQuery, and that's useful to realize. It's not actually yet stored in BitQuery permanently. It's in the streaming buffer. Remember what we talked about. We said that we can query within BitQuery, even as the data are streaming in. So let's go ahead and try that. So we'll go ahead and we will do a select. So I'll go into BitQuery, and I'll go into demos. And you can see there, it doesn't look like the table is flushed into disk. But we can still go ahead and do a query. So we'll go ahead and do a query and the query that I want to do is SELECT * FROM, this is going to be my same project. So demos.average_speeds ordered by time stamp, let's go ahead and change the options to be, and then run it. So let's hide that options, and boom, we basically have data. So again, it's not yet stored on disk, but we are querying off the streaming buffer into BitQuery, as the data are coming in. And we basically have data from 20:47, right, that's basically it, 147 here. So this is basically converted to UTC, but you can see that the data are up to date. And you basically have the latitude, the longitude, the highway direction, the speed, and the sensor ID. Those are all the information that has just come in. But these speeds, because this is, again, the average speeds that we're looking at. Those are the speeds, so that's why it's 72.458333, etc. It's averaged over a few readings over that time window, okay?

We can also look at, now we can change this. Let's go ahead and find just the maximum time stamp. So select max time stamp from,

That, and that will tell us the last time stamp. So again, this is just, SQL, so that's the last time stamp in that we've gotten. We could also look at the last ten minutes.

And so let's go ahead and do this and add -60600, okay, and we could do select,

Timestamp and sensorId. What was the name, sensorId? Well, we could do a timestamp, and I'm pretty sure there was a highway. Okay, so let's do that. Okay, let's just do timestamp on that. I don't remember what the, I see, this unfortunately is not standard SQL. So I need to go back and say I want to use legacy SQL here, and I run the query.

And with legacy SQL, I need to put it in parenthesis. Let's just do that, SELECT * FROM demos.average_speeds ordered by timestamp block.

It's an invalid time, so we didn't have enough data there. So let's just go ahead and reduce it to that end pool, so we do have now all of the data that is in the last 6,000 milliseconds.

Yeah, the reason the previous one didn't work with a few more is that we didn't have data for that long a period of time. We just started the simulation.

So let's also go back into data flow, and we can look at the number of workers. At this point, there's not much going on. So we're seeing that the job is actually just using one worker, and it's enough to keep up. But if our job had needed more workers, we would have basically seen this auto scaling and this graph reflecting the number of workers that's happening at any point in time. We could also monitor the logs. So you can click on logs. You can look at the logs in Stackdriver. Or you can look at the logs right here, and you can go into the Stackdriver logs, and you can monitor it, I'll skip that.

And then let's go ahead and do another streaming pipeline. This time, we will do current conditions. And it's actually, no, this is just to show you that you can have multiple

conditions, multiple things happening at the same time. So in addition to average speeds, we can do current conditions.

And then, if you go back to your BitQuery, your data flow,

You will see, let's see, let's wait for it to launch. It's still not launched.

We can go down here, and you'll see that there are two streaming pipelines now currently running. And they're both using independent resources, but they're both streaming into the same BitQuery data set.

So what we've done in this lab is that we've taken the simulated data, and we've processed it. And not only have we processed it, we kind of peeked ahead a little bit. As we are processing it, as we are computing averages, etc, we're also querying it, right? We are doing that in BitQuery, but that's what you're going to be looking at next in the next chapter, is how to build dashboards and how to do SQL queries on data that's streaming.
