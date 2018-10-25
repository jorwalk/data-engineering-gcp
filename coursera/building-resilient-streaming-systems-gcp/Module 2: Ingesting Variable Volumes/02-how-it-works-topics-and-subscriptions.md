## How It Works: Topics and Subscriptions
So let's look at how it works. And essentially the way Pub/Sub works is through topics and subscriptions.

So Pub/Sub implements an asynchronous publish subscribe pattern. So a publisher sends messages to a topic, so you could have multiple topics in Pub/Sub and a publisher sends a message to a topic.

If a subscriber is interested in those messages, the subscriber creates a subscription in the topic. So this subscription is owned by the subscriber.

And therefore if the subscriber goes down, the messages in the topic are held in the subscription for up to seven days for this subscriber 3 to come back up and collect those messages.

It is possible for you to set up a subscription and you can have multiple subscribers that process the messages in that subscription, maybe you have multiple workers, you can set that up. But in any case, every message in this topic is going to be in this subscription,
and that's the way the two sets of subscribers are independent. And you could have fast subscribers and slow subscribers, and neither one of them waits on the other.

So the decoupling of publishers and subscribers means that neither side needs to worry about whether the other one is up, available, overwhelmed, it doesn't matter, the message bus handles all of that.
## Create topic and publish message
To create a topic, you use gcloud. So gcloud is one of the tools that count with the cloud SDK. You can use this from Cloud Shell for example, or you can download the cloud SDK onto your local machine and just run this from the command line. So you say gcloud create a topic, and the name of the topic here is Sandiego. So I'm creating a topic called Sandiego in pubsub. And actually this topic will get created in the project. So it's going to be the namespace of this topic is going to include your project name.

And if you want to publish a message into this topic, you can use gcloud again. So you say gcloud pubsub topics publish into the topic sandiego, publish the message hello. Messages as far as pubsub are concerned are opaque. Pubsub is not going to parse these messages, it's just going to take this blob of bites you give it, and it's going to basically keep them and give them off to the subscribers. So pubsub, as far as pubsub is concerned the messages themselves, there is no parsing, it's just an opaque stream. Now, I'm showing you both of these using gcloud but the thing to realize is that what gcloud is doing is it's actually sending a web call, a REST API call.

And because it's just a REST API call, you can do that REST API call from your own application, all right.

So you can code it up yourself, you can code of the HTTP request and put in the headers, and send it along.

Or you can use one of the pubsub client libraries from any of those ten gRPC languages. Or you can use one of the hand coded libraries. And so that's the third code snippet on the slide that's showing you the Python library. So from google.cloud import pubsub, create pubsub client, and you use a pubsub client in for one of two things. To publish into pubsub or to subscribe from pubsub. So, here I'm going to, using the client and I'm basically creating a topic, sandiego, and then I'm publishing the message, hello, which is just a set of bytes, into the topic.

So both the glcoud commands and the Python code here on doing essentially the same thing.

## Other publishing options
So there are other publish options. One of them, you could just publish the message and you're done. But another way to do this is that you could publish a message adding in extra attributes. So these are key value pairs. Remember what I said, as far as Pub/Sub is concerned, the messages are opaque.

But if you want to pass in metadata about the message.

Then you could pass the message and you can pass in metadata with a key/value pairs. And then when the subscriber gets the message they get the message, but they also get the metadata. So they can look inside the metadata for key value pairs that they're interested in. But the attributes that you're providing, it's totally up to you. You decide what the attributes are and you just put them and you use them on the subscriber end. But one key attribute that we will often use is an attribute that reflects the timestamp of the message. Normally the timestamp of a message
is going to be the timestamp at which you called the publish method. The time at which you publish into pop sum is the timestamp of the message. So, that's going to very well if you're working in real time. But if you're not working in real time, if for example you're assimilating historical data. So you're publishing at some time in 2017, but the data actually comes from 2008. You might want to set metadata which is the time stamp of the message. Because again, pop service aren't going to poll this message and figure out what the actual time stamp is inside the message. The other up thing that you want to be aware of is that you don't have to call Publish one at a time for every message. Every time you call Publish, there's a network call from your application to Pub/Sub. And network calls introduce a little bit of latency. So if you have a bunch of messages that you want to publish, a good idea is to do a batch. So here's the way you do it, you say with topic.batch() as batch. This is in Python but you can do the same thing in any language that is supported. You say batch.publish first message, batch.publish second message batch.publish third message. And then when you come out of the with statement the whole batch gets published all at once.

So by publishing a set of messages as a batch, you help reduce the network costs.

## Push vs. Pull delivery flows

So there are two types of delivery flows, and a subscriber can choose. So within a topic you can have some subscribers that are pulling and some subscribers that request a push.

So how does this work?

In push delivery, pub/sub is going to initiate the request to the subscriber application to deliver messages. So whenever there's a new message in the topic, that message gets delivered to the subscriber by pub/sub. In a pull subscription, the subscriber asks pub/sub, is there a new message?

And if there is, it gets the message.

