---
layout: post
title: Bluetooth Pedal
categories: projects
---

This is a retrospective write-up on a project that I did circa late 2024/early 2025. 

In recent years, performing musicians have been picking up tablets, most commonly iPads [^1], in place of paper for displaying sheet music. It's not an uncommon sight to see iPad holders in place of traditional music stands, especially with smaller ensembles or solo performers.  Normally, with paper, the music on the page would have to be arranged in such a way as to give pauses at the end of pages, in order to give the player time to physically flip the page. If this has to be done *extremely* fast, sometimes  fumbling occurs that results in missed entrances. If the music copyist is particularly lazy, there may be no pauses at all which necessitates putting additional sheet music on the stand. 

One advantage of using iPads is that it enables the use of external controllers to "turn" the pages of the music; this is both instant and doesn't require the musician to stop playing. The most common implementation of controllers are Bluetooth foot pedals, which is not surprising since the feet are independent and (almost) no instruments use both feet for operation. There are most often two pedals, one to go forward a page, and one to go back, and they are basically all variations of these two shapes:

![](\assets\images\bluetooth-pedal\pedal1.PNG)
![](\assets\images\bluetooth-pedal\pedal2.jpg)


Although I don't have a bluetooth pedal to confirm this, I heavily suspect that the pedals are using Bluetooth's HID profile, sending the scan keys associated with the left/right arrow keys (or possibly up/down) on a keyboard. In any music app compatible with Bluetooth foot pedals, pressing the arrow keys on a connected keyboard will accomplish the same thing as a pedal would. In fewer words, Bluetooth foot pedals are really just this:

![](\assets\images\bluetooth-pedal\keys.jpg)

Yet if you go to a site like Amazon to buy one, you'll notice many of them are shocking expensive for what is essentially just a battery, a couple buttons, and a Bluetooth transmitter. At the time of this project's creation, the cheaper models definitely weren't as cheap as they are now.



[^1]: Seeing a musician use anything other than Apple products for personal computing is about as rare as 