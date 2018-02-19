0:00
So third key aspect of a good feature is that all your features have to be in numeric and they have to have a meaningful magnitude. Why? Because a neural network is simply an adding, multiplying, weighing machine. It's just carrying out arithmetic operations, computing trigonometric functions, algebraic functions on your input variables. So your inputs better be numbers and those magnitudes better have some meaning so that a number two is, you know, in some sense twice number one. So what do we mean by this?
0:54
So let's take a quiz again.
0:58
We're trying to predict the number of coupons that are going to be used, and we're looking at different features of that discount coupon.
1:08
So the percent value of the discount, for example, 10% off, 20% off, etc, is this numeric? Yes, meaningful magnitude is a 20% coupon worth twice a 10% off coupon, of course. So absolutely not a problem, the percent value of the discount is a numeric input.
1:38
The size of the coupon, suppose I define it as 4 square kilometers, 24 square kilometers, 48 squared, not kilometers, centimeters squared, so 4 centimeters squared, 24 centimeters squared, 48 centimeters squared, etc, is this numeric?
1:58
Yes, so six times more visible than one that's 4 square centimeters? Sure, that makes sense, so you could imagine that this is numeric. But also it's iffy whether the magnitude are actually meaningful. So if this were an ad that you're placing, the size of a banner ad, larger ads are better and you could argue that that makes sense. But if it's a physical coupon, if it's something that goes out on paper, then you have to wonder whether a 48 square centimeter coupon is twice as good as a 24 square centimeter coupon. But let's change this problem a little bit, suppose we define a size of coupon as small, medium and large.
2:52
At that point, small, medium or large, are they numeric?
2:58
Not at all.
3:00
Look, I'm not saying that you cannot have categorical variables as inputs to numeric networks. You can, it's just that you can't just use small, medium or large directly, you have to do something smart to them. We will look at this in a little bit. So you just have to find a way to represent them in numeric form. We will look at how to do that, shortly. But what I'm saying here is that you can not take something that's categorical, small, medium, large, and use them directly. Let's take the third, font an advertisement is in, Arial 18, Times New Roman 24, etc, is this numeric?
3:41
No, how do you convert Times New Roman to numeric?
3:47
Well, you could say Arial is number one, Times New Roman is number two, Roborto is number three, and Comic Sans is number four, etc. That's a number code, they don't have meaningful magnitudes. If we set Arial as one and Times New Roman as two, Times New Roman is not twice as good as Arial.
4:11
So the meaningful magnitude part is important.
4:17
Color of the coupon, red, black, blue, etc, again, no, these are not numeric. They don't have meaningful magnitudes.
4:29
Again, we can come up with a number like RGB values to make colors numbers, but they're not going to be meaningful numerically.
4:44
If I subtract two colors and the difference between them is three, does that mean if I subtract two other colors and the difference between them is also three, are these too commensurated?
5:00
No, and that's a problem.
5:03
That is a problem.
5:07
Item category, 1 for diary, 2 for deli, 3 for canned goods, etc, no, these are categorical, it's not numeric.
5:18
So I'm not saying that you can't use non-numerical values, I'm just saying that you need to do something to them, and we look at what things we need to do to them. So as an example, suppose you have words in a natural language processing system, the things that you do to the words to make them numeric is that you run, typically, something called Word2vec.
5:45
That's a very standard technique, and you basically take your words and apply this technique to make those words vectors, so each word becomes a vector. And at the end of Word2vec, when you look at these vectors, these vectors are such that if you take the vector from man and you take the vector from woman and you subtract them, the difference that you get, it's going to be the similar difference to if you take the vector for king and you take the vector for queen and you subtract them.
6:29
That's what Word2vec does, so changing an input variable that's not numeric to be numeric is not a simple matter, its a lot of work.
6:40
Well, you could just go ahead and throw some random encoding in there, but you can. But your ML model is not going to be as good as if you had started with a vector encoding that's nice enough that it understands the context of male and female, man and woman, king and queen. So that's what we're talking about when we say you need to have numeric features, they need to have meaningful magnitudes. They need to be useful, you need to be able to do arithmetic on them,
7:11
you need to find vector representations in such a way that these kinds of qualities exist.
7:21
One way that we can do this is to find these things automatically using processes called auto-encoding, embedding, etc. But often times, for example, if you're doing natural language processing, Word2vec already exists and you have dictionaries already available. And more commonly, that's what you will use, you will just go ahead and use one of these dictionaries to take your text and convert them into vectors, and go off and use them. You won't to have to actually built the mapping of something that's not numeric to something that's numeric, but in cases where the vocabulary is your own jargon, is your industry jargon, you may have to do it yourself. 
