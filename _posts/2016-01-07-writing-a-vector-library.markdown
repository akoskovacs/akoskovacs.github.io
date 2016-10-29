---
layout: post
title:  "Writing an easy-to-use vector library"
date:   2016-01-06 16:00:00 +0100
tags: javascript game vectors vectors tutorial
categories: en
---
In this tutorial I will show you, how easy it is to write a small, but useful
[vector][vector] library in [Javascript][js]. The purpose is
not to write a library that covers every little aspect or
optimized to the extremes, but something which could be
the base of a small game for example.

{% highlight js %}
// Making a vector object (x = 10, y = 20) by it's coordinates
var v1 = new Vector(10, 20);    // (no new as you can see)
// A vector with an angle of 90° and a length of 10
var v2 = Vector.createPolar(Math.PI/2, 10);

// Using the two will look like this:
var v3 = v1.add(v2);      // Adding the two together
v3.scale(v1.getLength()); // Scale the length of v3, with the length of v1
// and so, on...
{% endhighlight %}
<!--more-->

## Vector basics

We will limit this article about vectors in the **mathematical sense**, to the so called
**Eucledian vectors**, instead of the one dimensional arrays from computer science. You
have to be already familiar with that if you want to understand the code.

I also have to mention, that I am not an expert in vectors and I will only cover
relevant theory (as little as needed), excluding the nasty stuff.

### What are vectors?
> In mathematics, physics, and engineering, a Euclidean vector (sometimes called a geometric[1] or spatial vector,[2] or—as here—simply a vector) is a geometric object that has magnitude (or length) and direction. Vectors can be added to other vectors according to vector algebra.
> -- <cite>from [Wikipedia][evector]</cite>

So basically vector is an _"arrow"_ represented by two numbers in a coordinate system.
The point where the arrow originates is the coordinate system's **origin**, the $\left( 0 ,\, 0 \right)$.
When doing mathematical operations with vectors, it is really important to remember this. We will limit our interest
to only two dimensions.

Forgetting that these are not just _points_, but instead _directed arrows_ can yield to errors.
**Vectors can be** used as a container for points in the screen for example, but then performing vector operations on
random points on the screen makes no sense. _But, we will also them anyway._

The vector $ \overrightarrow{OA} = \left( 2 ,\, 3 \right) $ can be represented with this plot:
![Position Vector](/public/Position_vector.svg)

The arrow starts at the origin and points to the point A, at $\left( 2 ,\, 3 \right)$, so $x = 2, y = 3$.

### Why vectors?

In the next article, I will show you a space game, where some basic physics concepts, like position,
velocity (speed), acceleration and even gravity is implemented. And because velocity for example has
a **direction** _(where we go)_ and a **length** _(the speed itself)_. It is evident that these could be implemented with vectors. Euclidean vectors are all over physics, and if you want to write a game you have to understand them.

### Representation

Vectors are also great because they have multiple representations. I already showed the first one above. You assign
the distance from the origin in the $x$ and the $y$ axis and there you go. But sometimes, it would be better
_(like with velocity above)_ to have a direction and a length. So, instead of giving the two coordinates, we assign
an angle $ \varphi $ (from the $x$ axis) and the length. For the same $\overrightarrow{OA}$ vector we can plot it like this:

![Position Vector](/public/Position_vector2.svg)

Nothing changed, only how we imagine the same thing. The angle is $\varphi = 56.6° $, the length of our vector is
$| \overrightarrow{OA} | = 3.6$ units.

I will explain how you can calculate the values later.  

### Skeleton code

Now, lets just write with some code already! We will start with a skeleton. I don't go to details on Javascript.

``` js
var Vector = (function() {
  /* this is where our stuff will go... */
})();
```
This bizarre piece of code is actually a very common idiom in Js. This is all because, unfortunately Javascript doesn't do a very good job in classes. It has non. Instead, it uses several patterns to mimic classes.

And not just classes, but no constructors, inheritance, methods, static functions and so on. But it is still very easy.

To create a namespace, for instance we create an unnamed function and after it's definition we instantly call it. The result of this function will be our *"class"*.

