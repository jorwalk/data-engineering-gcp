0:00
But let's go back to this previous example of things that looked like that with a lot of blue points in the middle and the orange points off to the side. And let's go back and think about that problem again, but we don't want to add extra layers. We don't want to add neurons. As we talked about a triangle, right, a polygon would help you separate out the blue dots from the orange dots. And the way you can get a polygon is by combining a set of lines and that's essentially the idea that we did when we added extra neurons. But can we do this without adding layers? So let's go back and look at the problem again. But this time, I would like to not have any extra hidden layers. Well, one thing that we can do is that we can bring in our human insight.
0:53
And one of our insights is all the points that are close to the center seem to be blue and all of the points that are far away from the center seem to be orange.
1:05
Well, close and far away, that’s the distance and what's the equation of a distance? Well, Euclidean distance is the square root of x squared and y squared. So there's an x squared term and there's a y squared term. How about we take our inputs x and y and we add in an x squared and add in a y squared? So those are two extra features that I'm adding. And that terminology is important. x1 and x2 are the original inputs, but in addition, x1 squared and x2 squared are features of your calculating from those inputs. So the features are the real input to the neural network model. So you have your input and from your input you compute features. And those features you provide to your neural network model. So we have our features, x1, x2, which are the original inputs, x1 squared and x2 squared. So now let's go ahead and train the model, and boom, it's done, and we have a nice circle that separates out all the blue dots from the orange dots without any extra layers. The thing to realize is that here we brought our human insight into this problem. We knew that x squared and y squared were going to be useful. So this is called feature engineering, where we are creating features that help the machine learning model learn much faster. And this is going to be true even for this spiral case.
2:42
So what we just looked at in the previous demo was that by adding extra features, by engineering extra features, we can make the machine learning problem a lot easier. So in other words, if you're faced with a machine learning problem, you have two approaches. One is to basically make a more complex ML model by adding extra layers. The second is to bring human insight into the problem by adding some extra features. In reality, you will do both, because doing both gives you the benefits of both, more machine learning capability and better human insight that goes into the ML model.
3:25
So in your own words, write down definitions for these ML terms.
3:34
So a neuron is a way of combining its inputs. And so a neuron is essentially a weighted sum of its inputs followed by what is called an activation function. There are different activation functions, things that you can do to that weighted sum. But its base, the simplest neuron, does a weighted sum of its inputs.
4:00
A hidden layer, a layer of neurons, is a combination of neurons,
4:07
all of which share the same set of inputs.
4:13
Really, all the layers share the same set of inputs? Well, take a look.
4:20
All of the neurons in this layer, their inputs are x1 and x2.
4:29
All of the neurons in this layer, their inputs are the eight neurons in this layer, in the first layer. So all the neurons in the second layer, their inputs are the eight neurons in the first layer. And these five neurons in the third layer, their inputs are the eight neurons in the second layer. So a layer, the reason that this neuron is different from this neuron, is that they belong to different layers, and a layer consists of neurons that have the same inputs.
5:06
What are the inputs? These are the things that you feed into a neuron. So the inputs to the first layer of your neural network will be your original inputs, but could also be things that you calculate from those inputs. In other words, that's what we're going to call our features. So we have our inputs, and then we calculate things from those inputs, for example, x squared and y squared in our playground example. And feature engineering is this process of coming up with the features that you're going to calculate from your original inputs. 
