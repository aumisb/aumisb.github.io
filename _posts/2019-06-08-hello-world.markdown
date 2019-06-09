---
layout: post
title:  "Hello World"
date:   2019-06-08 21:00:00 -0500
categories: [jekyll] [blog]
---
This simply how to implement the module of permutations in python.

{% highlight ruby %}
>>> from itertools import permutations
>>> perms = [''.join(p)+"@gmail.com" for p in permutations('abc', 3)]
>>> for x in range(0, len(perms)):
...     print (perms[x])
... 
abc@gmail.com
acb@gmail.com
bac@gmail.com
bca@gmail.com
cab@gmail.com
cba@gmail.com
>>> 
{% endhighlight %}