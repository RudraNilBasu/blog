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