So when the subscriber is pulling messages, what does the subscriber need? The subscriber simply needs to be able to make a web call. It needs to be able to make an API call. That's pretty much it. So for pulling, the subscriber just needs to be able to make a call. How about for pushing? For pushing, the subscriber needs to be an HTTPS web application. It needs to have a web hook that is accessible via HTTPS because what pub/sub is going to do is that it's going to call that URL.

So in other words, in a push subscription, your subscriber needs to be a server application.

Okay, so the pull subscription, there's going to be a delay between publishing to the topic and the subscriber getting the topic. Because a subscriber typically is checking periodically. If it's checking 30 times a second, then you basically have a latency of 1 over 30 seconds, right, approximately.

In a push subscriber, though, you have no delays.

So the push is ideal if you want low-latency immediate processing of messages. The pull subscription is really good if you will have lots of subscribers, and those subscribers are dynamically created.

So use push if you want almost no latency, close to real time performance.

But wait a second, what happens in the case of a push subscription if the subscriber isn't running?

So the subscriber has set up a subscription. The messages are going into the subscription, but pub/sub will not be able to reach the subscriber because, for whatever reason, that web URL is not accessible. Well, in that case, what pub/sub does is that it does an exponential backoff. In other words, it tries and then it increases this time and then the delay and it tries again. And then it increases it some more and tries again. Increases it some more and tries again and so on. So then you can control this retry. But it'll continue retrying for up to seven days.

In a pull subscription, the subscriber explicitly requests delivery of a message that's in that subscription queue. And the pup/sub server basically responds to the message or it responds to an error if the queue is empty. And it responds with an acknowledgement ID.

And the subscriber is then responsible for calling the acknowledge method using that acknowledge ID.

If it doesn't, then pops up things that it sent the message, but in between the subscriber asking for the message and cloud pub/sub sending the message, the subscriber went down.

And because it believes that, it will try to resend the message. So if you're doing a pull subscription, it's important to acknowledge the message. Of course, the client libraries that you're using handle all this for you. This is if you are calling up the HTTP calls yourself.

But it's also important to realize why you might have duplicate messages delivered, right? You will have duplicate messages if you as a subscriber invoke a message call. You get the message, you process the message, or you do something to the message. But then between processing the message and acknowledging it, you crash.

So you've processed the message, you've done it. And then you come back up, and because pub/sub didn't realize that you actually meant to send acknowledge, but you crashed in between, it's going to send you this message again because that message was not acknowledged. But this is the kind of thing that data flow is going to handle if data flow's the next step in your pipeline.

In a push subscription, the pub/sub server is going to call this pre-configured HTTPS endpoint. And the subscriber's response to that call serves as an implicit acknowledgement. So if you get a success response, a 200 response, it indicates a message has been successfully processed. And so the pub/sub system can delete it from the subscription.

A non-success response, a 400 response, for example, a 400, 404, 429, etc., that that response indicates that the pub/sub server should resend the message. And to ensure that subscribers can handle the message flow, what pub/sub is going to do is that it's going to dynamically adjust this rate at which it's sending these messages. And it's going to use an algorithm to rate limit retries. So that it's trying to make sure that your subscriber endpoint doesn't get overwhelmed.

## Creating subscription, pull messages
So, we talked about creating a topic and we talked about publishing to that topic. So, how about the other side of the equation? Well, if you're interested in messages as a subscriber, you will create a subscription. So, gcloud pops up subscriptions create. So your creating a subscription and your creating a subscription for messages that show up in the topic San Diego, and you're giving that subscription and name. And again, this subscription gets created in your project and the project for the message and the topic could be different than the project for the subscribers. So, you do that. And then whenever you're interested, you can do a pull. So, it's a pull delivery here. You are saying, give me the next message and using gcloud pubsub subscriptions pull automatically acknowledge and you pull and the next new message out of that subscription. And now as messages get created into the topic, published into the topic Sand Diego, they're also going to show up in this subscription. You only processed to one of them. All the other messages are queuing up in that subscription for the next time for you to call pull. So whatever you can do in gcloud and gcloud is the mix of rest API call, you can also do in your client library code. So, here's Python.

So, you go to the topic.

The way you create a topic is very similar to the way you created the topic earlier. Recall that we created a topic by creating a pubsub.Client() and we said, client.topic("sandiego"). So, you get your topic. And once you have your topic, you create a subscription with the name of your subscription say, mySub1 and you create that subscription. And then whenever you're interested in a new message, you say, subscription.pull and you can ask pull to just sit and wait. Block, in other words, for a new message until a new message comes in. Or you can say, return_immediately. So here I'm saying, return_immediately is True. So when my results come back, they'll either be none or they'll actually have a message in there. So if the results is not none, I acknowledge subscription.acknowledge and here's the acknowledge for acknowledge ID and message in results. So all of the messages that I got in results, I'm going to acknowledge all of them. But then my results have all of the messages. I then go ahead and process them.

So in this lab, we will look at how to publish streaming data into Pub/Sub. So, we will have some historical data. We want to simulate it and we're going to call this Python program send_sensor_data.py. and we will use it to publish into Pub/Sub. So, take a look at the Python program and then look at running it. We will go through the process of creating all of the artifacts that we need in order to do this. 
