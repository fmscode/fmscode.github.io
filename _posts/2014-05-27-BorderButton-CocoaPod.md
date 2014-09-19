---
layout: post
title: BorderButton CocoaPod
---
This week I released a new CocoaPod called BorderButton for iOS. When iOS 7 launched and I was updating apps, I found that the new iOS 7 style button was the worst change in iOS 7. The new default button of iOS 7 did not have a border, which caused confusion to some users since they did not know where to tap and thought the button was text. BorderButton solves this issue by taking the text color attribute of the button and creating a border around the button with the same color. What is nice about this subclass is that all you have to do is change the class name in Interface Builder and BorderButton takes care of everything for you. For instance, the class will automatically draw a circular border around a button if the width and height are the same.


You can install BorderButton by adding it to your Podfile 'pod BorderButton' or by going to [GitHub][1]. Hopefully you find it as useful as I do.

[1]:https://github.com/fmscode/borderbutton