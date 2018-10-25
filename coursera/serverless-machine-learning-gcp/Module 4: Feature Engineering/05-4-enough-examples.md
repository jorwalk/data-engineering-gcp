0:03
Point number four, you need to have enough examples of in feature value in the dataset.
0:15
So it's the value of the feature, you need to have enough examples of this.
0:22
My rule of thumb and this is just a rule of thumb, is that I need to have at least five examples of any value, before I'll use it in my model. It's just my rule of thumb. At least five examples of a value before I use it in training.
0:39
So what do I mean by that?
0:42
What it means for example if you have category equals auto, then you must have enough transactions, fraud or no fraud of auto purchases, so you should have enough fraudulent auto transactions, enough not fraudulent auto transactions. If you- if you have only three auto transactions in your dataset, and all three of them are not fraudulent, then essentially a model is going to learn that nobody can ever commit fraud on auto transactions. And that's gonna be a problem. So you want to avoid having values of which you don't have enough examples. And notice that I'm not saying that you have at least five categories. I'm not saying that you have at least five samples. I'm saying for every value of a particular column you need to have at least five examples.
1:43
So again, let's consider that we are trying to predict the number of customers who will use a discount coupon. And we have, as a feature, the percent discount of the coupon.
1:55
So let's say I have a coupon that has a 10% discount.
1:59
We'll have at least five times that a 10% off coupon has been used. Probably, I mean I hope, if I have a 10% off or a 5% off or a 15% off, we probably at least five examples of these.
2:16
But what if we have this one special customer to whom you give an 85% discount?
2:24
Can you use that in your dataset?
2:27
No, you don't have enough samples. That 85% is now way too specific. You don't have enough examples of an 85% discount, so you have to throw it out, or you have to find five examples of when you did give somebody an 85% discount.
2:48
So that's cool if you have discrete values.
2:52
What if you have continuous numbers? Well, if you have continuous numbers, you may have to group them up, this is called discretization. And then see, if when discrete bends you have at least five examples of each bend.
3:08
So let's take the second one. The date that a promotional offer starts. Can I use that?
3:16
Assuming, again, that you may have to group things, all promotional offers that start in January. Do you have at least five promotional offers that you started in January? Do you have at least five promotional offers that you started in February? If you don't, you may have to group things again. You may not be able to use date. You may not be able to use month. You may have to use quarter. Do you have at least five examples of things that started in queue one and queue two and queue three and queue four, for example?
3:49
So you may have to group up your values so that you have enough examples of each value.
3:56
Number of customers who open an advertising email.
4:01
Yes, whatever number you pick, you hopefully have enough examples of that. You have different types of advertising emails, and you have some that were opened by 1,000 people, and some that were open by 1,200 people, and some that were opened by 8,000 people.
4:17
Maybe you will have enough till you get to the very tail end of your distribution, and then you only have one email that actually got opened by 15 million customers. And you know that's a outlier, you can't use 15 million in your dataset.
4:36
Whether a card holder has purchased these items at this store before.
4:44
This is to predict whether a credit card transaction is fraudulent.
4:48
Will we have enough examples of card holders who've purchased, and card holders who haven't purchased?
4:56
Yes, you would hope. It doesn't matter which item, which store because the way we're defining this we should have enough customers who've purchased it and enough customers who haven't purchased it.
5:09
But suppose we define this as whether the card holder or a customer has purchased a bag of diapers between 8 PM and 9 PM at this specific store. Now, it's way too specific. So it really depends on how we define it. But if we define it general enough, such that we'd have enough examples of a value, then you're good.
5:31
Distance between a card holder address and the store.
5:37
Will we have enough examples of customers who live ten miles away?
5:42
Sure. Sure. 50 miles away?
5:48
Maybe. Start to becoming a problem. So this is basically where you may have to start grouping them together. You can't use a value as is. You say, I'm going to take all the customers who live more than 50 miles away and I'll treat them as one lump. So you're not going to actually take 3,243 miles and use that in your training dataset. Because now your neural network will happily go and train that any time that somebody comes in from 3,243 miles away, whatever that person does is fine.
6:32
Because that one time that this person came and used their card, they didn't commit a fraud. So that's basically what you want to avoid. So we're talking about the values of the features, not the values of the labels. If you're going to use rainfall amount as a feature, you've got to make sure that you have enough days that it rained one centimeter, two centimeters, three centimeters, etc.
6:54
So how do you do this? How do you make sure that you have enough examples of a particular value?
7:00
Plot histograms of your input features.
7:04
Make sure that we have, again, my rule of thumb is five.
7:07
Okay, make sure you have at least five, but don't take the number five as gospel. Just my rule of thumb, something to hang my hat on. So I try to look for at least five. But you want to have enough examples So consider this category of the item being purchased. Sure, you'll have enough examples of that for any category that you choose. Online purchase, in-person purchase. Again, you'll have enough examples of those, so those shouldn't be a problem. 
