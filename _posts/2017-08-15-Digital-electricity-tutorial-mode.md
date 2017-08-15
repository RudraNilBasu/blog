---
layout:     post
title:      GCompris- Digital Electricity Tutorial mode
date:       2017-08-15 22:15:00 - 5:30:00
summary:    GCompris- Digital Electricity Tutorial mode
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

![header]({{ site.baseurl }}/images/gsoc/digital_electricity/digital_electricity_tutorial_mode.png)

The last week was mostly spent in creating more tutorial levels, optimising the tutorial dataset, ability to create wires in the playArea for tutorial mode when the level loads and checking the correctness of the answer provided by the user.

# The Dataset

The dataset for the tutorial mode is now updated to the following:


```
{
                    inputComponentList: [zero, one],
                    playAreaComponentList: [orGate, digitalLight],
                    determiningComponentsIndex: [1],
                    wires: [ [0, 0, 1, 0] ],
                    playAreaComponentPositionX: [0.4, 0.6],
                    playAreaComponentPositionY: [0.3, 0.3],
                    introMessage: [
                            qsTr("The OR Gate produces an output of 1 when at least one of the input terminal is of value 1"),
                            qsTr("Turn the Digital light on using an OR gate and the inputs provided")
                    ]
}
```

* The `inputComponentList` denotes the items that are provided to the user and can be used any number of times. It is present in the `ListWidget` component.
* `playAreaComponentList` denotes the items which are provided to the user in the `playArea`. It's position can be changed, but it cannot be deleted.
* The `wires` is an array denoting the wires which should be pre-defined in the `playArea`, it is defined in the following manner:
  ```
  [from_component_index, from_component_terminal_number, to_component_index, to_component_terminal_number]

  from_component_index: The index of the component from the playAreaComponentList, from which the wire is to be drawn
  from_component_terminal_number: The terminal number of the above component from which the wire is to be drawn
  to_component_index: The index of the component from the playAreaComponentList, to which the wire is to be drawn
  to_component_terminal_number: The terminal number of the above component to which the wire is to be drawn
  ```
* `playAreaComponentPositionX/Y`: The x/y position in the `playArea` where the component is to placed
* `introMessage`: The message which is to be shown in the beginning of the tutorial level. If no message is required, it is kept blank.

# Making playArea components indestructible

The components in the `playArea` is made indestructible by adding a property `destructible` (bool) to the electrical components, and the `MouseArea` is enabled via:

```
MouseArea {
        ...
        enabled: destructible
        ...
}
```

This property is disabled for the `playArea` components, by adding:

```
"destructible": false
```

while creating.

# Adding wires

Pre-defined wires in the `playArea` is added by traversing the `wires[]` array and creating wires accordingly by calling the `createWire(from_component, to_component, destructible)` method.

```
        // creating wires
        for (i = 0; i < levelProperties.wires.length; i++) {
            var terminal_number = levelProperties.wires[i][1]
            var outTerminal = components[levelProperties.wires[i][0]].outputTerminals.itemAt(terminal_number)

            terminal_number = levelProperties.wires[i][3]
            var inTerminal = components[levelProperties.wires[i][2]].inputTerminals.itemAt(terminal_number)

            createWire(inTerminal, outTerminal, false)
        }
```

Similar to the previous topic `destructible` decides whether the wire can be deleted by the "delete" tool or not, which is set to false for `playArea` wires.

# Checking answers

The answer checking process is divided into two parts:
* The levels which only check if the bulb is on or not
* The levels which asks the user to create a circuit so that the bulb glows only under certain conditions

The first case is very easy, which is achieved by checking the value of the bulb when the OK button is clicked.

```
        if (determiningComponents[0].inputTerminals.itemAt(0).value == 1)
            items.bonus.good('tux')
        else
            items.bonus.bad('tux')
```

For the second case, we traverse through all the possible scenario for the input, check it with the output whether it passes the test. If the configuration passes all the tests, the answer is declared as correct, else it is marked as incorrect:

```
        var digitalLight = determiningComponents[2]
        var switch1 = determiningComponents[0]
        var switch2 = determiningComponents[1]

        var switch1InitialState = switch1.imgSrc
        var switch2InitialState = switch2.imgSrc

        for (var A = 0; A <= 1; A++) {
            for (var B = 0; B <= 1; B++) {
                switch1.imgSrc = A == 1 ? "switchOn.svg" : "switchOff.svg"
                switch2.imgSrc = B == 1 ? "switchOn.svg" : "switchOff.svg"

                updateComponent(switch1.index)
                updateComponent(switch2.index)

                var operationResult = A ^ B

                if (operationResult != digitalLight.inputTerminals.itemAt(0).value) {
                    switch1.imgSrc = switch1InitialState
                    switch2.imgSrc = switch2InitialState
                    updateComponent(switch1.index)
                    updateComponent(switch2.index)
                    items.bonus.bad('tux')
                    return
                }
            }
        }
        items.bonus.good('tux')
```

# Future plans

For the next week, my plans are roughly:
* Create more levels, with a proper difficulty curve for each of the components provided.
* Add an option for "hint", so that the user can seek help when they are stuck.

### Relevant links

* [GSoC Proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)
* [Phabricator task](https://phabricator.kde.org/T1524)
* [Development branch](https://cgit.kde.org/gcompris.git/log/?h=gsoc_pulkit_digital_electricity)
* [KDE Status Report](https://community.kde.org/GSoC/2017/StatusReports/RudraNilBasu)


