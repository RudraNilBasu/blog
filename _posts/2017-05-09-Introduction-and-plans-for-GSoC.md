---
layout:     post
title:      Introduction and plans for GSoC
date:       2017-05-09 07:38:18
summary:    Introduction and plans for GSoC
categories: kde
thumbnail: gsoc
tags:
 - kde
 - gcompris
 - gsoc
---

### Introduction

Hi, I am Rudra Nil Basu, a third year CS undergrad from West Bengal, India. I have been contributing to GCompris for several months by adding features and improving layouts of existing activities, which got me familiar to the codebase of the project, which further helped me to work on my own "ordering activity": where one has to arrange a given set of numbers or alphabets in increasing or decreasing order. Of course, none of these would have been possible without the constant help from the community. Along with that, I decided to apply for this year's Google Summer of Code and finally got selected.

### About GCompris and GSoC

[GCompris](http://gcompris.net/index-en.html) is a high-quality educational suite which aims at making learning easier for children aged 2 to 10. GCompris currently has 137 activities on various topics such as science, maths, games with which it has successfully
created a great learning environment for children.

![gcompris](http://gcompris.net/screenshots_qt/small/root.png)

Given below is a list of categories present in GCompris and few activities belonging to that category:


* **computer discovery**: keyboard, mouse, different mouse gestures, ...
* **arithmetic**: table memory, enumeration, double entry table, mirror image, ...
* **science**: the canal lock, the water cycle, the submarine, electric simulation ...
* **geography**: place the country on the map
* **games**: chess, memory, connect 4, oware, sudoku ...
* **reading**: reading practice
* **other**: learn to tell time, puzzle of famous paintings, vector drawing, cartoon making, ...


However, there are few activities which were started previously but is not yet completed. I strongly believe in what GCompris stands for and during the GSoC period, I aim at taking GCompris one step forward by finishing three started activities.

[![gsoc_logo](https://developers.google.com/open-source/gsoc/resources/downloads/GSoC-logo-horizontal-800.png)](https://developers.google.com/open-source/gsoc/resources/downloads/GSoC2017Presentation.pdf)

[Link to GSoC proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)

The best way to learn something is by demonstration. But, it is not always possible to physically demonstrate every real world objects. To overcome that, simulation of real world objects is a useful solution. Keeping this in mind, I aim at creating simulation of real world scenarios to help children understand the working of submarines and digital circuit for this year's Google Summer of Code.

### Plans for GSoC period

My task for the GSoC period is to [finish three started activities](https://summerofcode.withgoogle.com/projects/#6332354560786432) in GCompris: *Pilot a Submarine* , *Family* and *Digital Electricity*. I will also be giving updates of my work at least weekly on my [blog](http://rudranilbasu.github.io/blog/).

* **Pilot a Submarine**: This is an activity to teach how a submarine works, which was originally present in the gtk version of GCompris. It will be built from scratch, with the initial levels containing tutorials explaining how a submarine works and the utilities of different working elements. The latter levels will focus on using the ideas from the tutorial levels to check whether the child has fully understood the mechanism or not.

![sub](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/submarine.png)

* **Family**: This is an activity is aimed to help children understand how they are related to their relatives. I will be improving the design of the activity to a tree like representation where the members belonging to the same generation will be in the same level of the tree.

![family_initial](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family.png)

*Current layout of the activity*

![family_final](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family_test_2.png)

*Expected layout of the activity*

I will also extend the activity in which given a pair, the user will have to choose the correct relation.

* **Digital Electricity**: The third and final activity is aimed at providing a real time simulation of an electric circuit. A *"free mode"* already exists, where the children can use any components to create a circuit on their own. I will be implementing levels which will help to teach the children how the individual components work in a circuit so that the activity will be easier to understand for someone who didn't have any prior experience with a specific component.

![digital](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/digital_level_4.png)

*Mockup of one of the levels in the Digital Electricity activity*

I will be providing more detailed progress on the implementation of these activities in my later blog posts. Looking forward to an exciting summer ahead!

### Contact Information

* **Freenode IRC nick**: rudra
* **Email address**: rudra.nil.basu.1996@gmail.com
* **Twitter**: [RudraNilBasu](https://twitter.com/RudraNilBasu)