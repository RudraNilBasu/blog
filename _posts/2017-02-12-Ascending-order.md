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
  * As of now, the rearrangement is done by clicking on a block, and then clicking on the target block, resulting in them being swapped.


Inspiration - [Ordering Game](http://www.mathsisfun.com/numbers/ordering-game.php)

### Workflow

We use `Flow` to arrange the blocks (as `Rectangle` in this case), so that it automatically wraps out depending on the screen size.

*Imports*

```qml

import "ascending_order.js" as Activity
import "qrc:/gcompris/src/core/core.js" as Core

```

*The Grids*

```qml

Rectangle {
    color: "transparent"
    width: parent.width; height: parent.height/2

    anchors {
        horizontalCenter: parent.horizontalCenter
        verticalCenter: parent.verticalCenter
    }

    Flow {
        anchors.fill: parent
        anchors.margins: 4
        spacing: 10
        Repeater {
            id: boxes
            model: 6
            Rectangle {
                id: box
                color: selected ? "lightblue" : "white"
                property bool selected: false
                property int imageX: 0
                property int pos
                property bool animateVert: false
                property bool animateHor: false
                property real currentPos
                width: 360/4 * ApplicationInfo.ratio
                height: 360/4 * ApplicationInfo.ratio
                radius: 10
                border{
                    color: "black"
                    width: 10
                }
                GCText {
                    id: numText
                    anchors.centerIn: parent
                    text: imageX.toString()
                }
                MouseArea {
                    anchors.fill: parent
                    onClicked :{
                        Activity.selectBox(box);
                    }
                }
                Behavior on color {
                    PropertyAnimation {
                        duration: 300
                        easing.type: Easing.InOutBack
                    }
                }
                Behavior on x {
                    ParallelAnimation {
                        PropertyAnimation {
                            duration: 500
                            easing.type: Easing.InOutBack
                        }
                    }
                }
                Behavior on y {
                    ParallelAnimation {
                        PropertyAnimation {
                            duration: 500
                            easing.type: Easing.InOutBack
                        }
                    }
                }
                Behavior on animateVert {
                    SequentialAnimation {
                        PropertyAnimation {
                            target: box
                            property: "y"
                            from: currentPos
                            to: currentPos + (box.pos == 1 ? 20 : -20)
                            duration: 250
                        }
                        PropertyAnimation {
                            target: box
                            property: "y"
                            from: currentPos + (box.pos == 1 ? 20 : -20)
                            to: currentPos
                            duration: 250
                        }
                    }
                }
                Behavior on animateHor {
                    SequentialAnimation {
                        PropertyAnimation {
                            target: box
                            property: "x"
                            from: currentPos
                            to: currentPos + (box.pos == 1 ? 20 : -20)
                            duration: 250
                        }
                        PropertyAnimation {
                            target: box
                            property: "x"
                            from: currentPos + (box.pos == 1 ? 20 : -20)
                            to: currentPos//-box.pos == 1 ? 20 : -20//0
                            duration: 250
                        }
                    }
                }
            }
        }
    }
}

```

So this is just a basic `Flow` created within a `Rectangle` (the main container). Inside the flow there is a `Repeater` containing the `Rectangle`s which will form the blocks in the activity. Each of the `Rectangle`s has a `GCText` which contains the numbers which is sort of a "_label_" for each block. Each of these `Rectangles` (the ones insider the `Repeater`) conotains an id _box_ and 6 variables. They are: 

property bool selected: false
                property int imageX: 0
                property int pos
                property bool animateVert: false
                property bool animateHor: false
                property real currentPos

* `selected` (`bool`): which tells whether the given Rectangle is selected by the user or not.
* `imageX` (`int`): the value of the given Rectangle (or the label on it).
* `pos` (`int`): a marker used during animating the blocks to determine whether the block will move up or down while animating.
* `animateVert` and `animateHor` (`bool`): To mark whether the block should move horizontally or vertically while it's position is being interchanged with another block.
* `currentPos` (`real`): To contain the current x or y position of the block.

Along with that, we have the instruction text: 

```qml

GCText {
    id: instruction
    wrapMode: TextEdit.WordWrap
    fontSize: tinySize
    anchors.horizontalCenter: parent.horizontalCenter
    text: "Arrange the given numbers in ascending order"
    color: 'white'
    Rectangle {
        z: -1
        opacity: 0.8
        gradient: Gradient {
            GradientStop { position: 0.0; color: "#000" }
            GradientStop { position: 0.9; color: "#666" }
            GradientStop { position: 1.0; color: "#AAA" }
        }
        radius: 10
        border.color: 'black'
        border.width: 1
        anchors.centerIn: parent
        width: parent.width * 1.1
        height: parent.contentHeight
    }
}

```

... and the _ok_ button:

```qml

BarButton {
  id: ok
  source: "qrc:/gcompris/src/core/resource/bar_ok.svg";
  sourceSize.width: 75 * ApplicationInfo.ratio
  visible: true
  anchors {
      right: parent.right
      bottom: parent.bottom
      bottomMargin: 10 * ApplicationInfo.ratio
      rightMargin: 10 * ApplicationInfo.ratio
  }
  onClicked: Activity.checkOrder()
}

```

`selectBox(box)` and `checkOrder()` are two functions present in `Activity`, which is the javascript which handles the backend tasks, and it is described later.

With this, the activity will look like this: 

![pic2]()
