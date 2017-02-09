---
layout:     post
title:      GCompris - Number sequence - addition of dots
date:       2017-02-08 15:11:18
summary:    Number sequence - addition of dots
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

[Commit Link]()


This is a very simple problem I noticed while examining the *drawletter* and *drawnumber* activities in GCompris. The problem arives when one clicks on a node, but no line needs to be drawn (1. when it is the first node 2. when the pen is drawn away from the page (think of letter '*B*') ). In this case the node just seems to "disappear" without giving any form of visual effect.

![pic1]()
![pic2]()

For this problem, I thought of an efficient solution which would enhance readability of the code and solve the problem at the same time. First I thought of adding a dot whenever a node was clicked, so that it will get covered whenever the line is drawn. But, it was then observed that the center of the line which was drawn does not pass through the center of the line between the two nodes. As a result of this, the dot won't be *"covered"* when the line is drawn over them.

So, the above idea needed to be drawn and I settled with drawing the dot whenever a line cannot be drawn, a black dot is made on that point.

### Code

*NumberSequence.qml*

First, we add the black point image (named *blackpoint.svg* ), then add the condition for the node as follows: 

```qml

Image {
    id: pointImage
    source: Activity.url + (highlight ?
    (pointImageOpacity ? "bluepoint.svg" : "bluepointHighlight.svg") :
     markedAsPoint ? "blackpoint.svg" : "greenpoint.svg")
    ...
}

```

*number_sequence.js*

In the `drawSegment(pointIndex)` function, we check whether we have to draw the point or not, and if we have to, we place the point on that node.

```javascript

// if we need to draw only a point instead of a line
if(mode == "drawletters" || mode == "drawnumbers") {
    items.pointImageRepeater.itemAt(pointIndex).highlight = false
    if(pointIndex>0) {
        items.pointImageRepeater.itemAt(pointIndex-1).opacity = 0
        items.pointImageRepeater.itemAt(pointIndex-1).markedAsPoint = false
    }
    /* draw a point in case of -
        1) First node
        2) No line is to be drawn between the current node and last node
    */
    var isPointMarked = false
    if(pointIndex == 0 || (pointPositions2 && pointPositions2[pointIndex] != pointPositions2[pointIndex-1])) {
        if(pointIndex<items.pointImageRepeater.count-1) {
            items.pointImageRepeater.itemAt(pointIndex).markedAsPoint = true
            items.pointImageRepeater.itemAt(pointIndex).scale = 0.2
            isPointMarked = true
        }
    }
    if(!isPointMarked) {
        items.pointImageRepeater.itemAt(pointIndex).opacity = 0
    }
} else {
    items.pointImageRepeater.itemAt(pointIndex).opacity = 0
}

```
