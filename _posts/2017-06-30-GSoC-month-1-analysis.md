---
layout:     post
title:      GSoC- End of first coding month- Analysis
date:       2017-06-30 12:59:00
summary:    GSoC- End of first coding month- Analysis
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

It has been one month since the coding period of 2017 Google Summer of Code officially began, and it was an awesome experience! In this post, I will look back at the work done in the last month, what went right and what went wrong.

### Useful Links

* [GSoC 2017 Proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)
* [Phabricator Task Link](https://phabricator.kde.org/T1529)
* [GSoC 2017 Status Report](https://community.kde.org/GSoC/2017/StatusReports/RudraNilBasu)
* [Development branch: gsoc_rudra_submarine](https://cgit.kde.org/gcompris.git/log/?h=gsoc_rudra_submarine)

### Pilot a submarine

The goal of the activity is to learn how a submarine works, focussing on the engines, rudders and the diving planes. The activity currently has:
* A total of 10 levels
* Physics and collision detection using [box2d](http://box2d.org/)

![collision](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/submarine/scr_4.png)

* The initial 3 levels are tutorial levels, giving an explanation of how each of the three components (engine, rudders and diving planes)

<img src="https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/submarine/scr_tutorial_1.jpg" align="left" hspace="20" />
<img src="https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/submarine/scr_tutorial_2.jpg" hspace="20" />

* Pickups, in the form of jewels like it was present in the gtk+ version

<img src="https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/submarine/scr_pickups_1.jpg" align="left" hspace="20" />
<img src="https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/submarine/scr_pickups_2.jpg" hspace="20" />

* Threats in the form of rocks in the latter levels to make the activity more challenging
* A control panel in the bottom of the screen to control the submarine, which is aimed at making simple enough for children to understand quickly

### What went right

* **The planning**: *Almost* everything went as planned (will explain the "almost" part later). The core mechanics came up pretty quickly, allowing me to move on to the polishing/bug fixing part very soon. . Polishing generally means to go through the following cycle as many times as possible:
  * Coming up with an idea
  * Conceptualising it
  * Implementation of the idea
  * Evaluation of the implemented idea

  Also, I spent quite a lot of time looking up at various resources online which helped me implement these features
* **Time Management**: Time management has been crucial during this period. I planned to implement the basic features as fast as possible to make sure that I get enough time for testing and polishing the activity. Thankfully, the implementations were completed before the scheduled date, allowing me to jump right into the bug fixing and improvement stage 

### What didn't go right

* As mentioned earlier, "almost" everything went as planned. The vertical velocity of the submarine is one exception. The vertical velocity of the submarine is affected by two components:
  - The Ballast Tanks and
  - The Diving Planes

  Implementing the Diving plane was easy, and so was implementing the Ballast tanks. But the problem arrised while integrating the above two in a single unit. There were lots of corner cases which I didn't expect initially. As of now, I settled with the following, which seems to work much better:

```
// check if the submarine is currently being operated
// under Ballast tanks or under the diving planes
if (submarine.y > 0 && submarine.velocity.x > 0 && wingsAngle != 0) {
	// currently under the influence of diving planes
} else {
	// currently under the influence of Ballast tanks
}

var targetDepth // the final depth to which the submarine should dive/rise

if (currently under diving planes) {
	// multiplier determines the depth, depending on the wings angle
	targetDepth = multiplier * maximum depth
} else {
	targetDepth = (amountOfWaterInTanks / totalCapacityOfTanks) * maximumDivingDepthOnFullTank
}

depthToMove = targetDepth - submarine.y
submarine.velocity.y = maxVerticalSpeed * (depthToMove / totalDepth)
```

### Looking forward

Currently, *Pilot a Submarine* is under testing and bug fixing / improvements stage. After that, I will move on to start working on the *Family* activity. If you are willing to try it out yourself, it is currently in the [gsoc_rudra_submarine](https://cgit.kde.org/gcompris.git/log/?h=gsoc_rudra_submarine) branch. Hope you enjoy playing around with it just a fraction of how much we enjoyed making it.
