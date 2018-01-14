---
layout: post
title: Self esteem snake
image: "/content/images/2015/12/Screen-Shot-2015-12-24-at-10-46-13-1.png"
date: '2015-12-23 23:22:00'
tags:
- java
- games
alias: /self-esteem-snake
feature-img: http://static.lrnk.co.uk/blog-content/snake-cover.png
---

In August 2013 I embarked on another new game project in my then-favourite language Java. Heartened by the knowledge that everyone loves a good classic game clone, I decide to write my own rendition of [Snake](https://en.wikipedia.org/wiki/Snake_(video_game)).

![lrnk snake](http://static.lrnk.co.uk/blog-content/lrnksnake.gif)

##### Download the game: [SelfEsteemSnake-1.1-beta.jar](https://github.com/laurieboyes/self-esteem-snake/releases/download/v1.1-beta/SelfEsteemSnake-1.1-beta.jar)
*Menu - arrow keys to select, return to confirm*<br/>
*Game - arrow keys to move, p to pause*

##### View the code: [github.com/laurieboyes/self-esteem-snake](https://github.com/laurieboyes/self-esteem-snake)
<span class="paragraph-space-forcer"></span>

As well as giving me one more shiny thing to put on my personal website, this project was contrived to help me learn [Git](https://git-scm.com/) (imagine the mess in a world without Git!) and to take a small chip out of the looming monolith that was my [imposter syndrome](https://en.wikipedia.org/wiki/Impostor_syndrome) by finally giving me a solid, first-hand understanding of working with [Test-driven Development](https://en.wikipedia.org/wiki/Test-driven_development).

I decided that the art style should be entirely text based and had a lot of fun with that, not limited to learning some things about when and how one should bundle fonts in jars. A massive bonus this decision gave me was being able to very clearly log the state of the game to the console when debugging. It was also thanks to this decision that I had the idea for the alternative game mode, **Bookworm**.

![bookworm](http://static.lrnk.co.uk/blog-content/lrnksnake-bookworm.gif)

As you can see, it's basically snake again, but you collect letters of an article. The game downloads a random article from wikipedia and uses that to form the *food string*.

There must have been a reason for me not using the MediaWiki API because I remember looking into it, but I'm quite pleased I didn't because this provided with with my first real exposure to web scraping using [jsoup](http://jsoup.org/).

All sorts of fun.

