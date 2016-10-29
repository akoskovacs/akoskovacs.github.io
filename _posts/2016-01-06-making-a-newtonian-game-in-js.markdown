---
layout: post
title:  "Making a Newtonian gravity game in Javascript"
date:   2016-01-06 16:00:00 +0100
tags: javascript game gravity html5 canvas browser
categories: en javascript
lang: en
article-hu: gravitcios-jatek-irasa
---
[Newtonian gravity][newton-gravity] is way cool. So, cool that I will show you how you can make your own HTML5
canvas game simulating gravity. Don't worry it won't be hard. I will write down every step
which is needed for our game.

{% include video.html video="miniksp.webm" width="640" loop="true" %}

<!--more-->
> It should be noted, that HTML5 is not a really new
> technology, but the canvas might still not work everywhere as expected (_blame IE_).

---
{% highlight js %}
function gravitateTo(planet, obj) {
  var r   = planet.pos.distanceTo(obj.pos),
      f   = (planet.mass*obj.mass)/(r*r),
      phi = obj.pos.angleTo(planet.pos);

  obj.gravity.setPolar(phi, f);
  return gravity;
}
---

{% endhighlight %}

## Vector basics
The game that we will write, needs a way to represent forces: velocity, acceleration and etc.
The tool that we need is [vectors][vector].

I won't really get into the vector basics, you must already be familiar with the easy stuff:

+ vector addition (therefore substitution)
* multiplication (and division)
* scaling (multipling a vector with a scalar number)

We will need some more than that, but this is the basics.

-----------

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
[newton-gravity]: https://en.wikipedia.org/wiki/Newton%27s_law_of_universal_gravitation
[vector]: https://en.wikipedia.org/wiki/Vector
