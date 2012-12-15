---
layout: post
title: "Vertical Rhythm Boilerplate with Stylus"
date: 2012-12-09 11:18:23
category: stylus
author: Yuya Saito
published: true
---

## tl;dr

**[Demo](/attempt/1/vertical-rhythm.html)**  
*This is **FAR FROM** perfect and it's just a starting point.*

~~~styl
// Settings
// Override this settings via something like site-settings.styl

$rembase = 16 // Most browser's default size
$ratio = 1.625 // Almost golden ratio
$line-height-base = $rembase * $ratio // You don't need to change this
$devmode = false // Set this true to show vertical grid

// mixins

// It calculates line-height when flag is true
// font-size must be declared with px
font-size(n, flag = true)
	if unit(n) is 'px'
		font-size: n
		font-size: (remove-unit(n) / $rembase) rem
		if flag
			line-height: -getlineheight()
	else
		error('Use px as unit')

// margin-bottom using rem with fallback
line($n)
	margin: 0 0 ($n * ($line-height-base px)) 0
	margin: 0 0 ($n * ($line-height-base / $rembase) rem) 0

// This is a function and by convention, starts with hyphen
-getlineheight()
	return remove-unit(@font-size) * ($line-height-base / (remove-unit(@font-size) * $rembase))

// Vertical guide using gradient
guide()
	unless $devmode is false
		background: -moz-linear-gradient(top, #BA9B9A 1px, transparent 1px );
		background-image: -webkit-linear-gradient(#BA9B9A 1px, transparent 1px);
		background-size: 100% ($line-height-base px);


//
html {
	font-size: 16px; // Assuming 16px...
	guide()

p,
ul,
ol,
dl,
blockquote,
pre,
td,
th,
label,
textarea,
caption {
  line: 1
}
h1, .h1, h2, .h2, h3, .h3, h4, .h4, h5, .h5, h6, .h6 {
	margin-top: 0;
	margin-bottom: 0;
}
h1,
.h1 {
	font-size: 64px;
	line: 1;
}
h2,
.h2 {
	font-size: 48px;
	line: 1;
}
h3,
.h3 {
	font-size: 32px;
	line: 1;
}
h4,
.h4 {
	font-size: 28px;
	line: 1.25;
}
h5,
.h5 {
	font-size: 24px;
	line: 0.5;
}
h6,
.h6 {
	font-size: 16px;
	line: 1;
}
~~~

## Explanation

My math sucks.  
Therefore most of the calcurations were based on [Web Typography Using The Golden Ratio and REM’s](http://gregrickaby.com/2012/10/web-typography-using-rem.html).
It is straightforward to see what's going on.

In last [article](/stylus/2012/12/08/font-size-using-rem-with-fallback-in-stylus/), I demonstrated "transparent" design of Stylus.  
I took that idea further by introducing `line` property. As you can see from
above snippet, `line` mixin can be used like CSS property.  
In my opinion, this mixin **should be** used as mixin.(ex: line(n))  
This "transparent" design of Stylus can be helpful when expanding CSS
properties. However, it is not ideal to use when drastically change CSS syntax.

For example, `font-size` can use `rem` unit, but due to browsers which doesn't
happened to support this unit, you almost always like to have fallback with px unit.  
By using `font-size` mixin, this problem can be solved without thinking about
it.  

When it's safe to say what you trying to archive should be default behaviour of
CSS, then it's ok to use this transparent way to declare mixins.  
When you're trying to re-using css properties, then use "mixins" way to declare it.

## Failure(s)

The hardest part of this attempt was clearly a math.  
The second hardest part was find out how to concatenate unit to variables where multiple values can be used.

`margin: 0 0 ($n * ($line-height-base px)) 0`

Since Stylus seems like it can't use `+` to concatenate unit to value, you need to wrap with parentheses.  
Above example is somewhat complex looking but 

`margin: 0 0 $line-height-base px 0`

this doesn't work.  
Say $line-height-base was 26, when you compile this to CSS, you will get

`margin: 0 0 26 px 0`

Yes, the space is not a typo.  

Then I tried:

`margin: 0 0 ($line-height-base)px 0`

it does work when value is single, but this doesn't work.  
So I end up with this:

`margin: 0 0 ($line-height-base px) 0`

It seems parentheses have two purpose in Stylus:

1. As you expected, change math operation order
2. Protect variables from other values

Oh, and `guide()` mixin is very handy I think.  
I came up with this because [Basehold.it](http://basehold.it/) was down when I was trying to use it.　　

Sometimes, you need to re-invent stuff, because it simply not available when you need it.

## Appendix

### Tools

- [Vertical Rhythm CSS rules for Web Design](http://soqr.fr/vertical-rhythm/)
- [Pearsonified’s Golden Ratio Typography Calculator](http://www.pearsonified.com/typography/)
- [Gridlover](http://www.gridlover.net/)

### Codes

- [jonschlinkert/vertical-rhythm · GitHub](https://github.com/jonschlinkert/vertical-rhythm)
- [A Sass/SCSS vertical rhythm mixin · CodePen](http://codepen.io/sturobson/pen/jFKlJ)

### Articles

- [24 ways: Composing the New Canon: Music, Harmony, Proportion](http://24ways.org/2011/composing-the-new-canon/)
- [A List Apart: Articles: More Meaningful Typography](http://www.alistapart.com/articles/more-meaningful-typography/)
- [How to Tune Typography Based on Characters Per Line](http://www.pearsonified.com/2012/01/characters-per-line.php)
- [R(a\|ela)tional Design](http://blog.8thlight.com/billy-whited/2011/10/28/r-a-ela-tional-design.html)
- [Web Typography Using The Golden Ratio and REM’s](http://gregrickaby.com/2012/10/web-typography-using-rem.html)