``` js
var Vector = (function() {
    /* Our contructor initializes x and y */
    function Vector(x, y) {
      /* If x or y are undefined, make them 0 */
      this.x = x || 0;
      this.y = y || 0;
      return this;
    }
    return Vector; /* giving back the constructor */
})();
```

To create a constructor we make a new named function *(the name doesn't matter)* in the anonymous one and returning it to the caller. The constructor will initialize everything as needed, even if the parameter is undefined *(hence the or-ing)*.

``` js
var Vector = (function() {
  // Normal constructor usage: var v = new Vector(coord1, coord2);
  function Vector(x, y) {

    this.x = x || 0;
    this.y = y || 0;
    return this;
  }

  // Static constructor usage: var v = Vector.createPolar(ang, len);
  Vector.createPolar = function(phi, len) {
    var v = new Vector();
    /* Convert to x, y */
    return v;
  }

  // Regular method, usage: v.toString();
  Vector.prototype.toString = function() {
    return '(' + this.x + ', ' + this.y + ')';
  }

  return Vector;
})();
```

### Operations

Having, the $x$ and $y$ instance variables set, now we can implement all operations that we need.
All basic operations follow the same principle: just do the operation with each
Let's define our vectors $\overrightarrow{v}$ and $\overrightarrow{u}$ as:

$\overrightarrow{v} = \left( x ,\, y \right)$

$\overrightarrow{u} = \left( z ,\, w \right)$

#### The four basic operations
* $ \overrightarrow{v} + \overrightarrow{u} = \left( x + z ,\, y + w \right) $
* $ \overrightarrow{v} - \overrightarrow{u} = \left( x - z ,\, y - w \right) $
* $ \overrightarrow{v} * \overrightarrow{u} = \left( x * z ,\, y * w \right) $
* $ \dfrac{\overrightarrow{v}}{\overrightarrow{u}} = \left( \dfrac{x}{z} ,\, \dfrac{y}{w} \right) $

The code is also quite, easy:

``` js
var Vector = (function() {
    // the stuff we already wrote...
    Vector.prototype.add = function(v) {
      return new Vector(this.x + v.x, this.y + v.y);
    }

    Vector.prototype.sub = function(v) {
      return new Vector(this.x - v.x, this.y - v.y);
    }

    Vector.prototype.multiply = function(v) {
      return new Vector(this.x * v.x, this.y * v.y);
    }

    Vector.prototype.divide = function(v) { // et impera
      return new Vector(this.x / v.x, this.y / v.y);
    }
})();
```
This is great, because we can test our library:

``` js
var v = new Vector(2, 3);
var u = new Vector(10, 5);
// v + u = (2+10, 3+5) => output (12, 8)
console.log(v.add(u));
// u + v = (10-2, 5-3) => output (8, 2)
console.log(u.sub(v));
// and so on...
```
There is only one problem. If you look closely, all function have to create a new `Vector` object.
This means `v` and `u` will never change. Sometimes it's good sometimes it does not. We call these
kind of functions **immutable**. These does not have any **side effects**, _they don't modify their object._



Like in most cases substraction is basically the same as addition.

We will approach vectors from two different _angles_. This will
give two different kind of representation for the same thing.
They are interchangeable, but one can be more

1. Distances from the origin in **different** axis (x, y, ...)
2. Distance from the origin and the **angle** from one of the axis

The first one is the most obvious. In human language it means,
that you just decide how many

I won't really get into the vector basics, you must already be familiar with the easy stuff:

+ vector addition (therefore substitution )
* multiplication (and division)
* scaling (multipling a vector with a scalar number)

We will need some more than that, but this is the basics.


Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
[newton-gravity]: https://en.wikipedia.org/wiki/Newton%27s_law_of_universal_gravitation
[vector]: https://en.wikipedia.org/wiki/Vector
[evector]: https://en.wikipedia.org/wiki/Euclidean_vector
[js]: https://en.wikipedia.org/wiki/Javascript
[html5]: https://en.wikipedia.org/wiki/HTML5
