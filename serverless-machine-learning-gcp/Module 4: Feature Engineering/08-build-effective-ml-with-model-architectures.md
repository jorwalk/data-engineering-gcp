0:00
So at this point, we've talked about how to do feature engineering, how to create features beyond real valued numbers. The last thing I want to talk about is model architecture. When we go back and look at our ice cream We had a feature that is the price, And it was represented
0:25
Just one, right? The price is represented It's a real valued column, But the employeeId,
0:38
So how many input neurons
0:42
If we have 25 employees in our ice Let's just say it's 25, 25 employees, 25 columns. So we talk of these things
0:59
Because we have to one hat encode And that blows things up, So price is a dense field. It's dense, it's a continuous number. And employeeId is a categorical feature,
1:19
Now, if you have an image So let's say we have an image. Every pixel value is dense. It's a number, it's very dense. R, G, B, and A, so there's four numbers. It's very dense, so subtracting, and doing all the things That's what a neural network is great at. It's at adding and multiplying, and But this is what your sparse feature looks like. In every row, there's a one somewhere, but in general, it's a zero, right? Something like this. Adding them and subtracting them. So if you take two rows here and you add them or you subtract them, it's still pretty much gonna be all zeros. So this is something that's very hard for a neural network to deal with, because there are so many weights that essentially have no impact, because you multiply a weight by zero, it's still zero. So you're basically gonna get stuck in some local minimum, and not be able to get out of it. But this is the kind of thing that a linear model is gonna do very, very, very well, because as we talked about with the taxi color and identifying taxis, it's very easy if you have a sparse thing to just assign high weights to certain things. So, for example, in our ice cream example, you may say, this particular employee, no matter what they do, no matter how long the customers wait, they're very chatty, they're very helpful, they're smiling all the time and the customer is always gonna be happy. So you can basically go ahead and assign weights to sparse features very easily. A linear model works very well for sparse features. So, our neural network works very well for
3:26
But for sparse features, we want to take our input and So we want to have a linear weights to just specific inputs.
3:42
But in reality, of dense features and wide features. Some real valued continuous numbers and So which one do we use? Do we use a linear model Or do we use a deep model because
4:09
Can we have both? Sure, what if you were to take all And say, there are some inputs And those inputs, I will pass through so we can do all of our fine grained And I'll take all my sparse features and
4:42
So can we have our cake and eat it too?
4:46
And that's exactly what a wide and a deep neural net linear combined So wide and deep model essentially says, tell me which ones are sparse, so Tell me which ones are dense, so that I And tell me how many layers and So this is a wide and deep network. So rather than it being black and white, We're going to take our features and decide which ones of them are dense, And then, we're going to create
5:41
So this is essentially a constructor for It asks you for the deep columns and and you just specify them. 
