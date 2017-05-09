---
layout:     post
title:      Introduction and plans for GSoC
date:       2017-05-09 07:38:18
summary:    Introduction and plans for GSoC
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
 - gsoc
---

### Introduction

Hi, I am Rudra Nil Basu, a third year CS undergrad from West Bengal, India. I have been contributing to GCompris for several months by adding features and improving layouts of existing activities, which got me familiar to the codebase of the project, which further helped me to work on my own "ordering activity": where one has to arrange a given set of numbers or alphabets in increasing or decreasing order. Of course, none of these would have been possible without the constant help from the community. Along with that, I decided to apply for this year's Google Summer of Code and finally got selected.

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