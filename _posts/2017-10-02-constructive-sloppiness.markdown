---
layout: post
title: Constructive sloppiness
image: "/content/images/2017/09/lol.svg"
date: '2017-10-02 06:51:34'
excerpt: <p>For hackathons and things, insecure database solutions can save you a big wodge of time and Chrome extensions are a very versatile tool.</p>
tags:
- javascript
- web
- the-ft
alias: /constructive-sloppiness
---

***Tl;dr*** *For hackathons and things, insecure database solutions can save you a big wodge of time and Chrome extensions are a very versatile tool.*

The other week, I took part in the FT’s annual internal hackathon. My team and I decided to play with our idea of bringing video-game-style ‘achievements’ into FT.com, with the aim of encouraging exploration and discovery of the site. It was great fun! Check it out:

![poliwag screenshot](http://static.lrnk.co.uk/blog-content/lazy-hackathon/poliwag-screenshot-min.png)

I got to try out some cringe-inducingly sloppy but highly effective techniques for quickly building rich prototypes. I’m not sure I should really be proud of them but I am.

### Using an online JSON store as a database

I reckon this saved us bags of time. We needed somewhere to store the info that would inform our users’ achievement progress. It needed to be publicly accessible so other folks could see our ‘profiles’, and ideally it needed to be easy to fiddle so we could throw in some dummy data for a smooth demo.

And along came [JSON Blob](https://jsonblob.com/). It may be designed for mocking backends for testing and development purposes, but here’s all the great benefits you get from using it as a database:

 - Simple HTTP API.
 - HTTPS and CORS enabled, allowing us to make requests from client-side code on FT.com.
 - Online UI for easy data fiddling.
 - All this with *zero setup*. Amazing!

Our trick was to have a single blob to act as the ‘index’, the URL for which was hard-coded in the app. The index had an entry for each of our users, linking their user IDs to the generated JSON Blob IDs.

It worked a treat. However, I’d never use this in the real world because:

 - No security whatsoever - a significant problem when you’re storing folks’ web history (even if it is only their FT article views).
 - It _probably_ wouldn’t scale to the real FT.com user-base, who together amass a mighty 700k article views per day. I imagine that [Tristan](https://github.com/tburch) (the developer of JSON Blob), though he’s kind enough to provide this tool for free, wouldn’t be particularly impressed if we were to try this out.

For hackathons and the like though, I couldn’t recommend this approach enough. It’s amazing how quickly things go when you gleefully disregard any notion of good security practices.

### Using a Chrome extension for collecting info

So we needed to be able to collect info on willing folks’ article views so we knew when they’d passed their achievement milestones.

How were we to get the info though? I’d have been very lucky to get a 👍 on a pull request in which I was trying to sneak some guerrilla tracking code into the production site, and I was pretty sure we weren’t allowed to make use of our real production analytics stuff (which probably would have been overkill anyway).

And then it came to us - a [Google Chrome extension](https://developer.chrome.com/extensions)! Once again, the advantages were many:

 - Super quick to get up and running. Bang out a manifest.json file, add a splash of JS and you’re away.
 - Easy development workflow. Reload the extension from local files to try out your code on the real live site.
 - As the extension injects code into a page that already has all of the session authentication and stuff, it can seamlessly get hold of the user’s ID.
 - Effortlessly share your work by publishing your Chrome extension and chucking people a link to its Chrome Web Store page.

On every FT.com page load, our extension peeped at the URL to check that we were on an article page. Once the reader was deemed to have had a good and genuine read, it would upload a new event to that user’s personal JSON blob.

As a cheeky bonus, we had our tracking code flash up a [web notification](https://developer.mozilla.org/en-US/docs/Web/API/notification) to show achievement progress when we saved an event. Pretty snazzy.

![notification](http://static.lrnk.co.uk/blog-content/lazy-hackathon/notification.gif)

### Displaying the achievements page with… the Chrome extension again!

Now that we were collecting all this juicy info, we needed a way of showing it off.

Ideally we’d have been able to build this too straight into FT.com, but this approach would flounder due to the reasons laid out about above, and even if we did get away with it, it would require a degree of cautious professionalism that would slow us down considerably.

Our saviour was, once again, the humble Chrome extension. Here’s what we did:

 - Chose a page to hijack (we went with the myFT page, just because).
 - Effectively removed the original content by hiding it with CSS.
 - Had the extension inject some JS to retrieve our tracking data, plug it into a [Handlebars](http://handlebarsjs.com/) template, and sneak it into the chosen page.

This made for some very speedy UI development. The end result was fairly slow to load (seeing as it had to load all the original content before our stuff kicked in) but perfectly serviceable. It also maintained the advantage that our entire hack was contained within the Chrome extension, making it very easy to share.

### Back to reality

In summary, the hackathon was great and really I enjoyed working at the speed of light and having license for laziness, if only for a couple of days (in fact, it would have been exhausting keep it going for any longer).

Here’s the codez for our thing if you’ll forgive the mess: [github.com/laurieboyes/poliwag](https://github.com/laurieboyes/poliwag).

Thanks to [@upthebuzzard](https://twitter.com/upthebuzzard) for organising, to [@marawanot](https://twitter.com/marawanot) for being the other half of my team’s development effort, and to Emma for some excellent design work. A final thank you to [Tristan Birch](https://github.com/tburch) for maintaining [jsonblob.com](https://jsonblob.com/).



