---
layout: post
title: One Step Backup
categories: projects
---

This was a project the idea for which I came up with while trying to use SharePoint's web browser interface to transfer files from a network shared drive to my local device. Each folder contained thousands of files of data, with three types of files: CSVs, PNGs, and TXTs. I only wanted the PNGs, so I sorted the list by file type, but oh no! Because "PNG" is alphabetically in the middle of the three, I would have to scroll down torwards the middle of the list to select the first file I wanted, then even further down to shift-click the last. This is inconvenient enough, but I couldn't even do that; there were so many files that SharePoint's interface would lag or bug out before I could scroll down far enough to select the first PNG. Searching for "png" didn't help because the SharePoint search bar is an affront to God; I'll type "png" and it'll list a few files with "png" in it, but when I press enter to show all found files, it shows me this:

![](\assets\images\one-step-backup\Capture.PNG)

What ended up saving me was that because the files were stored using Microsoft's OneDrive cloud infrastructure, I could open the folder as a network-shared drive in Windows' much faster desktop file explorer. From there, I could do what I tried to do before or, failing that, use a short command or script. If I wasn't using Windows, I might've had to wait until I met with someone who could download the files to transfer them the old-fashioned way with an external drive.

I thought to myself, what if someone who didn't know about OneDrive on desktop wanted to do this? Well, now they'd probably ask an LLM and that would *probably* give them a solution that worked. But I still wanted to build something, an application to solve this problem (or at least a tangentally related one) for two reasons:

- To get experience developing a "real" front-end application to solve a "real" problem, as if I were a "real" software developer developing a "real" product (including details like licensing and documentation)
- To get more practice building applications using the Qt framework. I had started learning Qt for a class in the third year of college, but partially because I was taking a lot of classes and partially because the professor wasn't so great at teaching the material (not helpful when dealing with someone with as many new concepts and unique syntax as Qt development), I didn't really feel I was internalizing the material. A large project using Qt was the first and only time I had to pull an all-nighter to complete an assignment.

Of course, this application isn't really a solution to my intiial problem because I'm not a developer on Microsoft's horrific SharePoint web interface. It isn't even remotely using the same technologies as Sharepoint. Still, I figured it would be a good developmental and potentially useful, open-ended expedition to make something.

So, what is "something"? Since I wanted to use Qt, I knew it would be a desktop application that streamlined the process of copying files. I knew that I wanted to be able to select specific file types to be able to be copied and ignore others. This gives a few mandatory elements:

- A source directory selector
- A destination directory selector
- A menu to select specific file types to include/exclude
- A "confirm"/"copy files" button

A nice-to-have would be a text box displaying files that will be copied with the current selection, or any other useful software state information.

The development was greatly helped by the relatively straightforward task, as well as the good documentation for Qt written by the Qt group. I built the project using Visual Studio Code/CMake for most code development, and Visual Studio/MSbuild to build the release executable. A couple lessons learned:

- Visual Studio may not be configured by default to show the qDebug() stream, even when using a debug build; it just shows you build information in the bottom pane "output" terminal. I ended up needing to manually configure the project to run the executable on the "console" subsystem, and not the "windows" one. VS Code does show this output by default. 
- Visual Studio Code wants to use the default Visual Studio configuration of MSVC in the VS folder to compile C++, but for Qt projects, the Qt-provided compiler must be specified. (I believe creating a project using a Qt template in Visual Studio does this configuration automatically)

I believed this state was sufficient for a 1.0 release; the basic functionality laid out in the beginning was met, it included an "About" menu, was visually pleasing, and was completely usable in non-technical settings. Future versions will make refinements, possibly including:

- A couple more steps to make truly one step:
    - Default source directory to the user folder (e.g. where the Desktop, Documents, Music, etc. folders are located in Windows)
    - Default destination directory to an external drive, if any is connected
- Other refinements:
    - Ask the user to approve overwriting existing files in the destination directory with a same name as one in the source, with additional "overwrite all" and "overwrite with new name" options