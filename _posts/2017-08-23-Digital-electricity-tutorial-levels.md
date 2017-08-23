---
layout:     post
title:      GCompris- Digital Electricity Tutorial levels
date:       2017-08-23 20:18:00 - 5:30:00
summary:    GCompris- Digital Electricity Tutorial levels
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

![header]({{ site.baseurl }}/images/gsoc/digital_electricity/digital_electricity_tutorial_mode.png)

In one of my previous posts, I talked about creating tutorial level properties on the dataset and importing it in the js to add the level. In this post, I will be going through the design process of the levels in the tutorial mode of the digital_electricity activity of GCompris.

Designing of the levels was less about creating some arbitary puzzles, rather, it is more about allowing the user to discover things that already exist.

The levels are of three types:
* Easy: Mainly aimed at introducing the users to a new component
* Medium: Using the introduced components, the user is asked to solve a particular problem.
* Challenging: 1 or 2 levels are aimed at this level, whose goal is to allow the user to discover certain facts about the components or the digital world itself.

The final job as a designer of the problems, was to make sure that the ramifications of the changes are explored to the fullest, via adding components/wires making immune to deletion and to allow the player to provide a setup to discover the underlying facts of the components by themselves.

As an example, one of the levels require the user to light the bulb only when both of the switches are turned on. Since all the basic gates are provided, the user will very easily use an AND gate for the problem.

On one of the later problems, the exact same problem is provided, but only this time, the sole gate provided is the NAND gate, forcing the user to find a solution to it with that specific gate only, along with teaching them how conversion of basic gates to universal gates work.

> "A puzzle is not about the puzzle. It is a communication of an idea between the designer and the user. Solving the puzzle is the player's way of saying "I Understand". "

# Future plans

In an unfortunate turn of events, I (yet again :( ) fell sick coming into the final week on GSoC, the updates may get a bit slow, but they are roughly as follows:
* Completing movement on the playArea via swipe for mobile devices.
* Finding a solution to represent the "Tools" icons, so that they do not appear to slow for mobile devices.
* Few other fixes/improvements.
* Writting a report on the final month of GSoC (and the whole 3 month period as a whole)

### Relevant links

* [GSoC Proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)
* [Phabricator task](https://phabricator.kde.org/T1524)
* [Development branch](https://cgit.kde.org/gcompris.git/log/?h=gsoc_pulkit_digital_electricity)
* [KDE Status Report](https://community.kde.org/GSoC/2017/StatusReports/RudraNilBasu)


