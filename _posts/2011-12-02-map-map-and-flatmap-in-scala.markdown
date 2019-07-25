---
layout: default
status: publish
published: true
title: Map, map and flatMap in Scala
featured:
  preview: /images/uploads/2011/12/1464056042_169a1b6a14_z-300x199.jpg
  large: /images/uploads/2011/12/1464056042_169a1b6a14_z.jpeg
  caption: Scala (stairs) by Paolo Campioni
  url: [TO BE MIGRATED]
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 1348
wordpress_url: http://www.brunton-spall.co.uk/?p=1348
date: '2011-12-02 10:56:39 +0000'
date_gmt: '2011-12-02 10:56:39 +0000'
categories:
- technical
- featured
tags:
- scala
- learning
comments: true
---
One of the things I like about Scala is it's collections framework. As a non CS graduate I only very lightly covered functional programming at university and I'd never come across it until Scala. One the benefits of Scala is that the functional programming concepts can be introduced slowly to the programmer. One of the first places you'll start to use functional constructs is with the collections framework.

Chances are your first collection will be a list of items and we might want to apply a function to each item in the list in some way.

<!--more-->

Map works by applying a function to each element in the list.

``` scala
scala> val l = List(1,2,3,4,5)

scala> l.map( x => x*2 )
res60: List[Int] = List(2, 4, 6, 8, 10)
```
So there are some occasions where you want to return a sequence or list from the function, for example an Option

``` scala
scala> def f(x: Int) = if (x > 2) Some(x) else None

scala> l.map(x => f(x))
res63: List[Option[Int]] = List(None, None, Some(3), Some(4), Some(5))
```
flatMap works applying a function that returns a sequence for each element in the list, and flattening the results into the original list. This is easier to show than to explain:

``` scala
scala> def g(v:Int) = List(v-1, v, v+1)
g: (v: Int)List[Int]

scala> l.map(x => g(x))
res64: List[List[Int]] = List(List(0, 1, 2), List(1, 2, 3), List(2, 3, 4), List(3, 4, 5), List(4, 5, 6))

scala> l.flatMap(x => g(x))
res65: List[Int] = List(0, 1, 2, 1, 2, 3, 2, 3, 4, 3, 4, 5, 4, 5, 6)
```
This comes in really useful with the built in Option class because an option can be considered a sequence that is either empty or has 1 item.

``` scala
scala> l.map(x => f(x))
res66: List[Option[Int]] = List(None, None, Some(3), Some(4), Some(5))

scala> l.flatMap(x => f(x))
res67: List[Int] = List(3, 4, 5)
```
So with that all covered, lets look at how you can apply those concepts to a Map. Now a map can be implemented a number of different ways, but regardless of how it is implemented it can be thought of as a sequence of Tuples, where a tuple is a pair of items, the key and the value.

``` scala
scala> val m = Map(1 -> 2, 2 -> 4, 3 -> 6)
m: scala.collection.immutable.Map[Int,Int] = Map(1 -> 2, 2 -> 4, 3 -> 6)

scala> m.toList
res69: List[(Int, Int)] = List((1,2), (2,4), (3,6))
```
We can access a tuple by accessing the inner variables _1 and _2

``` scala
scala> val t = (1,2)
t: (Int, Int) = (1,2)

scala> t._1
res70: Int = 1

scala> t._2
res71: Int = 2
```
So we want to think about using map and flatMap on our Map, but because of the way a map works it often doesn't make quite the same sense, we probably don't want to apply a function to the tuple, but to the value side of the tuple, leaving the key as is, so for example we might want to double all the values. Map provides us with a function to do exactly that.

``` scala
scala> m.mapValues(v => v*2)
res73: scala.collection.immutable.Map[Int,Int] = Map(1 -> 4, 2 -> 8, 3 -> 12)

scala> m.mapValues(v => f(v))
res74: scala.collection.immutable.Map[Int,Option[Int]] = Map(1 -> None, 2 -> Some(4), 3 -> Some(6))
```
But in my case I wanted to do something more like flat map in this case, I want a map to come out that misses out the key 1 because it's value is None. flatMap doesn't work on maps like mapValues, it get's passed the tuple and if it returns a List single items you'll get a list back, but if you return a tuple you'll get a Map back.

``` scala
scala> m.flatMap(e => List(e._2))
res85: scala.collection.immutable.Iterable[Int] = List(2, 4, 6)

scala> m.flatMap(e => List(e))
res86: scala.collection.immutable.Map[Int,Int] = Map(1 -> 2, 2 -> 4, 3 -> 6)
```
Ok so we are pretty close to using options with flatMap, we need to filter out our None's, we can do returning a list with just e => f(e._2) and we'll get the list of values without the None's, but that isn't really what I want. What I need to do is return an Option containing a tuple. So here's our updated function:

``` scala
scala> def h(k:Int, v:Int) = if (v > 2) Some(k->v) else None
h: (k: Int, v: Int)Option[(Int, Int)]
```
and here's how we might call it:

``` scala
scala> m.flatMap ( e => h(e._1,e._2) )
res109: scala.collection.immutable.Map[Int,Int] = Map(2 -> 4, 3 -> 6)
```
but this is pretty ugly, all those _1 and _2's make me sad. If only there was a nice way of unapplying the tuple into variables. Given that this works in python and in a number of places in scala I thought this code should work:

``` scala
scala> m.flatMap ( (k,v) => h(k,v) )
:10: error: wrong number of parameters; expected = 1
```
I spent way too long today looking at this (in 5 minute chunks broken by meetings to be fair), before I gave in and asked a coworker what the hell I was missing. The answer is seems is that an unapply is normally only executed in a PartialFunction, which in scala is most easily defined as a case statement. So this is the code that works as expected:

``` scala
scala> m.flatMap { case (k,v) => h(k,v) }
res108: scala.collection.immutable.Map[Int,Int] = Map(2 -> 4, 3 -> 6)
```
Note that we switch to using curly braces, indicating a function block rather than parameters, and the function is a case statement. This means that the function block we pass to flatMap is a partialFunction that is only invoked for items that match the case statement, and in the case statement the unapply method on tuple is called to extract the contents of the tuple into the variables. This form of variable extraction is very common, and you'll see it used a lot.

There is of course another way of writing that code that doesn't use flatMap. Since what we are doing is removing all members of the map that don't match a predicate, this is a use for the filter method:

``` scala
scala> m.filter( e => f(e._2) != None )
res114: scala.collection.immutable.Map[Int,Int] = Map(2 -> 4, 3 -> 6)

scala> m.filter { case (k,v) => f(v) != None }
res115: scala.collection.immutable.Map[Int,Int] = Map(2 -> 4, 3 -> 6)

scala> m.filter { case (k,v) => f(v).isDefined }
res116: scala.collection.immutable.Map[Int,Int] = Map(2 -> 4, 3 -> 6)
```
