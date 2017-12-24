---
layout: post
title: 'The Christmas fam scarf | Part three: ScarfParty - your solution to the pixel
  pattern problem'
image: "/content/images/2016/02/scarfparty2.png"
date: '2016-07-18 22:50:00'
tags:
- knitting
- javascript
- the-christmas-fam-scarf
---

Welcome to the eagerly-anticipated concluding chapter of the tale of the Christmas fam scarf! The series explores in excruciating detail the creation of the scarf I lovingly crafted for my brother and his girlfriend for Christmas 2015.

This third and final post showcases the app I made to solve a crucial problem in the completion of the scarf.

If you're unsure of what a _pixel pattern_ is, see [Part one: The intro and the pattern](http://blog.lrnk.co.uk/the-christmas-fam-scarf-part-1).

If you're pretty much on-board with the concept of knitting your own complex, non-repeating scarf design but aren't sure what problem I'm trying to solve here, see [Part two: The actual knitting](http://blog.lrnk.co.uk/the-christmas-fam-scarf-part-2). The general gist is that once these pixel patterns reach a certain level of complexity they do become very hard to follow, which is exactly why I made this:

![scarf party demo](http://static.lrnk.co.uk/blog-content/christmas-fam-scarf/scarf-party-demo.gif)

#####Use the app for your own pixel pattern: [ScarfParty](http://static.lrnk.co.uk/scarfparty2/)
<span class="paragraph-space-forcer"></span>

#####Check out the code: [github.com/laurieboyes/scarfparty-v2](https://github.com/laurieboyes/scarfparty-v2)
<span class="paragraph-space-forcer"></span>

The grid is a dynamic display of the pattern that updates along with your progress. Each square in the grid represents a single stitch. Regardless of your position in the scarf or what side you're on, the pattern is always read from right to left, and the stitches are always displayed in the colours you need to knit. This is achieved by flipping the colours and design at the end of each row, presenting the pattern similarly to in the demonstration at the end of [part 2 of this series](http://blog.lrnk.co.uk/the-christmas-fam-scarf-part-2).

This leaves the knitter with ample mental bandwidth remaining to siphon off into conversation or podcast consumption, and frees them from the anguish of constantly feeling the need to recount theirs rows and stitches. Phew!

###Special features

The pattern is generated directly from a small black-and-white image, just like the one we end up with as a result of following through the pattern creation guide in [part 1](http://blog.lrnk.co.uk/the-christmas-fam-scarf-part-1). This means we can swap in new versions of our pattern effortlessly (for the bits we haven't already knitted!) as we work out the finer points of our project. Beat that, graph paper.

A number of special features are baked in:

* **Fully-reponsive mobile-first design**

<span style="padding-left:2.5em; display:block">Knit in the pub! (Provided you can overcome the burning self-consciousness I find usually comes with knitting in public).</span>

* **Save your progress**

<span style="padding-left:2.5em; display:block">Pattern and progress info is automatically saved to your device's HTML5 local storage, allowing everything to be kept up to date and running smoothly without requiring a constant internet connection (Knit on the train!).</span>

* **Custom colours**

<span style="padding-left:2.5em; display:block">Change the colour display to match your own pattern.</span>

* **Custom stitch increment**

<span style="padding-left:2.5em; display:block">Make your own choice about how often you want to let ScarfParty know you've done a chunk of stitches.</span>

* **Stitch markers (experimental)**

<span style="padding-left:2.5em; display:block">Knitting something bigger, like a blanket? Put stitch markers in the pattern to make it easier to keep track of where you are in a single row.</span>


ScarfParty is written in ES2015, and makes use of [Origami Build Tools](https://github.com/Financial-Times/origami-build-tools) and the [polyfill service](https://cdn.polyfill.io/), both of which are developed and maintained by the good people at the Financial Times.

There were a lot of fun problems to solve here, some of which will bubble up as their own little blog posts. 

This was easily my favourite knitting project to date and ranks pretty highly on my list of favourite side projects. I'd be genuinely astounded and amazed if anyone else were to pick up this blog series and have a go at creating their own pixel-pattern project, but if anyone does, please tweet me!  