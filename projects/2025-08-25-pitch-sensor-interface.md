---
layout: post
title: Pitch sensor + interface
categories: projects
---

Web server repo: [https://github.com/jwinn03/practice-dashboard-server](https://github.com/jwinn03/practice-dashboard-server) 

ESP32 audio streamer repo: [https://github.com/jwinn03/esp-audio-streamer](https://github.com/jwinn03/esp-audio-streamer)

Try it [here](/demos/pitch.html)

Musicians who play instruments that don't have built-in pitches[^1] are constantly working on intonation (pitch accuracy). Having good and consistent intonation is one of many important elements in musicianship, alongside tone color, clarity, rhythmic accuracy, dynamic contrast, and many other quantifiable, unquantifiable and semi-quantifiable factors. Pitch accuracy however, is highly quantifiable, at least in theory. Every note is just a frequency, and "correct" frequencies are just multiples of a standard pitch (e.g. A4 = 440hz), specifically multiplying by the 12th root of 2. This led me to the idea of creating a platform for musicians to record takes of themselves practicing and providing feedback in the form of pitch accuracy checking, aimed at musicians who have gone beyond the basics.

While this could be implemented completely locally as a phone or web app, I decided to record and upload audio to a web server hosted in a Docker container using an INMP441 I2S microphone and an ESP32-S3 development board, for the purposes of having embedded and backend development components in the project. (Later, I added file upload and microphone input to the web interface.) The ESP32 operates as a WebSocket client, sending 16-bit, 16kHz raw binary audio data to the web server; this was the highest audio quality that would work without data dropping out, and is more than enough for this use case. The ESP32 component is simple enough that it could be implemented using the Arduino Core, but I decided to use the ESP-IDF development framework with FreeRTOS since it is allows for more control over scheduling, which is important in this performance-sensitive application, and because it is a more industry-standard way of programming embedded hardware in general. 

![](\assets\images\pitch-sensor-interface\IMG_3602.jpg)

## 1. Exposition, the idea:

Many software services catering to beginning musicians have practicing tools that emphasize pitch accuracy [^2]. They accomplish this by repeatedly comparing what the current pitch should be and what the pitch being played by the player actually is, which is not dissimilar to Guitar Hero. This approach relies on the existence of "charts" that exist on a per-piece/song basis that tell the software the expected pitch at each moment in time. This makes it not useful for people who have escaped beginner's purgatory for a few reasons:

-Rhythmic relations are fixed in place. This means while that a piece as a whole could be sped up or slowed down, there is no freedom in how a particular phrase might be expressively sped up or slowed down (classical musicians call this rubato).
    -This approach falls apart even further for anything improvisatory, 
-With the vast quantity of musical literature across many instruments, it would be a large task of creating charts for every piece users might want to play. 

The approach I came up with does not require "charts". Knowing that by this stage in musical development, the pitch being played is usually "mostly" correct, it becomes good enough to infer pitch intention by determining what the closest possible correct note is to the played note.

![](\assets\images\pitch-sensor-interface\Capture1.png)

## 2. Development, feature development

To detect and log pitches in the audio, a Javascript library called pitchy was used. Short samples of the audio are taken at hundreds of times per second. When a sample has a detectable pitch, its pitch and accuracy are pushed into a stack. This data log is then graphed as discrete points on an accuracy-versus-time graph, implemented using the chart.js library [^3].

To simplify feature development, I added a direct audio upload to the interface, instead of having to go through the ESP32.

One of the most important features to implement was the graphing of accuracy history, to be able to visualize the performance. Implementing an audio player on top of the graph would allow points of interest to quickly be inspected. 

This turned out to be more difficult than anticipated. Stacking an audio player element on top of the chart.js element did not initially seem doable to accomplish. I'm sure having experience in Javascript UI development would have helped, but initially I went with a highly jank solution of putting a red dot on top of the graph that tracks with the separate audio player. This way, at least the core functionality of visualizing the audio player's progress with its respective accuracy would be there. 

![](\assets\images\pitch-sensor-interface\Capture4.PNG)

Here's a few pictures of chart.js Pain from a certified Javascript noob while trying to it to work:

![](\assets\images\pitch-sensor-interface\Capture5.PNG)
![](\assets\images\pitch-sensor-interface\Capture7.PNG)

Eventually, with some help from Anthropic's latest coding model, Claude 4.5 Sonnet (I tried to limit usage of LLMs for use as a highly specifiable encyclopedia and not for code-writing duties), I was able to combine the audio player and accuracy chart into one visual element. This was done by having a hidden audio player element, controlled with a custom HTML play/pause button, as well as implementing scrubbing by using chart.js to monitor for clicks on the graph.

Other refinements included:
- Adding the ability to pan and zoom the chart, which is important for particularly long audio tracks. 
- Stopping a "point" rendering for every single data point of accuracy history, which would lead to absolute chaos when a large audio file was zoomed out. It worked to just remove the lines between points
- Adding highlighting of low clarity zones. This was done by lightly coloring in regions around any low clarity points, which naturally results in large bands of blue when there are many consecutive low clarity points.
- User-changeable options, including changing whether the y-axis is accuracy percentage or error in cents, and the frequency of A4 (440hz by default)
- In the case of long files, limiting the initial scope to a more visually manageable time frame instead of showing every point in the whole file at once
- Implementing direct microphone input from the browser. This took more work than I thought it would.

I could have also implemented highlighting of low accuracy zones. However, I chose not to because what is acceptable accuracy-wise depends on context and the player's own practice goals; the graph is plenty of visualization. 

Note that where pitch *clarity* is low, associated points are not rendered. There are occasional points where the pitch detector spuriously finds high clarity and thus renders a pseudo-random point even if there wouldn't be an obvious pitch to a human listener; this is natural given the imperfection of audio recording and digital signal processing.

The audio used during testing was a combination of sine wave tones and actual music. You could probably look at the images and guess which is which.

One lesson I learned is that sometimes, performance optimizations might be possible, but in practice completely unnecessary. One function, findClosestNote(), is given a measured pitch and finds the in-tune note closest to it. It does this by using an array of frequencies containing each possible frequency of an in-tune note. Since the array is already sorted, a search algorithm can be used, though it should search for the closest value instead of an exact match. Initially, for simplicity, I implemented a linear search. I later went back and changed this to a binary search since this theoretically has O(log n) instead of O(n) performance. I expected this to have a large impact on the time taken for an audio file to be analyzed, since the function is called for every sample, with hundreds of samples being taken per second. However, the difference was actually miniscule, within the margin of run-to-run variance:

![](\assets\images\pitch-sensor-interface\Capture8.PNG)
![](\assets\images\pitch-sensor-interface\Capture9.PNG)

In both implementations, running a search took at most a couple thousandths of a millisecond, stretching the ability of Javascript to measure small times.

![](\assets\images\pitch-sensor-interface\Capture10.PNG)

Even if tens of thousands of frequencies are being searched, the compounded difference would only be tens of milliseconds. Far more time is taken performing I/O operations and calling functions from the pitchy library, which uses FFTs to calculate the pitches of samples. If the array of frequencies were extremely large, maybe the difference would be noticeable, but there are only 88 values, reflecting the number of keys on a piano and a more-than sufficient range. 

## 3. Recapitulation: Final touches

Here's a few examples of the tool in action.

- Visualizing violin vibrato (example from Paganini's 24th Caprice, 3rd variation)

![](\assets\images\pitch-sensor-interface\Capture11.PNG)

The approach to pitch detection tends to struggle in these cases:
- Multiple tones at once, where the pitch detector will struggle to pick up any pitch at all
    - This makes it struggle in ensemble settings, though this tool was always meant more for individual practice. Anyone taking time to work on individual technique during ensemble rehearsals is not a serious musician anyways!
- Percussive moments, such as a "dropped" bow technique on a string instrument, or other transitory non-pitched sounds between notes. This is exasperated when there are rapid note changes, as there is less time for a clear, stable pitch to make itself apparent compared to the unclear ones.
- Too much extraneous noise. When testing with recordings from my laptop's internal microphone, the fan noise was too much for the pitch detector to find any clear tones, even though my humming was clearly audible above it.

These factors mean that this system works best with woodwind and brass instruments, where these weaknesses are mitigated. 

Because of the assumption that measured pitch will be within ±50 cents (a.k.a. a quartertone) of the intended pitch, if it was not within that window, the accuracy reading alone will not be reliable. In the future, this could be mitigated with an indicator that shows the presumed pitch. If the player sees that it does not match the intended pitch, then they know they were *way* off. However, this would be difficult to implement cleanly (there are ~100 data points per second, but we'd only want to show individual "average" assumed notes and therefore would have to somehow decide what the start and end of assumed notes are) and there would be the possibility of false positives that would make the experience confusing.

[^1]: Almost all instruments other than guitars and keyboards. Even woodwind players with poor embouchure can have subpar intonation. Except saxophone players, because the saxophone is an easy instrument.
[^2]: Search "learn to play \[instrument\] on your phone's app store if you don't know what I mean.
[^3]: In cases where a pitch is not detected, a data "point" is not rendered on the graph. This naturally leads to areas (mostly) devoid of points where there is silence or other non-tonal noise in the audio, which will lead the user to be able to safely assume to ignore those regions.
