---
layout: post
title: Pitch sensor + interface
---

*FYI: This page is a work-in-progress.*

Musicians who play instruments that don't have built-in pitches[^1] are constantly working on intonation (pitch accuracy). Having good and consistant intonation is one of many important elements in musicianship, alongside tone color, clarity, rhythmic accuracy, dynamic contrast, and many other quantifiable, unquantifiable and semi-quantifiable factors. Pitch accuracy however, is highly quantifiable, at least in theory. Every note is just a frequency, and "correct" frequencies are just multiples of a standard pitch (e.g. A4 = 440hz), specifically multiplying by the 12th root of 2. This led me to the idea of creating a platform for musicians to record takes of themselves practicing and providing feedback in the form of pitch accuracy checking, aimed at musicians who have gone beyond the basics.

While this could be implemented completely locally on a phone app, I decided to record and upload audio to a web server using an INMP441 I2S microphone and an ESP32-S3 development board for the purposes of having an embedded and web development component of the project. 

1. Exposition, first tests:

Many software services catering to beginning musicians have practicing tools that emphasize pitch accuracy [^2]. They accomplish this by repeatedly comparing what the current pitch should be and what the pitch being played by the player actually is, which is not dissimilar to Guitar Hero. This approach relies on the existence of "charts" that exist on a per-piece/song basis that tell the software the expected pitch at each moment in time. This makes it not useful for people who have escaped beginner's purgatory for a few reasons:

-Rhythmic relations are fixed in place. This means while that a piece as a whole could be sped up or slowed down, there is no freedom in how a particular phrase might be expressively sped up or slowed down (classical musicians call this rubato).
    -This approach falls apart even further for anything improvisatory, 
-With the vast quantity of musical literature across many instruments, it would be a large task of creating charts for every piece users might want to play. 

The approach I came up with does not require "charts". Knowing that by this stage in musical development, the pitch being played is usually "mostly" correct, it becomes good enough to infer pitch intention by determining what the closest possible correct note is to the played note.

Early tests

![](\assets\images\pitch-sensor-interface\Capture1.png)

2. Development, feature development

To simplify feature development, I added a direct audio upload to the interface, instead of having to go through the ESP32.

One of the most important features was to implement was the graphing of accuracy history, to be able to visualize the performance. Implementing an audio player on top of the graph would allow points of interest to quickly be inspected. 

This turned out to be more difficult than anticipated. Stacking an audio player element on top of the chart.js element did not seem doable to accomplish. I'm sure having experience in Javascript UI development would have helped, but initially I went with a highly jank solution of putting a red dot on top of the graph that tracks with the separate audio player. This way, at least the core functionality of visualizing the audio player's progress with its respective accuracy would be there. 

![](\assets\images\pitch-sensor-interface\Capture4.png)

Here's a few pictures of chart.js Pain from a certified Javascript noob while trying to it to work:

![](\assets\images\pitch-sensor-interface\Capture5.png)
![](\assets\images\pitch-sensor-interface\Capture7.png)

Eventually, with some help from Anthropic's latest coding model, Claude 4.5 Sonnet, I was able to combine the audio player and accuracy chart into one visual element. 

Other refinements included:
-Adding the ability to pan and zoom the chart, which is important for particularly long audio tracks. 
-Stopping a "point" rendering for every single data point of accuracy history, which would lead to absolute chaos when a large audio file was zoomed out. It worked to just remove the lines between points
-Adding highlighting of low accuracy zones. This was done by lightly coloring in red regions of many consecutive low accuracy points.

The audio used during testing was a combination of sine wave tones and actual music. You could probably look at the images and guess which is which.



[add image]

3. Recapitulation

[^1]: Almost all instruments other than guitars and keyboards. Even woodwind players with poor embrochure can have subpar intonation. Except saxophone players, because the saxophone is an easy instrument.
[^2]: Search "learn to play \[instrument\] on your phone's app store if you don't know what I mean.
