## Variable volumes makes it possible to derive real-time insights from growing data
So challenge number one, variable volumes. So volumes keep changing, and so you need the ability of your ingest to scale. So think again of our credit card scenario. The volume of transactions changes between holidays, nights, etc.
And in spite of variable volumes, our ingest application shouldn't crash. It needs to be available.
At the same time, we need any messages that we receive to remain received.
We shouldn't have people making a purchase and somehow, we've lost the record of when they've made that purchase. In other words, it needs to be durable. So available, our ingest shouldn't crash. And durable, we shouldn't lose these messages. Both of these are very important.
So when we talk about scaling here, we're essentially talking about being able to deal with faults as clients and servers and storage systems etc, fail unexpectedly. So how do you do that? How can you be fault tolerant while dealing with spikes in data, changes in the volume of data?

## Tightly coupled services propagate failures
What you cannot do is to tightly couple the sender and the receiver.
If the sender is directly sending messages to the receiver, and if the receiver gets overwhelmed and crashes then either all of those messages get lost, it's not durable in other words. Or the sender itself will get overwhelmed, so the sender won't be available in other words. So if you directly couple the sender with the receiver, you either have durability problems or you have availability problems.
So this is the case when you have, for example, a fan in. So you have multiple senders, sending messages to a receiver.
This diagram illustrates what happens when one of the senders has a problem. Perhaps it's sending lots and lots of messages. Now, the receiver is faced with that problem and so the receiver crashes, and that causes the whole system to go away.
This third algorithm illustrates a fan out scenario. So the sender is sending messages to multiple receivers, and this illustrates what happens if a receiver has a problem.
Now, the sender has to keep storing those messages for this receiver and at some point it gets overwhelmed, and it crashes, and brings the whole system down with it. So in order words, if you have tight coupling between a sender and a receiver
then errors propagate and that's not a good thing.

## Loosely-coupled systems scale better
So the solution to this is to buffer. So don't send messages directly from the publisher to the subscriber. But instead, send messages to a message bus.
And the message bus buffers these messages. And this architecture scales a lot better.
The architecture handles variable volumes. All the scenarios fan in, fan out, right? Whether it's fan in, whether it's fan out, it all still works. So, the message bus takes care of durability. It takes care of availability. So, subscribers and publishers, the message bus are loosely coupled now. And because they're loosely coupled, it scales a lot better.
So if, a subscriber goes away, the message bus basically just keeps those messages until the subscriber comes back and starts continuing those messages again. Or if a publisher goes away, then that's not a problem until the publisher comes back and starts sending messages.
So, this is pretty tolerant to failures at every which stage.  
