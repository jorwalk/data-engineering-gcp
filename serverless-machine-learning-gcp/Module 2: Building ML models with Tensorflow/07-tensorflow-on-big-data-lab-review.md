0:01
So in the lab that we just did, right? What did we do? We basically did what we talked about in the slides. First of all we refactored the imput so we could read out of a csv file. I mean I have a method called read data set, again same thing. I need an input function that takes no parameters but I need to know the name of the data set and all of that stuff, so I create a function called re-data set and that returns an input function. The re-data set has the file name number of feedbacks, all of those kinds of things. So when you say get to the train, I read the training data set to when I say, get valid and read the validation data set. Just we have re-using it rather than writing the same function three times, right? So I can just write it once and use the data set twice. >> [INAUDIBLE] >> It's basically the read up to returns two things. It probably returns a line number and a value.
1:00
Okay, and I'm just ignoring whatever it is, but I actually don't know what it returns, okay. But it returns two things and what I really care about is the second thing that it returns. >> [INAUDIBLE] >> Fair enough? Yeah. >> So then, what's in that second thing, one that-.
1:21
To a single file. >> No, it's a line out of a file.It is an array of lines. Value is an array of lines, correct. >> So the output
1:37
of text line reader is [INAUDIBLE] >> It's kind of like a key value. [INAUDIBLE] >> Yeah, right this is basically the read up to, it reads batch size lines and that's what the value is. The value is an away of lines and what I'm doing then is that I'm decoding each of the values using these records and I get back my columns, which is now columns one for each column. And then i am zipping data for the CSV columns. So that each of those is basically, is paired with these corresponding name. >> Now, we basically have features, which is the dictionary, where each CSV column has its own array of values.
2:29
Right? Good. Okay. The second thing is, I now have my input columns. I say I need a layered value, pickuplan, pickuplat, dropofflon, etc and those are my input columns. For now my feature columns are the same as my input columns, but you know what's going to come, right? I'm going to add extra inputs to it. Right now my only inputs are what I read from the file, but later on I'm going to actually change it and actually add extra features that are computered from these inputs, right? Just like we had x and x squared, right? Very similar to that. Then third thing that I did was as before create and train the model, evaluate the model that hasn't changed, fair enough?
3:19
So at this point we've basically learned how to read our CSV file, okay? And let's see, where was that.
3:27
So we've done this.
3:29
So the last thing that we want to do is we want to basically refactor for distribution and monitoring.
3:37
So that's the final refactoring that we're going to do.
3:40
Essentially, what we have so far Is that we're going, getting from a to b, we're training our model, but this is basically why we are getting across, right? Where they hanging on, say let me get you through, and it does. It gets us through but we made a few assumptions here. We run the training operation for number of steps or for number e box when we did the CS we did number of E box. But you gotta realise that number of Vpos is not the right thing to do, okay? We need to wait for some sort of convergence, some sort of all-fitting, some nicer stopping criterium the number of Epos, right? That's not great. The other thing is, as we did the training we saved checkpoints and we basically went ahead and used the last checkpoint as the model
4:31
that's what it did underneath. When we said model that fit, it kept fitting it, it kept saving, and then it got the last one and that was my model and that's the one I'm going to use. But remember over fitting a discussion, it may be that the 70th step is really the best model, not the 100th, right?
4:56
But we just went and picked the last one. So, what we need to do for realistic ways of getting from A to B. Right? Is we want to handle machine failure, number one because we are going to move distributer training. What happens if a machine goes down? We need to retry those jobs. We need to choose the model based on the second data set. Say as you keep training and the error keeps going down, look at the error at this other data set. When that error starts to increase. It is that checkpoint that is going to be remodeled. That's how I want to do early stopping stuff like that. We also want the monitor training, right? We don't want to just hit things, run on data lab and wait for the blue bar, right? We basically want, because this thing is going to take days What we want to do is we want to basically go back and figure out what the machines are doing.. Well at one level we can go to the compute engine VM and say how much CPU are you using but what we really want to know is are you on epoch three or are you on epoch eight?
6:00
We want to monitor the training, figure out what epoch it's on, figure out what the current loss is, stuff like that.
6:08
Right? So we want to look at monitoring. And, finally, it'd be kind of cool if we can resume training.
6:15
Why is that important? >> Data changes. >> Data changes. So maybe we've trained the model. We've now deployed this model, we've got new data. And we say, hey, take that model from where it was and continue doing. >> Whoever it gets, when it comes in completion it's actually [INAUDIBLE]. >> [LAUGH] >> [INAUDIBLE] >> [LAUGH] >> Does it cost to build a realistic
6:43
good model? I mean today is an eye-opener.
6:48
Yes. >> We are talking million dollars? >> No, no we are talking thousand of dollars. >> Tens of dollars?
6:56
>> Thousand of dollars. >> Thousand, okay. >> We thousand, few thousands dollars, right? For reasonably realistic model. >> Okay. >> But yes. It's not cheap.
7:06
It's not cheap. Basically because your data sets are large, and your models get large.
7:13
But you can do a typical tutorial type of things, you will get them down to $30 or so. >> Now, are we going to talk about, I noticed that in a couple of neural net
7:25
>> [INAUDIBLE] You kind of listed an array of the hidden. >> Not, right. >> That or this? >> We will talk about what those mean and how to choose them etc., but in a little about we'll talk about not architectures. Okay? But right now it's just choose some number. >> Just choose some number. Phone was part of the work. Okay. So if we want to do something better we need another class and this is called an experiment. So rather than just use a LinearRegressor directly we basically set up an experiment in TF learn. And the experiment you say, the model I want to use the LinearRegressor Okay, so that's the same model that we wrote before, but then you say my training input function is get train, my evaluation input function is get valid. The idea being that then it's going to train and it's going to stop training when this validation error goes up, it knows all that stuff right? So rather than using those things directly and kind of doing the loop ourself. >> We will use an experiment class, pass in a training function and evaluation function. But then when you do the evaluation function, remember we talked about it, right? The thing that you optimize, and the thing that you evaluate could be different things, right? You may optimize on cross-entropy and evaluate on recall. >> For example, right? So in order to do that, you can also pass an eval_metrics. And so here, I'm basically saying, I want to evaluate rmse, even though I'm optimizing on mean squared error, I'm going to ask it to print out rmse. Root mean squared error. And the way you do this is there is a nice root mean squared error method in tf.learn.metrics, so I'm just using that.
9:19
And then once I have my experiment, I basically say learn_runner.run that experiment function, and here's my output.
9:28
That's basically what you do.
9:31
Okay? This is the final refractor that they do. Now, at this point it no longer fits on a slide.
9:39
I hope that you can follow along with me when we went from the one thing that fitted on a slide. And we did make a couple of refactoring. Wherein some hard coding the data, we read out of a csv file, we read in batches, right? And then we basically spilt the input and the feature, we kind of made them separate. And then finally we wrapped them all into an experiment framework. And that's what we will use going forward, okay? So the other thing that you want to do is whenever you run TF learn is to basically set your verbosity to be info, right? By default, it's at warn, it doesn't show you a bunch of stuff. So if you wanted to show new things, change it to info or change it to debug. Yeah, so that's basically the way you do it and then you will get for every step what the loss is the other thing is to monitor training. There's something called TensorBoard. It's a graphical interface that you get. So you can use TensorBoard and you point it at your out Output directory
10:41
okay so you point TensorBoard at the output directory and that output directory could a local directory or it could be a directory on TCS and TensorBoard can read from both. 
