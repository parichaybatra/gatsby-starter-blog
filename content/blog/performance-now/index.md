---
title: Bye Bye Date.now(), Performance.now is here for rescue 
date: "2020-04-18T22:40:32.169Z"
description: Know how web browsers measure performance
---

Building amazing, beautiful and feature-rich website is easier than ever before. Not long ago, you’d have to fire up a text-editor and hand-craft a lot of HTML, CSS, and JavaScript for a basic website. Today, you can use tools and third-party libraries that make development of even complex websites/webapps much easier. The flip side of this is that it can be hard to see everything that’s going into your website which can have an impaxt on performance.

The good news is that modern web browsers expose lots of performance data that can help you understand how your web page performs. With the launch of Browser Insights today, we can analyze the performance from the perspective of the web browser and what the end user actually experiences.

[The Performance interface](https://developer.cdn.mozilla.net/en-US/docs/Web/API/Performance) provides access to performance-related information for the current page. It's part of the High Resolution Time API, but is enhanced by the Performance Timeline API, the Navigation Timing API, the User Timing API, and the Resource Timing API.

 In this blog post, we’ll dive into one of the methods provided by Performance interface, Performance.now() and why we need to start using it instead of out good old Date.now() to measure performance.


 ## Syntax

  
>
>  t = performance.now();
>
> **NOTE:** The [High Resolution Timer](https://w3c.github.io/hr-time/) was added by the [WebPerf Working Group](https://www.w3.org/webperf/) to allow measurement in the Web Platform that's more precise than what we've had with +new Date and the newer Date.now()


## Still measuring performance in milliseconds?


>
> var t0 = performance.now();   
> doSomething();   
> var t1 = performance.now();   
> console.log("Call to doSomething took " + (t1 - t0) + " milliseconds.");   
>


Unlike **Date.now()**, the timestamps returned by **performance.now()** are not limited to one-millisecond resolution. Instead, they represent times as floating-point numbers with up to microsecond precision.

Another notable difference is that the values returned by **performance.now()** always increase at a constant rate, independent of the system clock (which might be adjusted manually or skewed by software like NTP). Otherwise, **performance.timing.navigationStart** + **performance.now()** will be approximately equal to **Date.now()**.


### Comparison

The ECMAScript Language specification [ECMA-262](https://tc39.es/ecma262/) defines the [Date](https://tc39.es/ecma262/#sec-date-objects) object as a time value representing time in milliseconds since 01 January, 1970 UTC. For most purposes, this definition of time is sufficient as these values represent time to millisecond precision for any instant that is within approximately 285,616 years from 01 January, 1970 UTC.



> **So just to compare, here are the sorts of values you'd get back:**
> Date.now()         //  1587243955972   
> performance.now()  //  20303.427000007   
>  


You'll notice the two above values are many orders of magnitude different. **performance.now()** is a measurement of floating point milliseconds since that particular page started to load (*the performance.timing.navigationStart timeStamp to be specific*). You could argue that it could have been the number of milliseconds since the [unix epoch](https://en.wikipedia.org/wiki/Unix_time), but rarely does a web app need to know the distance between now and 1970. This number stays relative to the page because you'll be comparing two or more measurements against eachother.




> var mark_start = Date.now();    
> doTask(); // Some task    
> var duration = Date.now() - mark_start;    


For certain tasks this definition of time may not be sufficient as it does not allow for sub-millisecond resolution and is subject to system clock skew. For example:



1.  When attempting to accurately measure the elapsed time of navigating to a Document, fetching of resources or execution of script, a monotonically increasing clock with sub-millisecond resolution is desired.
2.  When calculating the animation state from script, developers will need to accurately know the amount of time that has elapsed in the animation in order to properly update the next scene of the animation.

3.  When calculating the frame rate of a script based animation, developers will need sub-millisecond resolution in order to determine if an animation is drawing at 60 FPS. Without sub-millisecond resolution, a developer can only determine if an animation is drawing at 58.8 FPS or 62.5 FPS.
4.  When attempting to cue audio to a specific point in an animation or ensure that the audio is synchronized with the animation, developers will need to accurately know the amount of time elapsed in the animation and audio.

5.  When multiple contexts need to synchronize work with sub-millisecond resolution (e.g. when using Worker or SharedWorker workers to drive animation, audio, etc., in a renderer context), or to create a unified view of the event timeline.


## Reduced time precision

To offer protection against timing attacks and fingerprinting, the precision of performance.now() might get rounded depending on browser settings.
In Firefox, the privacy.reduceTimerPrecision preference is enabled by default and defaults to 1ms.



> // reduced time precision (1ms) in Firefox 60  
> performance.now();  
> // 8781416  
> // 8781815  
> // 8782206  
> // ...  
>  
>  
> // reduced time precision with `privacy.resistFingerprinting` enabled  
> performance.now();  
> // 8865400  
> // 8866200  
> // 8866700  
> // ...  

In Firefox, you can also enable privacy.resistFingerprinting — this changes the precision to 100ms or the value of privacy.resistFingerprinting.reduceTimerPrecision.microseconds, whichever is larger.




### Summary

In summary, **performance.now()** is...

 - a double with microseconds in the fractional
 - relative to the **navigationStart** of the page rather than to the UNIX epoch
 - not skewed when the system time changes
 - available in all major browsers Chrome 24+, Edge 12+,  Firefox 15+, and IE10.