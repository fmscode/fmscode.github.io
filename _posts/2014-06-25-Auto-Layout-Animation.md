---
layout: post
title: Auto Layout Animation
---
When switching from springs and struts to Auto Layout in a recent project I found that my animations would occasionally break. For the longest time I always used animateWithDuration:animations: to alter the frame of a view. However, when you want to animate a view controlled by Auto Layout the process is a little different and requires some setup first. 

# Example
Below is the type of animation we are trying to accomplish.


## Springs & Struts
To accomplish this animation simply wrap the frame code inside of an animateWithDuration:animations: block.

{% highlight Objective-C %}

[UIView animateWithDuration:0.5 animations:^{
	_sideView.frame = CGRectMake(-320, 0, 320, 480);
}];

{% endhighlight %}

## Auto Layout
The first step to animating in Auto Layout is to ensure that your view is properly setup with all necessary constraints. Once that is setup you will need to create a reference to the constraint that you will want to animate. In this example you will need to create a reference to the constraint that controls the space between the left side of a view and it's superview. If you are using Interface Builder to setup your constraints, simply make an IBOutlet to the constraint in your View Controller.

{% highlight Objective-C %}

@property (nonatomic) IBOutlet NSLayoutConstraint *leftSide;

{% endhighlight %}

After making the connection you are now ready to animate the view. You will first set the new constant for the constraint, then in your animateWithDuration:animations: block you will call its superviews layoutIfNeeded. It is important to call the proper view that is controlling the layout of the animated view. In this example we would call it on self.view since it is controlling the layout of the constraint.

{% highlight Objective-C %}

_leftSide.constant = -320;

[UIView animateWithDuration:0.5 animations:^{
	[self.view layoutIfNeeded];
}];

{% endhighlight %}

While animating Auto Layout does require more setup, the code is a little more straight forward and cleaner than Springs & Struts code.