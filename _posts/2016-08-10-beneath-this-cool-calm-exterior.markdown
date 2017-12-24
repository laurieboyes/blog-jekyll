---
layout: post
title: Beneath this cool, calm exterior
image: "/content/images/2015/12/6545-shot0.png"
date: '2016-08-10 20:36:00'
tags:
- javascript
- games
---

In April 2014, after many opportunities passing by with excuses or genuine poor timing, I finally committed myself to participating in the [Ludum Dare 48-hour game competition](http://ludumdare.com/compo/).

My entry was, uh, pretty weird. The theme, announced at the very start of the 48-hour window late on the Friday evening, was *Beneath the surface*. Many people entered underwater themed games, or mining games. I decided that my game would take a look *beneath the surface* of  my own fragile outward calm.

![gameplay gif](http://static.lrnk.co.uk/blog-content/beneath-this-cool-calm/beneath-this-cool-calm.gif)

#####Play the game: [Beneath this cool, calm exterior](http://static.lrnk.co.uk/ludumdare29/)
*Move: wasd*<br/> 
*Shoot: arrow keys*<br/>
*Start, next level: space bar*

#####View the code: [github.com/laurieboyes/beneath-this-cool-calm-exterior](https://github.com/laurieboyes/beneath-this-cool-calm-exterior)
<span class="paragraph-space-forcer"></span>

#####View the competition entry: [Ludum Dare](http://ludumdare.com/compo/ludum-dare-29/?action=preview&uid=6545)
<span class="paragraph-space-forcer"></span>

Fundamentally, it's a very basic shooter game. The enemies are my insecurities, and some of them spout fun exaggerations on my own internal monologue. When a player collides with an insecurity, they are presented with a picture of my face, moving through an array of increasingly distressed expressions.

Did I win? No! Of course not. But I was pretty happy with my results. Competing against a not insignificant 2495 entries, I came in 14th place for humour (putting me in the top 1 percent!), 98th for theme, and... less good for everything else:

![results](http://static.lrnk.co.uk/blog-content/beneath-this-cool-calm/results.png)

###The making of
The heavy lifting in the code was done by a 2D game library called [melonJS](http://melonjs.org/). It was very nice to work with and I'd use it again.

####Bom bom bom bom

The rules of the competition state that all assets (e.g. graphics and sound) must be created within the 48-hour window. I did all the sound effects on [bfxr](http://www.bfxr.net/), and the music with my own mouth. Sadly, I didn't get around to putting in a mute button for the music, but I think the sense of agitation it evokes after about 8 seconds of listening really contributes to the overall feel.  

####Making it better

My flatmate, true to form, looked up from his own entry to peer over my shoulder and then put me onto a ridiculously good presentation by [Vlambeer](http://www.vlambeer.com/) about improving the 'game feel' (although that's a term they contest).

It came with an interactive demo, which sadly seems to have disappeared, but the talk it accompanied survives and is great: [Jan Willem Nijman - Vlambeer - 'The art of screenshake'](https://www.youtube.com/watch?v=AJdEqssNZ-U).

The talk is filled to bursting with rich wisdom nuggets, but with my short time allowance, I chose to implement a just few of the improvements I thought would be most effective:

* Upping the shoot frequency
* Upping the bullet size
* Adding some screen shake

The astonishing difference these small changes can make is hard to believe without watching that talk but you get some of the effect in these before and after gifs.

Pre tweak:
![before-tweak](http://static.lrnk.co.uk/blog-content/beneath-this-cool-calm/beneath-old.gif)

Post tweak:
![after-tweak](http://static.lrnk.co.uk/blog-content/beneath-this-cool-calm/beneath-new.gif)

In conclusion, this was my first timeboxed game/code jam and I enjoyed it a lot and will absolutely commit doing it or something similar again one of these days.