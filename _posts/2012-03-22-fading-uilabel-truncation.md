---
layout: post
title: "Fading UILabel Truncation"
category: Development
tags: [ios, development, tutorial]
---
{% include JB/setup %}

In an app I'm currently writing I wanted to make a UILabel fade out instead of just clipping or truncating. After a bit of tinkering this is the solution I came up with. It uses a `CAGradientLayer` to mask the end of the label with a gradient.

Here is the code, hope you find it useful!

{% highlight objc %}
    - (void)awakeFromNib
    {
        maskLayer = [CAGradientLayer layer];
        UIColor *outerColor = [UIColor colorWithWhite:1.0f alpha:1.0f];
        UIColor *innerColor = [UIColor colorWithWhite:1.0f alpha:.0f];
    
        maskLayer.startPoint = CGPointMake(.0f, .5f);
        maskLayer.endPoint = CGPointMake(1.0f, .5f);
        maskLayer.colors = [NSArray arrayWithObjects:(id)[outerColor CGColor],
                            (id)[outerColor CGColor],
                            (id)[innerColor CGColor], nil];
        maskLayer.locations = [NSArray arrayWithObjects:[NSNumber numberWithFloat:.0f],
                               [NSNumber numberWithFloat:.8f],
                               [NSNumber numberWithFloat:.99f], nil];
        maskLayer.frame = CGRectMake(0, 0, self.frame.size.width, self.frame.size.height);
        self.layer.mask = maskLayer;
    }
{% endhighlight %}

I'm doing this in a UILabel subclass that I assign my labels in _Interface Builder_, but could be done from the `UILabel`'s owener as well.

maskLayer is a `CAGradientLayer` that I apply as a mask on the label. Making long lines fade out.

If there are better ways of doing this, please tell me on [twitter](http://twitter.com/simmelj)!