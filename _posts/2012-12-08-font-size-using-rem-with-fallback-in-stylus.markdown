---
layout: post
title: "font-size using rem with fallback in Stylus"
date: 2012-12-08 21:15:3
category: stylus
author: Yuya Saito
---

TJ Holowaychuk, the creator of [Stylus](http://learnboost.github.com/stylus/) said on the video ["Stylus - the most CSS-like higher level language"](https://vimeo.com/33462524):

> Stylus to be transparent when you what it to be.

Today I attempt to glimpse this "transparent" side of Stylus by making a mixin.

## tl;dr

~~~styl
// Font-size using rem with fallback
// Use px to define font size.
// This will calculate rem value for you.
// I'm assuming base font-size is 10px = 62.5%
font-size(n)
	if unit(n) is 'px'
		font-size: n
		font-size: (remove-unit(n) / 10) rem
	else
		error('Use px as unit')

// Usage
body
	font-size: 16px
// or
body {
	font-size: 16px;
}
~~~

After compile:

~~~css
body {
  font-size: 16px;
  font-size: 1.6rem;
}
~~~

## Explanation

In Stylus, a mixin can be defined like CSS properties and arguments for mixin can be passed like CSS value.  
This is what TJ Holowaychuk means when he said "Stylus can be transparent".

To be honest, I've found about this design by accident when I defined mixin name same as CSS properties.

While I like this "transparent" way, it is very easy to forget which properties are mixins.

`if unit(n) is 'px'` is there because of that.  
If I forget all about it and use unit other than `px`, Stylus will give me an error.

## Failure(s)

When I wrote:  

~~~stylys
// Error!
`if unit(n) is a 'px' 
~~~
as [examples on the document](http://learnboost.github.com/stylus/docs/conditionals.html), Stylus gave me an error.  
I found the solution in [source code on functions in Stylus](https://github.com/LearnBoost/stylus/blob/master/lib/functions/index.styl#L77)(where I also find out about `remove-unit()` function, by the way).

~~~stylys
// Correct!
`if unit(n) is 'px'
~~~

Difference between the two is **a**.  
I still have no idea when I need **a** in conditionals.  
If somebody can tell me, please do([Twitter@cssradar](http://twitter.com/cssradar)).

This reminds me when I have(and still do) problem with articles in English.
