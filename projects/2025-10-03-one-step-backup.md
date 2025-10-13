---
layout: post
title: One Step Backup
categories: projects
---

This was a project whose idea I came up with while trying to use SharePoint's web browser interface to transfer files from a network shared drive to my local device. Each folder contained thousands of files of data, with three types of files: CSVs, PNGs, and TXTs. I only wanted the PNGs, so I sorted the list by file type, but oh no! Because "PNG" is alphabetically in the middle of the three, I would have to scroll down torwards the middle of the list to select the first file I wanted, then even further down to shift-click the last. This is inconvenient enough, but I couldn't even do that; there were so many files that the interface would lag or bug out before I could scroll down far enough to select the first PNG. Searching for "png" didn't help because the SharePoint search bar is an affront to God; it'll list a few files with "png" in it before I press enter, but when I do, it shows me this:

What if someone who didn't have as much technical knowledge wanted to do this? Well, now they'd probably ask an LLM and that would probably give them something that worked. But I still wanted to create something to "solve" this problem for two reasons:

- To get experience developing a "real" front-end application to solve a "real" problem, as if I were a "real" software developer (including details like licensing and documentation)
- To get more practice building applications using the Qt framework. I had started using Qt for a third-year class in college, but partially because I was taking a lot of classes


A couple lessons:

Visual Studio may not be configured by default to show the qDebug() stream, even when using a debug build; it just shows you build information in the bottom pane "output" terminal. I ended up needing to manually configure the project to run the executable on the "console" subsystem, and not the "windows" one. VS Code does show this output by default. 

Couple more steps to make truly one step:

-Default source directory to user's folder
-Default destination directory to an external drive, if any is connected