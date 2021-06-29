---
layout: post
title: CSS for img tags that look like custom emojis
date: '2021-06-29 10:54:00'
tags:
  - web
feature-img: /assets/img/2021-06-29-css-for-img-tags-that-look-like-custom-emojis/cover.png
---


**_Tl;dr_** _some css to make inlined img tags look like custom emojis_

### The problem

You want to copy messages from slack into an html page, including the emojis. If your slack messages only contain standard emojis, this is easy, but if you want to include messages with _custom_ emojis, this is slightly less easy.


### Why?

Maybe you want to preserve slack messages in some sort of ‘hall of fame’ website. It’s a weird thing to want to do, but at least one person has found themselves in that situation (it’s me, I’m the person).

### Ok I’ll bite, give me the codez

```html
<style>
   .emoji {
      
       /* use em for sizing so that your emoji scales with your font size ✨ */
       height: 1.3em;
 
       /* this aligns the emoji vertically in such a way that it looks pretty much like it looks on slack ✨ */
       vertical-align: text-bottom;
 
   }
</style>
 
<p>
   hey check it out
   <img class="emoji" src="https://emoji-town.org/wizard.gif" />
   it feels just like slack
   <img class="emoji" src="https://emoji-town.org/slack.png" />
</p>
```

<style>
    img.emoji {
        
        /* use em for sizing so that your emoji scales with your font size ✨ */
        height: 1.3em;

        /* this aligns the emoji vertically in such a way that it looks pretty much like it looks on slack ✨ */
        vertical-align: text-bottom;

        display: inline-block;

    }
</style>

<p style="margin-top: 50px;">
    hey check it out <img class="emoji" src="/assets/img/2021-06-29-css-for-img-tags-that-look-like-custom-emojis/emoji-party-wizard.gif"> it feels just like slack <img class="emoji" src="/assets/img/2021-06-29-css-for-img-tags-that-look-like-custom-emojis/emoji-slack.png">
</p>

### Cool I guess

That’s all for now. If you’re hungry for more, have a browse of the archive. See you next year!