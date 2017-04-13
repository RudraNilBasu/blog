---
layout:     post
title:      GCompris- Ascending order
date:       2017-02-12 15:11:18
summary:    Adding Ascending order in GCompris
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

<!--
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

![pic2](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/GCompris/ascending_activity_1.png)

# Working

<iframe width="560" height="315" src="https://www.youtube.com/embed/gSPcWVt0R0s" frameborder="0" allowfullscreen></iframe>

With new drag and drop

The Final Version involve dragging and dropping the tiles in it's correct position and shifting the remaining tiles.

<iframe width="560" height="315" src="https://www.youtube.com/embed/wx3GQltFn4A" frameborder="0" allowfullscreen></iframe>

-->

# The QML Code

In the beginning, we declare a variable called `mode` to identify which mode we are currently in. As if now, there are two modes: 1. Numbers and 2. Alphabets. We are going to talk about alphabets shortly after, right now, let's concentrate on the numbers.

For the numbers activity, we set the `mode` variable to `number"`

```
property string mode: "number"
```

We then create a `QtObject` which will store all the QML items we need in the javascript. It looks like the following:

```
        QtObject {
            id: items
            property Item main: activity.main
            property alias background: background
            property alias bar: bar
            property alias bonus: bonus
            property alias boxes: boxes
            property alias flow: flow
            property alias container: container
            property alias instruction: instruction
            property real ratio: ApplicationInfo.ratio
            property alias score: score
        }
```

