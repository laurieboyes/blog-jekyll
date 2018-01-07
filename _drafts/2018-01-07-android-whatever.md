---
layout: post
title: Android whatever
date: '2018-01-07 09:51:34'
tags:
- android
- java
---

This weekend I was delighted to discover that, after 3 or 4 years of JavaScript super-fanaticism, I'm still able to Java! A bit anyway. Enough at least to write a small native Android app to plug a gap in the otherwise wonderful and powerful suite of tools available to modern web apps.

### Why go native?

My friend and colleague at the FT [@lc512k](https://twitter.com/lc512k) wrote a really cool web app that uses the [service worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) to give you timely push notifications with some info that's based on your location.

It works great! However, with the web app alone, whenever your location changes and you want the app to know about it, you have to go into the app and press an 'update my location' button. This is because while service workers do give web apps powers they'd never dreamed of before, they don't yet allow them to continually track the user's location, presumably because this functionality has the potential to be a full-blown privacy nightmare.

However, native Android apps have been navigating these trecherous waters for yonks, so I thought I'd take a deep breath and give it ago. Amazingly, it worked!

### Cool, what was involved?

Here's a list of stuff I learned how to do on Android:

#### Used the JobScheduler API to schedule a regularly repeating background job

I took a rather circuitious route to get here, but this allowed me to schedule a job that I could start and stop from my app, but would keep on ticking after the app has closed.

These resources combined to help a great deal:

 - [https://developer.android.com/reference/android/app/job/JobScheduler.html](https://developer.android.com/reference/android/app/job/JobScheduler.html)
 - [https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
 - [https://code.tutsplus.com/tutorials/using-the-jobscheduler-api-on-android-lollipop--cms-23562](https://code.tutsplus.com/tutorials/using-the-jobscheduler-api-on-android-lollipop--cms-23562)

#### Used Google Play services to access the devices last known location

This included having it ask the user nicely for permission to do so. I like that the location API allows you to get the last known location without having to do any real-time, battery-draining GPS stuff. Apparently on Android 8, this will be updated a few times an hour in the background, which is good enough for our purposes.

The Android developer docs were very helpful on this subject: [https://developer.android.com/training/location/retrieve-current.html](https://developer.android.com/training/location/retrieve-current.html)

