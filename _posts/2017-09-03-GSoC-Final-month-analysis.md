---
layout:     post
title:      GSoC- Final month analysis
date:       2017-09-03 21:51:00 - 5:30:00
summary:    GSoC- Final month analysis
categories: kde
thumbnail: kde
tags:
 - kde
 - gcompris
---

![header]({{ site.baseurl }}/images/gsoc/gsoc_logo.png)

So, the final month of GSoC just wrapped up, and in this post I will be talking about the last month, the implementation of tutorial mode for the Digital Electricity activity.

I started off the month with separating the two modes of the activity, with the help of an additional `config` option in the bar.

![config]({{ site.baseurl }}/images/gsoc/digital_electricity/modes.png)

Whenever the selected value is changed, the current mode is saved in `dataToSave["modes"]` using the `saveData` signal, to ensure that the user can start of with the mode in which they left off, as the current mode is set to the value stored in the `dataToSave["modes"]` variable whenever the activity is loaded.

Next, I moved on to create a dataset which will essentially contain some values which is to be provided to the activity for every level, with the goal of removing redundancy in the code. It mainly consists of:
* a list of all the components present in the activity
* data required for each of the tutorial levels

Creating and importing data from the dataset went really smoothly, using which pre-loaded components were placed for the tutorial levels. The pre-loaded components (the electrical components and the wires) were made immune to deletion by disabling the `MouseArea` component.

![tuts]({{ site.baseurl }}/images/gsoc/digital_electricity/tutorial_elements.png)

By the next week, I started implementing levels with various types, maintaining a constant difficulty curve. Regarding the levels, instead of keeping the difficulty of the levels increase gradually, I thought of varying the difficulty curve, so that once the user has solved a hard puzzle, they are rewarded with a *relatively* easy one.

![graph_2]({{ site.baseurl }}/images/gsoc/digital_electricity/graph_2.jpg)

*Constant increase in difficulty*

![graph_1]({{ site.baseurl }}/images/gsoc/digital_electricity/graph_1.jpg)

*Rewarding the user with an easy level for a difficult one*

By this time, the biggest task that remained on the checklist was comparing the user's answers and checking the correctness. We came up with a number of approaches to solve the problem and ended up with the following one, with the goal of keeping a good balance between readability, optimisation and low redundancy:
* The levels are divided into few similar parts:
  * lightTheBulb: The levels with the goal to light the given bulb
  * equation1Variable: The levels which asks the user to solve a puzzle based on an equation with 1 variable
  * equation2Variables: Solving a similar equation using 2 variables
  * equation3Variables: Solving a similar equation using 3 variables
  * others: Special levels, which needs to be dealt with separately

Implementing the first type was very easy, we just needed to check the values of the bulb when the "OK" button was pressed and display the results accordingly.

For the levels requiring the player to solve a specific equation, the general algorithm was:

```
- Store the current state of the switches
- Loop through all the possible input scenarios
  - for each input combinations, check the answer via result() function
  - if the expected answer and current answer are not the same, restore previous state and return
- display correct answer and move on to the next level
```

The `result()` function is present in the dataset for each levels of "equationXVariables" type, which takes the input as arguments and returns the calculated result specific to that level. As an example:

```
result: function(A, B, C) {
        return A | (B & C)
}
```

The final task in the todo list was to come up with a way to access the toolbars for devices with small screen size, which was too small to be targetted correctly as shown below:

![toolbar_initial]({{ site.baseurl }}/images/gsoc/digital_electricity/toolbar_initial.png)

The solution we came up was, to replace the four icons with one single "Tools" button, on clicking which will bring up a menu of toolbar options. The goal of this implementation was to keep a balance between ease of accecibility and number of inputs required to access a specific tool. The final result was something like this:

![toolbar_final]({{ site.baseurl }}/images/gsoc/digital_electricity/toolbar_final.png)

On further testing, we thought that it would be nice to have an option of zooming in and out of the playArea, in order to allow the player to create and test bigger circuits. The zooming was implemented via multiplying the width, height and the position of each component by `currentZoom` component. For moving around the area, we take in the player input and move the components relative to it, providing the following result:

![toolbar_final]({{ site.baseurl }}/images/gsoc/digital_electricity/zoom.gif)

### What is remaining?

Besides from finding and fixing bugs, the only major task that remains ahead for the activity is to ensure the traversal along the playArea in touch screen devices as well, which currently only supports the input via arrow keys. The idea as of now, is to implement swipe along the playArea as the source of input. Will post an update once the implementation starts rolling out.

---

This concludes the overview of the final month of GSoC, it was a fun ride and I will be back with another post covering the whole GSoC experience in general in the coming week.

### Relevant links

* [GSoC Proposal](http://rudranilbasu.me/docs/gsoc_2017_proposal.pdf)
* [Phabricator task](https://phabricator.kde.org/T1524)
* [Development branch](https://cgit.kde.org/gcompris.git/log/?h=gsoc_pulkit_digital_electricity)
* [KDE Status Report](https://community.kde.org/GSoC/2017/StatusReports/RudraNilBasu)


