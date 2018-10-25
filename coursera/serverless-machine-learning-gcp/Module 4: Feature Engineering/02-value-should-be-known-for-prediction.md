0:00
Second aspect of a good feature, you need to know the value at the time that you're predicting. Remember that the whole reason to build the machine learning model is so you can predict with it. If you can't predict with it, there is no point of building the machine learning model. So a lot of times you will look in your data warehouse and this is a mistake a lot of people make. You look in your data warehouse just take all the data you find in there, all the related fields and throw them into the model. So if you take all these fields and you just use it in machine learning model what happens when you're going to go predict with it? When you go predict with it, maybe you'll discover that your data warehouse had sales data. So that's an input into your model. How many things were sold the previous day? That's an input into your model. But then it turns out that daily sales data actually comes in a month later. It takes some time for information to come out from your store. There is a delay in collecting this data. Your data warehouse has the information because somebody went through the trouble of taking all the data or joining the tables, putting them in there. But at prediction time at real time, you don't have it. So some of the information in a data warehouse is known immediately, and some other information is not known in real time. And if you have used the data that is not known at prediction time, if you've used such data as an input to your model, now your whole model is useless because you don't have a numeric value for that input that your model needs. So make sure that every input that you're using for your model, every feature, make sure that you will have them at prediction time. You want to make sure that those input variables are available. You're collecting it in a timely manner. Many cases you also have to worry about whether it's legal or ethical to collect that data at the time that you're doing the prediction. Sometimes that's information that you will have available to you in your data warehouse but you cannot collect it from the user at the time that you're trying to do the prediction. If that's the case then you cannot use it in your ML model. So let's take an example here.
2:41
Again, our discount coupon case. Total number of discountable items that have been sold.
2:52
Well, how long a period
2:59
are we looking at this total number for?
3:03
And how long does it take for us to know that value? This is not a yes or no answer, but it's a question that you need to ask before you take it as an input. The number of discountable items sold the previous month. Yeah, now this is getting closer.
3:23
This seems like something that you should have available to you. So notice that there is a way to define this. If it's something as vague as total number of discountable items sold, it's too vague. You don't have a time period, you don't know how long it takes to collect. But if you make it a lot more practical, total number of discountable items sold the previous month.
3:47
At this point, you've defined it in a way that you can have it. And of course, this time frame is going to depend on the latency in your system. So this is, again, a prompt for you to go find out these kinds of things. How long does it take for you to actually get this data before you can use this data in real time?
4:08
Number of customers who viewed ads about an item.
4:15
Again, this is a question of timing.
4:18
How long does it take for you to get the ad analysis back so you can use it?
4:26
So now, this is about fraudulent transactions. Whether a card holder has purchased these items at this store before. Again, you have to define this very carefully.
4:42
With this card, as of three days ago, because it takes three days before we'll actually get the data.
4:50
It may turn out that when somebody uses a credit card, we don't know about it immediately because the store takes three days to actually send in the transaction.
5:02
And so if it takes three days before we'll have the data in hand,
5:06
when we do the training, we have to train the data as of three days ago.
5:13
This is important.
5:15
You cannot train with current data and predict with stale data. So you have to go into your data warehouse, and you cannot just use all of the values from the same row because not all of those values are going to be available at the same time. So what you may have to do is you may have to go ahead and take the value of some field, the number of times a card has been used,
5:43
not of the current date but of three days ago. So you have to train with stale data, if stale data is what you will have in real time.
5:54
You've got to define your problem. And then when you train, you can't train on warehouse data that's up-to-date. You have to train on warehouse data as of three days ago.
6:07
That's because when you do your prediction, when you do your serving, the information that will be in your database at that time will be as of three days ago. So when you're doing a prediction on May 15, the data in your database is only going to be current as of May 12. Which means when you're training, and you're training on some data on Feb 12, that will use the input variable. The number of times this card has been used as of Feb 9, you have to correspondingly correct for the staleness of your data.
6:51
I hope you see this difference. You can't just use warehouse data as it is.
6:56
If you take a fraudulent transaction, and then you ignore the purchases happening the previous three days, that's the only way you will get the right kind of training and the right kind of model. So we need to be able to do our computation, taking into account the latency of the data. Because if you don't, If you've trained your model, assuming that you know exactly the data up to the minute, but then at prediction time, you'll only know the data as of three days ago, then you'll have a very poorly performing machine learning model.
7:40
Do you understand this point? If you don't, please rewind the video, make sure you get it. This is a very common mistake that I see people make.
7:51
So you have to think about the temporal nature of all of the input variables that you're using.
8:01
Okay, next one.
8:04
Is the item new at the store? And so it cannot have been purchased before.
8:10
Yes, this is something you should know from your catalogue.
8:15
Perfectly valid input. Category of the item being purchased.
8:22
No problem, again, we'll know it. We'll know if this thing's a grocery item, or if this thing's an apparel item, or if this thing's an electronic item. No problem, we can look at the time, and we know what it is. Online purchase or in-person purchase.
8:40
Yes, you know this thing, too, in real time. It's not a problem. 
