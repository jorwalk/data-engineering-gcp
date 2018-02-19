0:00
Building ML models with TensorFlow. In this module, we will look at what TensorFlow is and how to use TensorFlow for machine learning. And then we will look at how to write distributed TensorFlow models so that we can carry out machine learning training on a number of machines, rather than just on a single machine. So what is TensorFlow?
0:29
TensorFlow at it's heart is So it's a library that lets you
0:41
It's open source, it's now open source and So if you are familiar with Java, that TensorFlow is like Java. It's a way for you to write code that works on a variety of different hardware Graphical Processing Units, or
1:11
You want to be able to run So there's two aspects to There's the aspect of training And then there's the aspect of predicting So you may want to train a machine you may want to run it on a mobile phone. So when you are looking at a machine is portable across a variety of
1:45
So TensorFlow lets you And the way TensorFlow writes the, numerical code is that it represents in this graphs themselves can be executed
2:08
So if you look at the TensorFlow the TensorFlow is written in C++. So core TensorFlow is written in C++. And that's the part that is portable the job of virtual machine. The JVM is typically written in C. But the code that you one day write Java, very similarly the core of portability reasons so that it can run very efficiently on But then the code that you and
2:53
So we will write TensorFlow the Python code that we write will
3:02
Now the Python code at the core of because it is just a numerical So it's all about numerical processing, it's about adding and However the thing that we is that we're writing neural networks. And if we're going to be writing neural level of adding and multiplying matrices it makes sense to At the level of layers, and
3:40
So you can use these higher level But you can go even higher use the estimator API. And that's what we're going we look at using the estimator API to So this is all about TensorFlow, and level if you wanted to at C++, but But those tend to be extremely low level. Or you can use higher level extractions metrics, etc.
4:20
Or you can use something more object oriented in Now this entire thing all of these that you write them, you can run
4:40
So let's first look at the Python API for The Python API for TensorFlow, the core Python API, So if you wanted to add two b) and you get a new tensor, c.
5:02
a and b are tensors, c is a tensor, So what's a tensor?
5:12
One dimension's a vector, two dimension's So whenever you see the word tensor, n-dimensional matrices or But at that point, all you've got something that can hold data. If you want to know what this data is, Feed in actual values for a and So in order to get the value of c, then you would run the session. Saying, go ahead and get me the value of And at that point, a NumPy array of c.
6:04
What exactly do I mean here? What I mean is this, suppose you were working in NumPy, If you had an array, 5, 3, 8, saying np.array 5, 3, 8, and a,
6:26
It actually holds three numbers.
6:30
Similarly, b is a NumPy array that And then when you say np.add (a, b), you get a new array C, And so if your print out the value of c, 8, 5 plus 3 is 8. And 2, and 8 plus 2 is 10, So we get this array of three numbers. So that's the way a NumPy works. NumPy works with arrays
7:03
How about TensorFlow?
7:06
With TensorFlow also, you can take So you can say, hey, I have a one-dimensional tensor And b is another one-dimensional tensor b, and it contains 3, -1, 2. So we now have two tensors a and we can do tf.add(a, b), and And we can print this tensor c, you don't actually get as in the NumPy C is simply a tensor length 3 and its values will be integers.
8:00
The addition has not yet essentially will hold this but right now all that we have done In order to run the graph to get create a TensorFlow session and you would say, You would say, I would like the value for the tensor c and And now this result is a NumPy array and when you print the result But until you evaluate c in
8:53
So you can do this by either saying session.run in parenthesis c. They both do exact same thing. You can tell the session to run c or you can tell c to evaluate itself Regardless, unless you do that, you haven't materialized The addition happens
9:22
Why does TensorFlow work that way?
9:25
The reason it works this way is so that you can separate out the creation of And so you can create the graph, but gets run does not need to be the exact So for example, between say a matrix an add operation, a receive node into the graph. So that the matrix multiplication can the results sent over to another In order for the way TensorFlow works is that it remotely execute them, So even if it's on the same machine, some parts of the graph may execute on on a multi GPU machine, So all of the things that you tend to that you can execute them or So, another way to look at what tensor flow is is to say that you have your hardware CPU's, GPU's, androids, iOS et cetera. You have your operations; add, multiply, print, reshape et cetera. And the execution system that the Python Front-End or the C++ Front-End go through that execution system can itself take the original graphs and add new nodes to it. So as to be able to distribute the competition. 
