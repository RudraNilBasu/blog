---
layout:     post
title:      Detecting Ascending Sequence
date:       2017-01-03 15:11:18
summary:    GCompris activities
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

Just completed the basic logic for Detection of ascending order. Roughly, I created a `Grid` item with few number of `Rectangle`s as `Repeators`. Each of the individual Rectangles will have unique numbers for the users to choose from.

```
Grid {
    id: grids
    rows: 1
    spacing: 12
    Repeater {
        id: boxes
        model: 4
        Rectangle {
            property int imageX: 0
            width: 360/2
            height: 360/2
            radius: 20
            property int clicked:0

            MouseArea {
                anchors.fill: parent
                onClicked :{
                    Activity.check(numText.text)
                }
            }

            GCText {
                id: numText
                anchors.centerIn: parent
                text: imageX.toString()
            }
        }
    }
}
```

(Few datas are hardcoded, it will be generalised later)

The object that is sent to the javascript code is given as follow:

```
QtObject {
    id: items
    property Item main: activity.main
    property alias background: background
    property alias bar: bar
    property alias bonus: bonus
    property alias grids: grids
    property alias boxes: boxes
    property alias ansText: ansText
    property alias ok: ok
}
```

`ansText` is a `GCText` component to display both the user's answer and the correct answer. `ok` is a button which will reload the current level if the user's answer is not the correct one.

<<<<<<< HEAD
=======
Moving on to the javascript file, the start method looks like this

```
function start(items_) {
    items = items_
    currentLevel = 0
    initLevel()
}

function initLevel() {
    items.bar.level = currentLevel + 1
    numEntered=0
    items.ansText.text=""
    initGrids()
}
```

`currentLevel` indicates the index of the level (starting from 0), and `items.bar.level` is the level number (starting from 1). `initGrids()` initialises the number of grids, based of the level number.

```
function initGrids() {
    items.boxes.model = 2*(items.bar.level)+1

    generateRandomNumbers()

    hash=[]
    entered=[]
    for(var i=0;i<items.boxes.model;i++) {
        items.boxes.itemAt(i).imageX=num[i]
        items.boxes.itemAt(i).color=initColor
        hash[i]=0
    }
}
```

First, we are setting the number of grids which should be in a given level. This is equal to `2*(levelNumber)+1` in this case. Then we generated the same number of *unique* random numbers within the given range and store it in `num[]` array. Then we traverse through each grids individually and set their text (= the number written on them) and color. We also mark it as unvisited by making `hash[i]=0`.

```
function generateRandomNumbers() {
    var n=items.boxes.model
    // generate n random numbers and store it in num[]
    num=[]
    var upperBound = (items.bar.level)*100
    while(num.length < n) {
        var randomNumber=Math.ceil(Math.random()*upperBound)
        if(num.indexOf(randomNumber) > -1) {
            continue;
        }
        num[num.length]=randomNumber
    }
}
```
>>>>>>> e4fd95f8f5a7836f18961a04af01f3764dd28d61
