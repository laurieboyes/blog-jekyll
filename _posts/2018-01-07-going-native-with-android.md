---
layout: post
title: Dusting off old language skills and going native with Android (temporarily)
date: '2018-01-07 09:51:34'
tags:
- android
- java
feature-img: http://static.lrnk.co.uk/blog-content/android/cover.png
---

***Tl;dr*** *I wrote an Android app. It does some background processing and some location tracking and [here's the code](https://github.com/laurieboyes/pokemon-radar-location-tracker) (it's messy).*

This weekend I was delighted to discover that, after 3 or 4 years of JavaScript super-fanaticism, I'm still able to Java! A bit anyway. Enough at least to write a small native Android app to plug a gap in the otherwise wonderful and increasingly powerful suite of tools available to modern web apps.

Here's my gnarly spaghetti Java code: [github.com/laurieboyes/pokemon-radar-location-tracker](https://github.com/laurieboyes/pokemon-radar-location-tracker).

### Why go native?

My friend and colleague at the FT [@lc512k](https://twitter.com/lc512k) wrote a really cool web app that uses the [service worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) to give you timely push notifications with some info that's based on your location.

It works great! However, with the web app alone, whenever your location changes and you want the app to know about it, you have to go into the app and press an 'update my location' button. This is because while service workers do give web apps powers they'd never dreamed of before, they don't yet allow them to continually track the user's location, presumably because this functionality has the potential to be a full-blown privacy nightmare.

However, native Android apps have been navigating these trecherous waters for yonks, so I thought I'd take a deep breath and give it ago. Amazingly, it worked!

### Cool, what was involved?

Here's a list of stuff I learned how to do on Android:

#### Use the JobScheduler API to schedule a regularly repeating background job

I took a rather circuitious route to get here, but this allowed me to schedule a job that I could start and stop from my app, but would keep on ticking after the app had closed.

These resources combined to help a great deal:

[https://developer.android.com/reference/android/app/job/JobScheduler.html](https://developer.android.com/reference/android/app/job/JobScheduler.html)

[https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)

[https://code.tutsplus.com/tutorials/using-the-jobscheduler-api-on-android-lollipop--cms-23562](https://code.tutsplus.com/tutorials/using-the-jobscheduler-api-on-android-lollipop--cms-23562)

#### Use Google Play services to access the device's last known location

This included having it ask the user nicely for permission to do so. I like that the location API allows you to get the last known location without having to do any real-time, battery-draining GPS stuff. Apparently on Android 8, this will be updated a few times an hour in the background, which is good enough for our purposes.

The Android developer docs were very helpful on this subject: [https://developer.android.com/training/location/retrieve-current.html](https://developer.android.com/training/location/retrieve-current.html)

#### Make a JSON HTTP POST request

This was so that on every tick the updated location could be POSTed to the web app's backend (provided it was far enough away from the previous location).

The Android developer docs recommend usage of the Volley library for these purposes, which I felt was enough endorsement to encourage me to do the same: [https://developer.android.com/training/volley/request.html](https://developer.android.com/training/volley/request.html)

#### Use Android Studio’s layout editor to make a simple UI

![layout](http://static.lrnk.co.uk/blog-content/android/layout.png?cachebust=1)

(And it is very simple)

I thought I'd be crying out for the familiar and comfortable feeling of writing layouts with HTML and CSS, but the layout editor made this surprisingly easy! Although I couldn't say for certain whether or not my enthusiasm would hold out if I were to attempt a more complex UI.

I went for a constraint-based layout, which means everything is positioned according to its proximity to everything else. There are plenty of other layout types (grid, frame, etc) but I'm happy with this one for my app.

#### Persisted user input in the SharedPreferences thing

Amongst other storage solutions, Android has a thing called _SharedPreferences_ which is a service for creating a persisted key value stores retreivable by an assigned name. This is pretty neat. I guess it's like [browser localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) but accessible between apps.

The Android developer docs told me everything I needed: [https://developer.android.com/guide/topics/data/data-storage.html#pref](https://developer.android.com/guide/topics/data/data-storage.html#pref)

### Neat

Yeah I know, right? The linked resources really helped me and I'm sure would help anyone trying to acheive similar ends. If that's you, good luck!
