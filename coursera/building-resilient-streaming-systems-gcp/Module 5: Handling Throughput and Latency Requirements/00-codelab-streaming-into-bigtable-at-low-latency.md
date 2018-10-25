Streaming into Bigtable at low-latency
In this lab you will use Dataflow to collect traffic events from simulated traffic sensor data made available through Google Cloud PubSub, and write them into a Bigtable table.

What you need
To complete this lab, you need:

To have set up the project
All other resources necessary for completing the lab you will create during the lab.

What you learn
In this lab, you will learn how to:

Launch Dataflow pipeline to read from PubSub and write into Bigtable
Open an HBase shell to query the Bigtable data
Begin the lab
https://codelabs.developers.google.com/codelabs/cpb104-bigtable-cbt/

In this lab, what we will do is that we will take the Dataflow pipeline that we had in earlier labs, the pipeline that was producing and processing the current conditions, and writing them to BigQuery and we will also write it to Bigtable. In order to do that, what we will do is that we'll use the current conditions Dataflow pipeline, and in that pipeline there's an option get BigTable. And what this option does is that if it's enabled, it also writes into BigTable the current conditions. So I I have started the Pub/Sub publishing pipeline. So I'll go ahead and start in new Cloud Shell window.

And within in these new Cloud Shell window I'll make sure to authenticate as myself so that everything runs with my user credentials, the gcloud auth application-default login.

And Yes.

And make sure that I go through the OR2 workflow.

So remember that this is something that you have to do for every Cloud Shell tab that you open, so that everything that you launch from this Cloud Shell tab runs as yourself. So at this point what we're going to do is that we will go into training data analyst/courses/streaming/process/sandi- ego. And let's make sure to create the Cloud BigTable.sh. So what does that do? So if you go into the sandiego directory, there's a create Cloud BigTable.sh and what it does is that it calls gcloud BigTable instances create

an instance called sandiego. So that's what we're going to be creating. So when I do ./create_cbt.sh, I can now go into the BigTable instances.

And you will see, That there is an instance now called sandiego that's been created. So now that this instance has been created, I can go ahead and launch, run_oncloud.sh. And run_oncloud.sh runs the project name, which in my case is cloud training demos. And bucket name which is the same, and it also runs a class name which is CurrentConditions, and in addition I'll do -bigtable, so that I'm writing to both BigQuery and to Bigtable. So at this point I will launch the Dataflow pipeline and it'll take two to three minutes to completely launch and compute the first set of aggregates and write them out. Actually, there's no aggregates, this is an element by element processing. So it takes about two to three minutes to start, so we will wait for that to happen.

So now if we go back to Dataflow and we look, we should see a current conditions that has just been started, it's running for 25, 28 seconds, and so this has started, let's wait two to three minutes until something starts happening in that final box to write to Cloud BigTable.
So at this point, the number of CPU's running is still zero, so things are still starting up.

Right, so now 4 CPU's have started.

And we can see that we launched the pipeline. The BigTable is true. So there's one worker pool with one worker. That worker has four CPUs. So that's essentially what we're currently using.

The app name is CurrentConditions, and it's streaming. And it has been drumming for a minute and a half. So let's wait approximately another minute and it should completely start. Again, I'm looking for number of elements processed to show off at the final step of the pipeline. Then I know that things are being written in the Cloud BigTable.

Right, so 262 elements per second being written out, perfect. So let's now go back to our Cloud Shell window.

And let's go into quickstart and do .quickstart.sh.

Quickstart is the HBase command line shell and the reason we can use the HBase command line shell to interact with Bigtable, is that Bigtable supports the HBase API. Let me go ahead and start the quickstart. This will give me back an HBase prompt. And once I'm in the HBase prompt, I'll do a table scan of current conditions table.

All right. So the HBase Shell has started, if I was saying, hibe I'm sorry. The HBase API. So the HBase API current conditions and we'll do limit, actually I believe it's something like this, limit.

This too.

No. Better copy and paste.

Let's see what the actual syntax is.

Limit, scan current conditions, limit two.

All right, so now we have received the last two rows, right? We said limit is two. But that row itself consists of many columns. So we've got the line lane direction, lane highway. The latitude, the longitude, sensor ID, etc. But we got it at two timestamps, because we set limit as two. So we got at the timestamp 0900, and we got at the timestamp 0600. So we essentially got two rows of data, but we're storing it as a tall and narrow table. So that's why we have so many entries here. But this essentially corresponds to two rows in our BigTable.

So let's go ahead and try a few other things out.

So we could, again, scan the current conditions. But get me the last 10 rows.

But we want to basically limit it to keys that start with 15 south. So we will only get sensors that are on highway number 15 in the direction going south. And lane number 1 to lane number 999, so essentially all of the lanes. And we will pull in only the speed column. So, let's go ahead and try to do that. I'll do this. Go back, I'll paste that. So, I'm scanning the current conditions, 10 rows for highway 15.

And lo and behold, this is the lane speeds. So over time, so the last ten timestamps, we had 72, 75, 72, etc. So you can see that the latency here is a lot lower than it would be in say, BigQuery. We're able to get the data much faster. But of course, we're not using SQL.

So, you can go ahead and play some other things out, but let's just go ahead Quit, and we can also go back and clean up. So we can stop the Dataflow job by hitting Stop here. But while, instead of just hitting Stop, let's go ahead and look at the BigTable here, where you see that there is a lag of about five seconds in the watermark. It's also very clear between, remember there's the same data that's being written out, and you can notice that the amount of resources needed to write to this training buffer and BigQuery is a lot more than the amount of resources to write out into BigTable. You can look at it at the 4 minutes here, it's just 7 seconds here. So the amount of work that needs to be done, the amount of latency that you're basically going to be encountering, all of those things are lowered because BigTable is meant essentially for high trooper, essentially the scenario that we're dealing with.
