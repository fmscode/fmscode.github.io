---
layout: post
title: Auto Layout In Code
---
Recently I had to learn how to setup Auto Layout constraints within code. The usual method of setting up Auto Layout is through a storyboard with Interface Builder. Apple has [documentation][1] on how to write constraints in code, but I thought they fell short on providing real world examples. I still highly recommend reading the documentation since it does describe what the syntax is for writing these constraints in more detail than this post will.


## Setup
There are two very important steps to do before writing constraints.

- Set "translatesAutoresizingMaskIntoConstraints" to false on the view you plan on applying constraints to. If you do not set this property on the view, nothing will work properly due to conflicting constraints.
- Add the view to your superview before applying the constraints.

## Stretch to fill screen
The first example is applying constraints to a table view. When you turn your device, and you support landscape orientation, the expected behavior is for the table view to expand and fill the entire screen. To do this within code, setup two constraints on the superview for the horizontal and vertical alignment. First the horizontal code:

{% highlight Objective-C %}

NSArray *horizontalTable = [NSLayoutConstraint 
	constraintsWithVisualFormat:@"H:|-(0)-[table]-(0)-|" 
	options:NSLayoutFormatAlignmentMask 
	metrics:nil 
	views:@{@"table": self.tableView}];
	
{% endhighlight %}

The VisualFormat parameter is what defines the rule. The basic format for a constraint is "Orientation:Secondary View-(Constant)-[Primary View]-(Constant)-Secondary View".

  - **Orientation**: Either "H" or "V" for horizontal and vertical.
  - **Secondary View**: The view to the left and right for horizontal constraints, or directly above and below for vertical constraints.
  - **Constant**: The actual value for the constraint to follow.
  - **Primary View**: The view we want the constraint to apply to.
  
This format string is essentially saying to keep 0 pixels between the table and the superview on the left and right side.

With the hard part over, there are just a few more parameters.

- The options parameter should be set to "NSLayoutFormatAlignmentMask", since we are defining both left and right constraints. 
- For this example, metrics does not need to be set.
- The last important part of the constraint is the "views" dictionary. This is where you link your format string to actual views. In the format string we declared "table" as a view, so we need to link that to an actual view in our view hierarchy. To create this link we pass a dictionary with a key "table" then pass our actual table as its value.

Once you have setup the horizontal constraint, the vertical constraint is very similar. The main difference is the use of "V" instead of "H".

{% highlight Objective-C %}

NSArray *verticalTable = [NSLayoutConstraint 
	constraintsWithVisualFormat:@"V:|-(0)-[table]-(0)-|" 	options:NSLayoutFormatAlignmentMask 
	metrics:nil 
	views:@{@"table": self.tableView}];

{% endhighlight %}

To apply these constraints we add them to the superview since it is taking care of the layout in this example.

{% highlight Objective-C %}

[self.view addConstraints:horizontalTable];
[self.view addConstraints:verticalTable];

{% endhighlight %}

If you run your project and rotate your device, you will see that the table will stretch to fill the screen as expected.


## Bottom Toolbar
When you have a toolbar within a view it usually belongs fixed to the bottom of the view.

The first constraint will be for the horizontal alignment, which will look similar to the table view horizontal constraint.

{% highlight Objective-C %}

NSArray *horizontalToolbar = [NSLayoutConstraint 
	constraintsWithVisualFormat:@"H:|-(0)-[toolbar]-(0)-|" 
	options:NSLayoutFormatAlignmentMask 
	metrics:nil 
	views:@{@"toolbar": actionsBar}];

{% endhighlight %}

The vertical alignment is a little bit different since we want the view to be pinned to the bottom of the view.

{% highlight Objective-C %}

NSArray *verticalToolbar = [NSLayoutConstraint 
	constraintsWithVisualFormat:@"V:[toolbar]-(0)-|" 
	options:NSLayoutFormatAlignAllBottom 
	metrics:nil 
	views:@{@"toolbar": actionsBar}];

{% endhighlight %}

The difference here is that we only have the superview pipe character as the last item. This is because we want the view pinned to the bottom of its superview.

Now add the constraints to the superview and run the app. You should have a toolbar that is pinned to the bottom of the view.

## Conclusion
While working with Auto Layout in code is not as easy compared to creating them in Interface Builder, it is still very useful to know in case you ever want to create a view in code rather than in a storyboard or nib.




[1]: https://developer.apple.com/library/mac/releasenotes/UserExperience/RNAutomaticLayout/index.html#//apple_ref/doc/uid/TP40010631-CH1-SW3