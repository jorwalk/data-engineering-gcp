# Welcome to Serverless Machine Learning on Google Cloud Platform
#### Welcome to the Course
#### Data Engineering on Google Cloud Platform Training
Looking at how to build data handling capabilities on Google Cloud platform.

So who's a data engineer? A data engineer is someone who enables decision making. Typically, they do this by building data pipelines, by ingesting data, processing data, building tools to analyze data, building dashboards, building machine learning models. So a data engineer's job is to enable decision-making within the company in a very systematic way. And in order to be a good data engineer, you needed to know, both programming and statistics to a great deal of depth. But with the advent of Cloud services, particularly the fully managed auto-scaling services on Google Cloud, the amount of infrastructure that you need to know, the amount of programming that you need to know, has gotten a lot simpler. At the same time, the statistics realm has also gotten a lot simpler. You now have libraries that take care of a lot of the low level programming that you have to do. And a lot of the mathematical concepts that you have to know, to the extent that you can now program with data. You can build statistical machine learning models a lot simpler when you're building using these libraries and packages. So what is happened is that, over time, the amount of programming that you needed to know has gotten simplified. So what you're going to be doing in these sets of courses is that we're going to look at how to build data pipelines, data processing systems on Google Cloud Platform.

#### How to Think About Machine Learning
The way you work with machine learning is that you start with a very general purpose function, like a neural network, and you find weights on that function. These are configurable parameters, these are tunable parameters. So you tune those parameters of the function based on a known dataset such that it can very well approximate that dataset. And then you turn around, and you take this function that you have tuned on this known dataset and you use to predict unknown values.

So that's essentially what machine learning is gonna be about. And if you want to do machine learning, well, it turns out that you need to approximate this function on a large amount of data, and you need to do this at scale. And that's something that Google Cloud does very well. So what do you learn in this course is how to do machine learning on Google Cloud platform using TensorFlow, using Cloud ML. But you will also realize that with Cloud ML, you get serverless machine learning, which essentially means that the amount of configuration and work that you need to do is very much reduced. So you don't have to basically manage a cluster of machines, and you basically write TensorFlow code and you submit it to the cloud, and Cloud ML essentially takes care of the whole of running it for you.

So, in this course, what you're gonna do is that we'll introduce you to the basics of machine learning, and then we'll delve into how you can apply machine learning to solve a particular problem. And the example of the problem that we're gonna take is that of estimating the amount of fare that you're gonna pay if a taxicab picks you up at this location and drops you off there, how much are you gonna pay in taxi fare? And this is gonna depend on the route that you take, and these are gonna be taxes in New York. So depending on the bridge that you take, depending on the time it's going to take you, you're gonna pay a certain amount. And the idea is, given the time of day, given the pickup and drop-off location, we want to basically estimate how much it's going to cost you. Now, you could do this with rules. The other way to do it has to do with machine learning. And by using machine learning, we will, in effect, have our ML model, based on a billion taxicab rides, learn the map of New York. 

## Module 1: Getting Started with Machine Learning
Identify use cases for machine learning
#### Getting Started with Machine Learning Introduction
#### What is Machine Learning (ML)?
#### Playing with ML
#### Effective Machine Learning
#### Creating Machine Learning Datasets
#### Lab: Create Machine Learning Datasets

## Module 2: Building ML models with Tensorflow
Build an ML model using TensorFlow
#### Building ML Models with TensorFlow Introduction
#### Getting Started with TensorFlow
#### Lab: Getting Started with TensorFlow
#### TensorFlow for Machine Learning
#### Lab: Machine Learning using tf.learn
#### Gaining More Flexibility
#### Lab: TensorFlow on Big Data
#### The Experiment Framework
#### TensorFlow Resources

## Module 3: Scaling ML models with Cloud ML Engine
Build scalable, deployable ML models using Cloud ML
#### Scaling ML Models with Cloud ML Engine Introduction
#### Why Cloud ML Engine?
#### Packing up a TensorFlow Model
#### Lab: Scaling with Cloud ML Engine

## Module 4: Feature Engineering
Express the importance of preprocessing and combining features

Apply advanced ML concepts into their models

#### Feature Engineering Introduction
#### Improving ML Through Feature Engineering
#### Lab: Feature Engineering
#### Hyperparameter Tuning + Demo
#### Review and Resources
