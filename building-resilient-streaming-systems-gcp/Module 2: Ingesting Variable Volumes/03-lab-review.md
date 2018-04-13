## Lab Review

So in this area lab, we are going to be creating a topic, and we're going to be publishing traffic data into it. So to get started, let's go to the web console. Your web console might look slightly different than mine. Don't worry, these things keep changing. And so we open up Cloud Shell. And notice that I'm in my project cloud training demos when I did that. So my Cloud Shell is also going to be in that project. And it's going to be logged in as me. The project ID is cloud-training-demos. So let's go ahead and cd into training-data-analyst/courses/streaming/p- ublish. So in here, I have a script called send_sensor_data.py. This is the one I'm going to be using. But before that, let's explore Pub/Sub a little bit. To explore Pub/Sub let's go ahead and create a topic called San Diego. So there's two ways to do it. One way would be to go up here, and where there is Pub/Sub. So find Pub/Sub, and say go ahead and create a topic. And the topic would be in the projects/cloud-training-demos because that's the project that I'm in and the name of my topic could be San Diego.

So let's go ahead and create that topic. And at this point the topic has been created. There is the San Diego. And now let's try to publish something into it. We could publish something into it again using the web console by saying publish message. But let's do, this time let's do it using G-cloud. So what we will do is I will say G-cloud beta pops-up topics publish. So we'll go ahead and publish into the topic San Diego, the message hello. And at that point we've published it, we get a message ID so that's great but where does this message go?

The answer is nowhere. There's no subscribers, so there's nobody. There's no push subscribers. So there's no URL that we registered that is this message is going to send to. There are no poll subscribers, either. As that said, I'm interested in messages from this topic. So we published a message to the topic, but nothing really happened with that message. So let's fix this. The next thing that we want to do is we want to create a subscription for the topic. So let's create a subscription and we will call that subscription mysub1. So it's going to be a subscription created in the topic San Diego. And now that we have mysub1 let's go ahead, let's see.

Okay sorry. I guess I've already created that subscription before.

So let's remove that, I've created it while I was trying things out. So let me just remove that subscription and let's now create it again.

So now, this subscription will get created and let's go ahead and pull messages from that subscription. So, what are we doing?

We are saying subscriptions pulled from the subscription mysub1 and we basically get zero items. Why do we get zero items? Because this subscription was not around at the time that we published a message into the topic. So now, let's try to publish some other message. So I can hit the up arrow to get my old messages back. And last time I published hello. This time, let's publish hello again.

And then we get a new message ID. So last time the message ID was 9772. This time the message ID is 9281. So now let's go ahead and click the up arrow. So this time, now I'm doing subscriptions pull out of mysub1. And we will get the message hello again whose message ID is 9281. There were no attributes, so there are no attributes here.

So let's go ahead and now cancel the subscription, because we don't need it anymore. So we could again do what I did last time, I could use a interface and go to the subscriptions and cancel it. But what I'll do is I'll use the G-cloud command and cancel the subscription.

So the next thing that we want to do is that we want to explore send_sensor_data.py. Now to explore it I like to use the code editor, so let me just go ahead and launch the code editor. So again the way I launched that was that I clicked on this icon, an icon that looks like an editor. I clicked on that icon in Cloud Shell, at the top of Cloud Shell. It opened up a new window and it's going to basically give me all the content. So there is training-data-analyst. I can go into courses. I can go into streaming. I can go into publish, and sensor_data.py. And we see that very nicely it's all syntax colored. I can edit, I can save, I can do File, Save. And the cool thing is that it's now getting saved on that Cloud Shell VM. So let's go ahead and let's look at what this code is doing.

So what the code is doing is that, note there's a published method. So publish a set of events to a topic. And what it does is that it goes ahead and publishes all of those events as a batch. Set the number of observations just the length of events if it is greater than zero so there's stuff to publish. So which topic has badge it goes ahead. And for event data and events published that events data. So it just publishes all of them.

And the next thing that we do is to simulate, we're basically going ahead and we're sleeping in between. So we compute the number of sleep seconds, and we sleep for the appropriate amount of time. And after that returns we go ahead and we publish the next set of messages. So essentially what this code is doing is that it's going ahead and reading

a file and figuring out all of the messages in that file. Parsing the times stamp and using it to simulate. So for example if you have a set of messages that all happen in 2008, at 12 o'clock, it publishes all of them as a batch. And then it goes ahead and looks at the next message. And if the next message is a minute later, it sleeps for a minute, and then it publishes the next set of messages. Now if we take the data and we publish it exactly at real time speed this is going to take a long time. So what this also does is instead of just sleeping for a minute, if the next message is a minute away. It asks you what speed would you like to simulate at? And if you say you want to simulate at 30 times the speed instead of it sleeping for a minute, it would only sleep for two seconds. So that's basically what it's computing here. It's computing the simulated time elapsed and figuring out how many seconds to sleep based on that amount of time.

So let's go ahead. So now that we have this code, let's go ahead and download the traffic data set, so we can use it to simulate it. So to do that, the other thing that we do, is in publish we have the download data. And what that's doing is that it's getting the sensor observations from the 2008 to this local machine. So I'll do ./download data under switch and it's going to get me that data. And let's authenticate the shell to make sure that we have the right permissions. So there, okay.

And Okay, so now follow this link.

Paste in the browser.

And that's where I'm going to be I love it.

And copy this, paste.

So the reason that this is important is that I would like some piece of code to run as me, with my permissions. And so you don't want to store user credentials in a virtual machine. Because this virtual machine is part of a project. And because it's part of a project anybody in that project will be able to go look at your user credentials. So you don't want to save your user password on a virtual machine. So this is why we go through this what do step. And we say for this particular session for this particular time it's okay. But it's not going to get stored for, once I close the cloud store machine or even if I open a new tab. That tab could be someone else logged in to the same project. And we don't want them to be able to do things as me with my user credentials. So this is why we go ahead and set, for this application, for this period of time, let it work with my personal permissions. So I'm basically giving my permissions for this application to run as me for short period of time. And then I can run the sensor data and [INAUDIBLE] says specifying the speed factor as 60. Which means that it'll send an hour of data every second. So for example, it's just published 477 events from 2008, November 1 and then slept for five seconds. So this is actually five minutes of data. Because our sensors are sending data every five minutes. So, it's sending all the data that it received and then the next set of data is going to be five minutes later. So this is going to sleep for five seconds. Because if something's sleeping for five minutes it'll take too long for us to start seeing results.

So once we do this, so we send this data. And then let's go ahead and create a new tab in Cloud Shell.

Now this new tab for all GCP nodes could be someone else who has access to this project. So this tab doesn't have my user credentials either. So I will go into cd training-data-analyst/courses/streaming/p- ublish. And let's go ahead and create.

Add subscription for this topic, and then see if we can pull messages from that subscription. So it's creating a subscription.

And then we are pulling a message and this is the last message. And you can see that the message basically contains some kind of a time stamp. And it contains a latitude, a longitude. This happens to be I think the highway, highway 163 in the south direction, lane number one, the average speed is 69.9 miles per hour. So that's the data that we got from that traffic sensor. And if I run the acknowledge command again I will probably get a new message.

And yes so this time it is 60.8 miles per hour from lane number two, right? And it's from actually the same time stamp, cool.

So let's go ahead finally, so now that we have a topic, we have a way to publish things into that topic, let's go ahead and delete.
