0:03
The final thing that we're going to talk about is, how to bring human insight into a problem. Beyond the fact of taking the raw data and using it as it is, let's think about what other things we can do. And one of the most powerful things you can do is something that's called a feature cross.
0:25
Let's say we have a machine learning model needs to figure out,
0:34
So that's my machine learning model, is it not a taxi.
0:40
And we have two inputs, the color of the
0:48
Fair enough,
0:52
So you have data that looks like, And you have a machine learning model. And you want to use the machine whether it's a taxi or not. Two inputs, And
1:13
Let's just, for argument sake,
1:19
What's a linear model?
1:21
There is a weight associated with a color, So you have a weight for
1:30
And you want to basically say the output
1:36
So let's say, yellow cars in New York
1:46
Now what happens?
1:48
Regardless of which city it is in, all And that's not true.
1:56
So you say, well, in New York, So you basically go assign unfortunately that means that white cars
2:10
You see the problem? It's impossible to, really, for a simple linear model, to somehow give a weight such that our idea that That's very hard for
2:28
And the way you would solve this, But can we bring human
2:37
Make the machine learning probably easier. So we can still use a linear model but
2:46
Well, you can't.
2:48
What if you add a third
2:52
And this third column is simply
2:57
So red, Rome, red and Rome. White, Rome, white and Rome, etc. And now you take this third column and So that RR is one if it's RR and all the other columns are zero and so on. Now you pretty much see You essentially provide yellow in New York, and WR of YN, yellow in New York and zero weight to everything else and The whole point being, make it easy for
3:47
Now, this is something that, again,
3:51
But the things to realize is that you learning model.
3:57
The machine leaning model is
4:04
Okay, so make it easy for
4:08
And the way you do this is So you take the two categorical you cross them to provide And that crossed feature makes life
4:27
So heuristically, if your human insights is what's important, then you explicitly So how do you create a feature
4:43
You basically say, I want to do because remember categorical features So you have the sparse column day of and then you combine them you say that you want to do this, and And you could choose any number of you essentially get some amount of
5:21
So regardless of how you created whether it is a sparse column with keys, sparse column with integerize features,
5:32
So all that you do, as long as you have two or more columns. Notice that this square So you basically provide a list of
5:54
So in our taxi model, this is how We're going to say, 3 o'clock on a Friday is rush hour.
6:07
We're not going to explicitly say it. It's just that we know that in those are going to be important. So we're going to cross the hour of and let the model figure associated with rush hour at
6:30
So how do you do your feature crosses Well if you have a real valued column, You discretize floats. So for example, This comes out of a data So you're trying to predict each house has a latitude and you look at the distribution of latitudes
7:01
Why do you have two spikes? Well one of them is the other is
7:10
Okay, so you look at this and difference between a latitude of 34.01 and Well, the two latitudes Pretty much the houses
7:28
So rather than take the latitude to run the risk of overfitting, you basically
7:42
So you say, go ahead take my latitudes And now, it's a categorical feature and
7:54
The code itself Is just very simple. You'll basically say I want to passing in the original real valued
8:14
And how do I get the boundaries? In this case, I'm asking np, np is num pi, to equally divide values between 32 and 42,
8:31
Why do I just not hard code nbuckets?
8:35
Because I don't know whether I 100 buckets, or And because I don't know, And the way I can experiment, is if I make this a command line
8:56
We will look at this shortly. But if I make this a command line then CloudML engine. We can tell it to go explore find a good value for nbuckets. So this is basically our
9:16
We take our inputs, the pre processing is where We do our feature crosses, It's going to do all of these things. And then you're going to which is like your feature crosses And then you're going to train your model. And your pre processing stage is the vocabulary for your categorical What employees do I have in my data? What states do I have in my data? What ZIP codes do I have in my data, etc.? 
