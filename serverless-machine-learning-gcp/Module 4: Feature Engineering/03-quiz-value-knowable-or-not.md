0:00
So let's take an example here.
0:04
Again, our discount coupon case. Total number of discountable items that have been sold.
0:15
Well, how long a period
0:22
are we looking at this total number for?
0:26
And how long does it take for us to know that value? This is not a yes or no answer, but it's a question that you need to ask before you take it as an input. The number of discountable items sold the previous month. Yeah, now this is getting closer.
0:46
This seems like something that you should have available to you. So notice that there is a way to define this. If it's something as vague as total number of discountable items sold, it's too vague. You don't have a time period, you don't know how long it takes to collect. But if you make it a lot more practical, total number of discountable items sold the previous month.
1:10
At this point, you've defined it in a way that you can have it. And of course, this time frame is going to depend on the latency in your system. So this is, again, a prompt for you to go find out these kinds of things. How long does it take for you to actually get this data before you can use this data in real time?
1:30
Number of customers who viewed ads about an item.
1:37
Again, this is a question of timing.
1:41
How long does it take for you to get the ad analysis back so you can use it?
1:49
So now, this is about fraudulent transactions. Whether a card holder has purchased these items at this store before. Again, you have to define this very carefully.
2:05
With this card, as of three days ago, because it takes three days before we'll actually get the data.
2:13
It may turn out that when somebody uses a credit card, we don't know about it immediately because the store takes three days to actually send in the transaction.
2:24
And so if it takes three days before we'll have the data in hand,
2:29
when we do the training, we have to train the data as of three days ago.
2:36
This is important.
2:38
You cannot train with current data and predict with stale data. So you have to go into your data warehouse, and you cannot just use all of the values from the same row because not all of those values are going to be available at the same time. So what you may have to do is you may have to go ahead and take the value of some field, the number of times a card has been used,
3:05
not of the current date but of three days ago. So you have to train with stale data, if stale data is what you will have in real time.
3:17
You've got to define your problem. And then when you train, you can't train on warehouse data that's up-to-date. You have to train on warehouse data as of three days ago.
3:30
That's because when you do your prediction, when you do your serving, the information that will be in your database at that time will be as of three days ago. So when you're doing a prediction on May 15, the data in your database is only going to be current as of May 12. Which means when you're training, and you're training on some data on Feb 12, that will use the input variable. The number of times this card has been used as of Feb 9, you have to correspondingly correct for the staleness of your data.
4:13
I hope you see this difference. You can't just use warehouse data as it is.
4:19
If you take a fraudulent transaction, and then you ignore the purchases happening the previous three days, that's the only way you will get the right kind of training and the right kind of model. So we need to be able to do our computation, taking into account the latency of the data. Because if you don't, then you have training servings queue. If you've trained your model, assuming that you know exactly the data up to the minute, but then at prediction time, you'll only know the data as of three days ago, then you'll have a very poorly performing machine learning model.
5:03
Do you understand this point? If you don't, please rewind the video, make sure you get it. This is a very common mistake that I see people make.
5:14
So you have to think about the temporal nature of all of the input variables that you're using.
5:24
Okay, next one.
5:26
Is the item new at the store? And so it cannot have been purchased before.
5:33
Yes, this is something you should know from your catalogue.
5:37
Perfectly valid input. Category of the item being purchased.
5:45
No problem, again, we'll know it. We'll know if this thing's a grocery item, or if this thing's an apparel item, or if this thing's an electronic item. No problem, we can look at the time, and we know what it is. Online purchase or in-person purchase.
6:02
Yes, you know this thing, too, in real time. It's not a problem. 
