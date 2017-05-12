---
layout:     post
title:      GCompris- Changes made in roman_numerals activity
date:       2017-05-12 15:50:18
summary:    GCompris- Changes made in roman_numerals activity
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

In this post, I am going to talk about the recent design changes made in the *roman_numerals* activity.

### Inspiration

The major goal of the task was to improve the following two aspects of the activity:

* The evaluation of the answer was initially calculated via a timer. This means that whenever the answers in the input field was changed, the answer was checked with the correct answer and the resultant feedback was displayed. This was inconvinient, specially if the user is not fast enough. So a definite input was needed to confirm whether the user wants their answer to be evaluated.

![before_1](../../../../../images/roman_numerals/before_1.jpg)

* Redesigning the layout of the activity to make the activity more user-friendly, as [Ilya mentioned in a blog post](https://ilyabizyaev.wordpress.com/2017/01/13/gcompris-review/)

![before_2](../../../../../images/roman_numerals/before_2.png)

### Changing the answer evaluation process

For this, the simplest solution is to remove the existing timer and replace it with an OK button to compare the answer by the user with the correct answer. 

```qml
        BarButton {
          id: ok
          source: "qrc:/gcompris/src/core/resource/bar_ok.svg";
          sourceSize.width: 60 * ApplicationInfo.ratio
          visible: true
          anchors {
              bottom: bar.top
              right: score.left
              rightMargin: 10 * ApplicationInfo.ratio
              bottomMargin: 10 * ApplicationInfo.ratio
          }
          onClicked: items.check()
        }
```

`check()` is a method under `items` which simply checks the answer by the user with the correct one ans displays the result.

```qml
            property bool isChecking: false
            function check() {
                if(isChecking) {
                    return
                }
                isChecking = true
                if( feedback.value == items.questionValue ) {
                    bonus.good('tux')
                } else {
                  bonus.bad('tux')
                }
            }
```

Along with the OK button, the `check()` method is also called the *Enter* or *Return* key is pressed, just to make things easier

```
        Keys.onPressed: {
            if ((event.key === Qt.Key_Enter) || (event.key === Qt.Key_Return)) {
                items.check()
            }
        }
```

### Changes in the overall layout of the activity

The instructions were centrally aligned and was represented in a tabular form, to make it look more user-friendly. Following this, a problem can arrive when the instruction text will need more space than allocated. For overcoming this, we use a `Flickable` to restrict the text to it's given allocated space

```qml
            Rectangle {
                color: "transparent"
                width: parent.width
                height: (background.height - (y + bar.height + ok.height + keyboard.height) * 1.1 )
                Flickable {
                    width: parent.width
                    height: parent.height
                    contentWidth: parent.width
                    contentHeight: instructionContainer.height
                    anchors.margins: 8
                    anchors.fill: parent
                    flickableDirection: Flickable.VerticalFlick
                    clip: true
                    GCText {
                        id: instruction
                        visible: items.instruction != ''
                        wrapMode: TextEdit.WordWrap
                        fontSize: tinySize
                        anchors.horizontalCenter: parent.horizontalCenter
                        width: parent.width * 0.9
                        text: items.instruction
                        horizontalAlignment: Text.AlignHCenter
                        color: 'white'
                        Rectangle {
                            id: instructionContainer
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
                }
            }
        }
```

Along with this, it was decided that the dynamic conversion was not always necessary, if the user wanted to challenge themselves. For this, a simple `Hint` was added to the `Bar`, with which the user can turn the dynamic conversion on or off

```diff
Bar {
    id: bar
    anchors.bottom: keyboard.top
-   content: BarEnumContent { value: help | home | level }
+   content: BarEnumContent { value: help | home | level | hint }
    onHelpClicked: {
        displayDialog(dialogHelp)
}
```

And just turn it on/off likewise

```
onHintClicked: feedback.visible = !feedback.visible
```

![after](../../../../../images/roman_numerals/after_1.jpg)

Overall, I was satisfied with the final result of this. The full changes can be viewed [here](https://cgit.kde.org/gcompris.git/commit/?id=0d0a6f094734ed8944ccf9bc31359397bb3007a8). Any suggestions for further improvements are always welcome.