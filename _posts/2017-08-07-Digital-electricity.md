---
layout:     post
title:      GCompris- Digital Electricity
date:       2017-08-07 20:30:00 - 5:30:00
summary:    GCompris- Digital Electricity
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

![header]({{ site.baseurl }}/images/gsoc/digital_electricity/digital_electricity_header.png)

# Overview

The *Digital Electricity* activity in GCompris aims at creating and simulating a digital electric schema. Currently, there exists a *"Free Mode"*, where a user can freely create and check the working of a circuit on their own. During the final month of the GSoC period, I will be adding a *"Tutorial Mode"* alongside the existing *free mode*. The tutorial mode is aimed at teaching the users how the individual components work in a digital circuit.

![scr_1]({{ site.baseurl }}/images/gsoc/digital_electricity/scr_1.png)

# Implementation

### Interchanging modes in the activity

Interchanging between the "Free Mode" and "Tutorial Mode" can be done using the `config` option present in the bar.The `config` option will provide a `GCComboBox` from which the user can select the mode they want the activity to run on. 

```
               Item {
                    property alias modesComboBox: modesComboBox

                    property var availableModes: [
                        { "text": qsTr("Tutorial Mode"), "value": "tutorial" },
                        { "text": qsTr("Free Mode"), "value": "free" },
                    ]

                    Flow {
                        id: flow
                        spacing: 5
                        width: dialogActivityConfig.width
                        GCComboBox {
                            id: modesComboBox
                            model: availableModes
                            background: dialogActivityConfig
                            label: qsTr("Select your Mode")
                        }
                    }
                }
```

Whenever the choosed option is changed, the option will be saved in the `dataToSave` variable whenever the `saveData` signal is released:

```
if (newMode !== activity.mode) {
    activity.mode = newMode;
    dataToSave = {"modes": activity.mode};
    Activity.reset()
}
```

Whenever the activity is loaded and the `loadData` signal is released, the `activity.mode` variable (which stores the current mode of the activity) assumes the value present in `dataToSave["modes"]`

During the *Free Mode*, there will only be one level, due to which the "level" bit is turned off when the activity is in the tutorial mode:

```
 content: BarEnumContent { value: help | home | ( activity.isTutorialMode ? level : 0) | reload | config}
```

### Loading tutorial levels from the dataset

#### The dataset

We maintain a `Dataset.qml` file to store the details of each of the tutorial levels. The general structure of a dataset file contains: 
* Definition of each components, containing the basic informations such as image name, source, width, height and information about the component. It typically looks like:

```
    property var andGate: {
        'imageName': 'gateAnd.svg',
        'componentSource': 'AndGate.qml',
        'width': 0.15,
        'height': 0.12,
        'toolTipText': qsTr("AND gate")
    }
```
* Definition of each tutorial levels, containing the list of the above components required for each levels stored in the `tutorialLevels` array

#### Loading from the dataset

Loading from the dataset for each tutorial levels is done by traversing the `tutorialLevels[currentLevel]` elements and create the respective elements dynamically and appending it in the `availablePieces` Item (if they are input components) or dynamically created using Qt.createcomponent() (if they are needed to be inserted to the play area directly):

```
        var levelProperties = items.tutorialDataset.tutorialLevels[currentLevel - 1]
        for (var i = 0; i < levelProperties.totalInputComponents; i++) {
            items.availablePieces.model.append( {
                "imgName": levelProperties.imageName[i],
                "componentSrc": levelProperties.componentSource[i],
                "imgWidth": levelProperties.imgWidth[i] * sizeMultiplier,
                "imgHeight": levelProperties.imgHeight[i] * sizeMultiplier,
                "toolTipText": levelProperties.toolTipText[i]
            })
        }

        for (var i = levelProperties.totalInputComponents; i < levelProperties.imageName.length; i++) {
            var index = components.length
            var staticElectricalComponent = Qt.createComponent("qrc:/gcompris/src/activities/digital_electricity/components/" + levelProperties.componentSource[i])
            components[index] = staticElectricalComponent.createObject(
                        items.playArea, {
                          "index": index,
                          "posX": levelProperties.playAreaComponentPositionX[i],
                          "posY": levelProperties.playAreaComponentPositionY[i],
                          "imgSrc": levelProperties.imageName[i],
                          "toolTipTxt": levelProperties.toolTipText[i],
                          "imgWidth": levelProperties.imgWidth[i],
                          "imgHeight": levelProperties.imgHeight[i]
                        });
        }
```

# Future plans

For the next week, my plans are roughly:
* Creating the logic for checking the answers in the tutorial mode
* Implementing wires which will be connected to few of the components in the play area
* Adding more levels and electrical components

### Relevant links

* [GSoC Proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)
* [Phabricator task](https://phabricator.kde.org/T1524)
* [Development branch](https://cgit.kde.org/gcompris.git/log/?h=gsoc_pulkit_digital_electricity)
* [KDE Status Report](https://community.kde.org/GSoC/2017/StatusReports/RudraNilBasu)


