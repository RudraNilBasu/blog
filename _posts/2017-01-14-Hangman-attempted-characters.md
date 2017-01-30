---
layout:     post
title:      Hangman - Attempted Characters
date:       2017-01-14 15:11:18
summary:    GCompris Hangman
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

First merge always feels good! I was working on adding this feature for the last few days, and it is done finally.

[Commit Link on Github](https://github.com/gcompris/GCompris-qt/commit/8ab75acf49431c685021f3cd0e58cf31f3fa4568)

### Task

To add a Textfield to display the characters which were attempted by the user in the Hangman activity.

![pic]()

### Explanation

*Hangman.qml*

```
GCText {
    id: guessedText
    fontSize: smallSize
    color: "#FFFFFF"
    wrapMode: Text.WordWrap
    horizontalAlignment: Text.AlignHCenter
    width: parent.width - 2*clock.width
    anchors {
        horizontalCenter: parent.horizontalCenter
    }
    z: 12
}
```

The textfield which displays the characters which are attempted by the user. The color of the text is white ( `#FFFFFF` ), and the width is `width of the screen - 2*(width of the clock)` to avoid the text and the clock to get overlapped.

```
Rectangle {
    width: guessedText.width
    height: guessedText.height
    radius: 10
    border.width: 1
    gradient: Gradient {
        GradientStop { position: 0.0; color: "#000" }
        GradientStop { position: 0.9; color: "#666" }
        GradientStop { position: 1.0; color: "#AAA" }
    }
    anchors {
        horizontalCenter: parent.horizontalCenter
    }
    z: 11
}
```

This is the Rectangle that will be under the text. A gradient is given to this Rectangle from black (top ) to a lighter color (bottom).

Also, we need to make sure that the hint image doesn't get overlapped with the text. For that, we change the width of the imageframe to:

```
Item {
        id: imageframe
        width: Math.min(300 * ApplicationInfo.ratio,
        background.width * 0.8,
        hidden.y) - guessedText.height
```


*Hangman.js*

In the javascript file, we create the logic to generate the text in the following way:

```
function createAttemptedText() {
    alreadyTypedLetters.sort()
    items.guessedText.text = qsTr("Attempted: %1").arg(alreadyTypedLetters.join(", "))
}
```

`alreadyTypedLetters` is an array that stores the characters that are typed by the user. We are using `qsTr()` to enable translation for the given text to various other languages.

To make it work, we call `createAttemptedText()` from:

* `initSubLevel()` function, right after declaring `alreadyTypedLetters` array. At this point, the array will be empty, so we will have only the header in the text field.
* `processKeyPress(text)` function, write after the newly typed character is pushed into the alreadyTypedLetters array. Thus, whenever a new charater is typed by the user, `createAttemptedText()` will be called. 