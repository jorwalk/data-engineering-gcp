## Challenge #3: Need instant insights
So the solution to Challenge #2 on GCP, how to deal with late data, unordered data, is to use Cloud Dataflow.
Challenge #3 is that we need instant insights.
We need to keep the data moving to keep the latency down, right? The data have to keep moving, otherwise there'd be significant delays. We can't just stop the data because we haven't analyzed it.
But at the same time, while the data are coming in, we want to power live dashboards.
So what do we mean by this?
In order to achieve low latency, a system must be able to perform message processing without having to store the data. So you shouldn't have to store the data and then do the query on the stored data. You should be able to do streaming analytics on a stream buffer of data, because storage adds latency.
## BigQuery lets you ingest streaming data and run queries as the data arrives
So BigQuery lets you do this. With BigQuery, you can ingest streaming data, and run queries is the data flies by.
So from Dataflow, remember, we wrote a p transform, BigQueryIO.writeTableRows.to some table.
Well guess what, that worked for streaming too.
And even as Dataflow is streaming table rows to BigQuery.
We can go to the BigQuery console or use a BigQuery client and we can run a SQL. Select average speed from this table joined with something else and this SQL query will get carried out on the streaming buffer even before it has been saved to disk, [INAUDIBLE] storage.
So we've looked at BigQuery as a bad system, but in this chapter we're going to look at its streaming ability. It works exactly the same way so which is very cool, right, both Dataflow and BigQuery. The code is the same. Whether you're doing streaming, or whether you're doing batch, but it's very powerful. The fact that you can use the same kinds of analysis, the same kinds of queries, the same kinds of speed transforms on both batch data as on streaming data.
## Need to use unified language when querying historic and streaming data for seamless integration
So that's important, right? We need to be able to use a unified language when you're querying historic data and streaming data because you want a system that can deal with your reference data, your historical data, and your live data. You need that if you want to have real-time insights. For example, if you want to do fraud detection, what's fraud detection but figuring out if this live data is very much unlike the historical data. So this thing that this credit card is being used for is unlike anything in that user's history. So this requires you to be able to analyze both the live data and historical data, and using two different systems for this is hard. It's much better if you can do the same kind of analysis on both historical data and live data. So that's what BigQuery makes possible. To summarize then, Pub/Sub is a global message bus. Dataflow is capable of doing batching streaming, the code doesn't change. It gives you the ability to deal with late data and unordered data. BigQuery gives you the power of doing analytics both on historical data and on streaming data. Later we'll also look at an alternate of sync which is not BigQuery but BigTable. The issue with BigQuery is that it's going to be OK for most purposes, but if you need very high throughput, very low latency, then you have another solution which is BigTable.
## Stream processing on GCP
So this then is the architecture for stream processing on GCP.
You have some data source, and that data source provides events, metrics, and so on. And this data source could be a sensor network, a mobile application, a web client, a server log, even something from the Internet of Things.
And it goes into a message bus.
This message bus, Pub/Sub, is reliable, it's high throughput, it's low latency.
It buffers up messages, and then you have a stream processing system, which is dataflow. This is a computational framework that can carry out computations on data streams, continuous computations, computations that you have to keep carrying out all the time.
Meanwhile, you take those computed values, typically aggregations of sorts or maybe even the raw data, and you stream it in to something like BigQuery or into Bigtable. And this gives you the ability to do ad hoc interactive queries on this data. So dataflow is about continuous queries, queries that you carry out all the time. BigQuery and Bigtable are about user-generated queries, ad hoc queries, queries that you do once in a long while.
So the difference between BigQuery and Bigtable, again, is that the latency in BigQuery is on the order of seconds. The latency in Bigtable is on the order of milliseconds. BigQuery is sequel, Bigtable is no sequel. But all of these are fully managed no-op services.  
