---
layout: post
title: Mucking about with image data in JavaScript
image: "/content/images/2016/07/Screen-Shot-2016-07-24-at-00-28-42.png"
date: '2016-07-24 09:42:05'
tags:
- javascript
alias: /mucking-about-with-image-data-in-javascript
feature-img: http://static.lrnk.co.uk/blog-content/playing-with-image-data/image-data-cover.png
---

Earlier this year I wrote _ScarfParty_, a knitting companion that centred around a dynamic pattern display that changed along with the knitter's progress. The pattern information was loaded in from a small black and white image. If you're interested, you can [learn more about my adventures with scarves](/tags.html#the-christmas-fam-scarf).

![image mucking](http://static.lrnk.co.uk/blog-content/playing-with-image-data/image-mucking.png)

This post is about the code behind taking in that image and regurgitating it in a different form that I could play around with.

_All the code referenced in this post:_ [github.com/laurieboyes/scarfparty-v2/blob/master/src/pattern.js](https://github.com/laurieboyes/scarfparty-v2/blob/master/src/pattern.js)

_And the tests:_ [github.com/laurieboyes/scarfparty-v2/blob/master/test/tests/pattern.spec.js](https://github.com/laurieboyes/scarfparty-v2/blob/master/test/tests/pattern.spec.js)

#### Reading the image

Problem number one was to get the information out of the image with JavaScript.

I hadn't a clue how this would happen or whether there'd even be an API for it but I had one if those googling eureka moments when I came across the technique of drawing an image to a canvas and interrogating the canvas for the information using `ctx.getImageData` ([I think it was this StackOverflow answer](http://stackoverflow.com/a/17789253/1841845)). Neat.

```javascript
/**
 * @param img
 * @returns {Array} array of integers representing the image, 
 * wherein each pixel is represented by a group of 4 numbers
 * between 0 and 255 corresponding to its rgba values
 */
function getImageData(img) {
    //Create a canvas in memory that we can use to get the pattern image data
    const canvas = document.createElement('canvas');
    canvas.width = img.naturalWidth;
    canvas.height = img.naturalHeight;

    const ctx = canvas.getContext('2d');
    ctx.drawImage(img, 0, 0);
    return Array.from(ctx.getImageData(0, 0, img.naturalWidth, img.naturalHeight).data);
}
```
<span class="paragraph-space-forcer"></span>
For example, when passed this _very_ small image 3 x 2 pixels in size:

![image mucking](http://static.lrnk.co.uk/blog-content/playing-with-image-data/tiny-image.png)

My `getImageData` function returns the following array:

```javascript
[255,255,255,255,0,0,0,255,0,0,0,255,255,255,255,255,255,255,255,255,255,255,255,255]
```
<span class="paragraph-space-forcer"></span>

Broken down into pixels, we can see that each is represented by its 4-number rgba value

```javascript
[
 255,255,255,255,   0,  0,  0,  255,   0,  0,  0,  255,

 255,255,255,255,   255,255,255,255,   255,255,255,255
]
```

<span class="paragraph-space-forcer"></span>
#### Translating the image data

I needed to turn this flat array of information into something meaningful and easy to use to recreate the image. All I really needed to know was which pixels were black and which were white, so I wrote this small function to take the results of the above and turn them into a flat array of 1s and 0s.

```javascript
/**
 * @param imageData
 * @returns {Array} array of 1s and 0s, one element for each pixel in the pattern, read one row at a time from left to
 * right. A value of 1 represents that the pixel is the dark colour and 0 that it is light
 */
function getPixelsOnOff(imageData) {
    return imageData

        // only have to look at one colour channel to know if it's black or white, so just look at the red of each
        // pixel, filtering the rest out
        .filter((_, i) => i % 4 === 0)

        // if the value is greater than 127 (~half of the maximum of 255) it's light, else it's dark
        .map(pixel => pixel > 127 ? 0 : 1);
}
```
<span class="paragraph-space-forcer"></span>
When passed the array resulting from the `getImageData` function call above, the `getPixelsOnOff` function returns this array:
```
[0,1,1,0,0,0]
```
<span class="paragraph-space-forcer"></span>

The final step was to group the array into rows, all set to be visualised. As we know the image dimensions from the image itself, this part is trivial.

```
[
    [0,1,1],
    [0,0,0]
]
```
<span class="paragraph-space-forcer"></span>

And there we have it! We now have an easy to read array of the on/off state of each pixel, grouped into rows, which we can use to redisplay the image in any way we choose. In my case this basically involved looping over the nested arrays and drawing squares on a canvas with the appropriate position and colour.

#### Controversial choices

My flatmate, peering over my shoulder, has just pointed out that returning 1s and 0s rather than booleans is a bit of a code smell. My best guess as to why I didn't think of this myself at the time is that doing it this way made my test cases look nicer:

```javascript
// if you squint you can almost see it
expect(pattern.rows).to.deep.equal([
	[0, 0, 0, 0, 0, 0, 0, 0, 0],
	[0, 0, 0, 0, 0, 0, 0, 0, 0],
	[0, 0, 1, 0, 0, 0, 1, 0, 0],
	[0, 1, 1, 1, 0, 1, 1, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 0, 1, 1, 1, 1, 1, 0, 0],
	[0, 0, 0, 1, 1, 1, 0, 0, 0],
	[0, 0, 0, 0, 1, 0, 0, 0, 0],
	[0, 0, 0, 0, 0, 0, 0, 0, 0],
	[0, 0, 0, 0, 0, 0, 0, 0, 0]
]);
```