* `background`: The `Image` component which contains the background image for the activity
* `boxes`: The `Repeater` component which contains a specific number of `Rectangle` components which serves as the individual boxes for the activity.
* `flow`: The [`Flow`](http://doc.qt.io/qt-5/qml-qtquick-flow.html) component, which is responsible for positioning the Rectangles side by side
* `container`: The `Rectangle` component which defines the area under which the `flow` should be present
* `instruction`: A `GCText` component containing the instructions for each levels
* `score`: A `score` component to display the score for the current level

### The instructions

The instructions is a `GCText` component which displays whether the elements for the current level should be arranged in ascending or descending order. The width of this component should be a little less than the width of it's parent and it should be anchored along with the horizontalCenter of it's parent. The text should also get wrapped accordingly, if it doesn't fit in the current screen.

For creating a background of the text, we create a `Rectangle` element under it, with the gradient starting from #000 to #666 to #AAA

```
        GCText {
            id: instruction
            wrapMode: TextEdit.WordWrap
            fontSize: tinySize
            horizontalAlignment: Text.Center
            anchors.horizontalCenter: parent.horizontalCenter
            width: parent.width * 0.9
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

### The items

For the items, we first have an invisible container which will contain the `flow` and `boxes`. It is a transparent Rectangle, centered in the center of the screen. The width depends on the number of Rectangles in the level. This is done to make sure that the children of the `flow` element are always at the center of the screen. The width is given by the formula:

```
width: Math.min(parent.width, ((boxes.itemAt(0)).width*boxes.model)+(boxes.model-1)*flow.spacing)
```

As the child of this transparent Rectangle is the `Flow`, with two variables, `onGoingAnimationCount` and `validMousePress`:

* `onGoingAnimationCount`: Counts the number of on-going animations in the current level. No input will be taken as valid if the value is != 0
* `validMousePress`: A boolean variable to determine whether we can make a valid input or not (we can't give input if there are any ongoing tasks)

As the child of the `flow`, we have a `Repeater` element which will repeat few instances of a `Rectangle` type, which will be our blocks which we drag and drop in the activity.

The blocks are a white `Rectangle` of width and height `65 * ApplicationInfo.ratio` with a black border. As a child of this rectangle is a `GCText`, which is the label (number or alphabet) on the blocks.

For the purpose of drag and drop, we have a `MouseArea` which has `onPressed` and `onReleased` to deal with the drag and drop

```
                            onPressed: {
                                box.beginDragPosition = Qt.point(box.x, box.y)
                            }
                            onReleased: {
                                Activity.placeBlock(box, box.beginDragPosition);
                            }
```

The `beginDragPosition` is a `point` variable under `box` and `placeBlock()` is a function on the javascript, which we will discuss shortly

There are two animation component, which plays whenever there is a change in `x` and `y` positions in the blocks

```
        Rectangle {
            id: container
            color: "transparent"
            width: Math.min(parent.width, ((boxes.itemAt(0)).width*boxes.model)+(boxes.model-1)*flow.spacing)
            height: parent.height/2

            anchors {
                horizontalCenter: parent.horizontalCenter
                verticalCenter: parent.verticalCenter
            }

            Flow {
                id: flow
                spacing: 10

                /*
                 * Count number of active animations in the activity
                 * at this current time
                 * (No input will be taken at this time)
                 */
                property int onGoingAnimationCount: 0
                property bool validMousePress
                anchors {
                    fill: parent
                }
                Repeater {
                    id: boxes
                    model: 6
                    Rectangle {
                        id: box
                        color: "white"
                        z: mouseArea.drag.active ||  mouseArea.pressed ? 2 : 1
                        property int boxValue: 0
                        property point beginDragPosition

                        width: 65 * ApplicationInfo.ratio
                        height: 65 * ApplicationInfo.ratio
                        radius: 10
                        border{
                            color: "black"
                            width: 3 * ApplicationInfo.ratio
                        }
                        GCText {
                            id: numText
                            anchors.centerIn: parent
                            text: mode == "alphabets" ? String.fromCharCode(boxValue) : boxValue.toString()
                            font.pointSize: 20 * ApplicationInfo.ratio
                        }
                        MouseArea {
                            id: mouseArea
                            anchors.fill: parent
                            drag.target: parent
                            enabled: (flow.onGoingAnimationCount == 0 && flow.validMousePress == true) ? true : false
                            onPressed: {
                                box.beginDragPosition = Qt.point(box.x, box.y)
                            }
                            onReleased: {
                                Activity.placeBlock(box, box.beginDragPosition);
                            }
                        }
                        Behavior on x {
                            PropertyAnimation {
                                id: animationX
                                duration: 500
                                easing.type: Easing.InOutBack
                                onRunningChanged: {
                                    if(animationX.running) {
                                        flow.onGoingAnimationCount++
                                    } else {
                                        flow.onGoingAnimationCount--
                                    }
                                }
                            }
                        }
                        Behavior on y {
                            PropertyAnimation {
                                id: animationY
                                duration: 500
                                easing.type: Easing.InOutBack
                                onRunningChanged: {
                                    if(animationY.running) {
                                        flow.onGoingAnimationCount++
                                    } else {
                                        flow.onGoingAnimationCount--
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
```

For the score for each levels we use the `Score` component in-built in GCompris.

```
        Score {
            id: score
            anchors {
                right: parent.right
                top: instruction.bottom
                bottom: undefined
            }
        }
```

We now move on to the javascript part, which mainly deals with the mechanism to shift the positions of the blocks on drag and drop.

# The javascript code

### The variables

```
var currentLevel = 0
var numberOfLevel = 4

var items
var mode

// num[] will contain the random numbers
var num = []
var originalArrangement = []

var ascendingOrder
var thresholdDistance
var noOfTilesInPreviousLevel
```

* `currentLevel`: The current level
* `numberOfLevel`: The number of levels
* `items`: The `QtObject` containing the QML objects
* `mode`: The variable to determine the current mode of the activity ( `numbers` or `alphabets` )
* `num`: An array containing the random numbers
* `originalArrangement`: The original arrangement provided to the user
* `thresholdDistance`: The minimum distance from the drop area to the closest block to allow shifting of the blocks
* `noOfTilesInPreviousLevel`: To store the number of blocks in the previous level, used to reset variables
