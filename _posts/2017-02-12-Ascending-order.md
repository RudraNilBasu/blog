---
layout:     post
title:      Ascending order
date:       2017-02-12 15:11:18
summary:    Adding Ascending order in GCompris
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

### Original Idea

![pic1](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/GCompris/ascending.png)

Key points: 

* Few blocks, with same width and different heights are given to the user as input
* The user will then have to arrange the blocks in increasing order of their heights, thus creating a stair to help tux move up the stairs to reach the door.

### Revised Idea

The previous idea was scrapped off since the new one can be used in multiple ways. The new one looks like: 

* Blocks containing numbers will be present in one line
* User will have to rearrange them to a specific order (ascending order in this case)

### Workflow

We use `Flow` to arrange the blocks (as `Rectangle` in this case), so that it automatically wraps out depending on the screen size.
