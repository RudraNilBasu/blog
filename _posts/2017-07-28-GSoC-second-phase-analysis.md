---
layout:     post
title:      GSoC Second phase analysis
date:       2017-07-28 21:20:00 - 5:30:00
summary:    GSoC Second phase analysis
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

![header]()

Hello everyone, with the second evaluation results just few ~~hours~~ minutes away, I am going to look back at the whole month in general, to reflect on what I have done, and what I should've done.

The month took a rough start with me getting ill from the very start of the month. I decided to get the relatively "easy" job done within the first month, so that I can fully focus on the topics that require much more concentration once I start recovering. I started out by [refactoring the code and using nodeWidth and nodeHeight]({{ site.baseurl }}/kde/2017/07/10/GSoC-phase2-week-1/) and keeping the same generation node within the same vertical level. Also, I made sure that the changes get reflected with change in the dimensions of the activity.

The progress for the first week was slow (though I didn't expect it to go too well either), but by the end of the week, I had a clear idea on what to do next, and keeping up with the plans and an improving health kept my heads up.

The next few weeks went much better than I expected. I started out with [implementing the Grid layout](({{ site.baseurl }}/kde/2017/07/19/family-grid-wise-layout/)) that I had planned the past weekend and it worked wonderfully well, with minimal errors, to my surprise. It also turned out that using the specific layout along with the width and height of each node allowed the nodes and edges to fit perfectly well (more on that on the specified blog post).

![img]({{ site.baseurl }}/images/gsoc/family/diffs/level_6/after_1.jpg)

Having completed the original *Family* activity, I quickly switched on to [implementing the family_find_relatives]({{ site.baseurl }}/kde/2017/07/27/family-find-relatives/) activity. The implementation looked quick, I realised that there was a major bug in the implementation, that there can be more than one answer for a particular level (which made the overall activity much more interesting, though). I had to redo most of the implementation, but in the end it was worth it.

With the end of the third week, I had plenty of time to test both of the activities, and in doing so, I realised that the pair matching activity became very predictable once the *Family* activity was completed. So after discussing with my mentors, we decided to randomise the levels by shuffling it every time the activity is loaded. The end result was very satisfactory.

![result]({{ site.baseurl }}/images/gsoc/family/find_relatives/result.gif)

# In conclusion...

Overall, I am happy with how the activities turned out. Roughly 2/3 of the total project is complete, and I am glad that inspite of a rough start, I managed to keep up to date with my proposed timeline.

### Relevant links

* [GSoC Proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)
* [Phabricator task](https://phabricator.kde.org/T6096)
* [Development branch](https://cgit.kde.org/gcompris.git/log/?h=GSoC-family)
* [KDE Status Report](https://community.kde.org/GSoC/2017/StatusReports/RudraNilBasu)


