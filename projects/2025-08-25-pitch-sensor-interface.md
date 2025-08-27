---
layout: post
title: Pitch sensor + interface
---

Musicians who play instruments that don't have built-in pitches[^1] are constantly working on intonation (pitch accuracy). Having good and consistant intonation is one of many important elements in musicianship, alongside tone color, clarity, rhythmic accuracy, dynamic contrast, and many other quantifiable, unquantifiable and semi-quantifiable factors. Pitch accuracy however, is highly quantifiable, at least in theory. Every note is just a frequency, and "correct" frequencies are just multiples of a standard pitch (e.g. A4 = 440hz), specifically multiplying by the 12th root of 2. This led me to the idea of creating a platform for musicians to record takes of themselves practicing and providing feedback in the form of pitch accuracy checking, aimed at musicians who have gone beyond the basics.

While this could be implemented completely locally on a phone app, I decided to record and upload audio to a web server using an INMP441 I2S microphone and an ESP32 development board for the purposes of having an embedded and web development component of the project. 

1. Exposition, first tests:

Many software services catering to beginning musicians have practicing tools that emphasize pitch accuracy [^2]. They accomplish this by repeatedly comparing what the current pitch should be and what the pitch being played by the player actually is, which is not dissimilar to Guitar Hero. This approach relies on the existence of "charts" that exist on a per-piece/song basis that tell the software the expected pitch at each moment in time. This makes it not useful for people who have escaped beginner's purgatory for a few reasons:

-Rhythmic relations are fixed in place. This means while that a piece as a whole could be sped up or slowed down, there is no freedom in how a particular phrase might be expressively sped up or slowed down (classical musicians call this rubato).
    -This approach falls apart even further for anything improvisatory, 
-With the vast quantity of musical literature across many instruments, it would be a large task of creating charts for every piece users might want to play. 

The approach I came up with does not require "charts". Knowing that by this stage in musical development, the pitch being played is usually "mostly" correct, it becomes good enough to infer pitch intention by determining what the closest possible correct note is to the played note.

Early tests

![](\assets\images\pitch-sensor-interface\Capture1.png)

2. Development, feature development

(For testing purposes, I added a direct audio upload)

3. Recapitulation

[^1]: Almost all instruments other than guitars and keyboards. Even woodwind players with poor embrochure can have subpar intonation. Except saxophone players, because the saxophone is an easy instrument.
[^2]: Search "learn to play \[instrument\] on your phone's app store if you don't know what I mean.
