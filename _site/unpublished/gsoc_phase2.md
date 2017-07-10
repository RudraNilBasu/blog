![header](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family/family_header.jpg)

### Improving layout of GCompris's Family activity

Everything was well planned for a great start for the second phase of GSoC and I anticipated everything to go smoothly since I wouldn't have any college exams to worry about. Soon enough, I was proven wrong almost immediately by an incoming health issue, which took a lot of working hours from my schedule. Anyways, I started out with the family activity along with continuing with the bug fixes and improvements on the submarine activity.

My goal for this week was to convert the current layout of the family activity into a much more intuitive tree-like representation, where the family members of the same generation would lie in the same vertical layer. As an example, I created a mockup of a currently present level, which looks like the following:

![plan_1](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family/family.png)

and planned to represent it in the following manner:

![plan_2](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family/family_test_2.png)

### The Implementation

To start things of, firstly I moved the entire dataset from `family.js` into a separate `Dataset.qml` file. It can be viewed in [this commit](https://cgit.kde.org/gcompris.git/commit/?h=GSoC-family&id=2877e0adfccabf050bec1cfd1ee33aad6f578ded). The `Dataset.qml` roughly looks like the following:

```qml
QtObject {
    property real nodeWidth: background.nodeWidthRatio
    property real nodeHeight: background.nodeHeightRatio
    property var levelElements: [
                    // level 1
                    {  edgeList: [
                                   [0.37, 0.25, 0.64, 0.25],
                                   [0.51, 0.25, 0.51, 0.50]
                                ],
                       nodePositions: [
                               [0.211, 0.20],
                               [0.633, 0.20],
                               [0.40, 0.50]
                       ],
                       captions: [ [0.27, 0.57],
                                  [0.101, 0.25]
                                ],
                       nodeleave: ["man3.svg", "lady2.svg", "boy1.svg"],
                       currentstate: ["activeTo", "deactive", "active"],
                       edgeState:["married","others"],
                       answer: [qsTr("Father")],
                       optionss: [qsTr("Father"), qsTr("Grandfather"), qsTr("Uncle")]

                    },
                    // level 2
                    {
                    	...
                    },
                    ...
    ]
}

```

`nodeWidth` and `nodeHeight` is used to keep track of the width and height of a single node (all nodes are of same dimension), which are useful for drawing edges accurately from/to the end of a specific node. Followed by this, we have an array called `levelElements` which stores the `(x,y)` values of each nodes, `(x1, y1) and (x2, y2)` values of each edges (endpoints) and the properties of each nodes and edges.

From this, I decided to improve on the 9th and 10th levels of the activity, which looked like the following:
![family_9](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family/fam_1.png)

My goal was to connect the edges from the bottom of the node instead of connecting them horizontally. For that, instead of using fully brute-forced numbers for the edges, I used the x-y coordinates of the nodes and used the width and height values of the nodes to accurately find the start/end positions of the edges. This, however, leads to a problem: since the value of `nodeWidth` and `nodeHeight` changes with the change in width and height of the activity, the dataset needs to be reloaded whenever there is a change in screen resolution, in order to avoid some incorrect start/end positions of the edges.

```qml
		onWidthChanged: loadDatasetDelay.start()
        onHeightChanged: if (!loadDatasetDelay.running) {
                            loadDatasetDelay.start()
                         }
		/*
         * Adding a delay before reloading the datasets
         * needed for fast width / height changes
         */
        Timer {
            id: loadDatasetDelay
            running: false
            repeat: false
            interval: 100
            onTriggered: Activity.loadDatasets()
        }
```

Using these, the 9th and 10th level turned out to be like this:
![family_9_final](https://raw.githubusercontent.com/RudraNilBasu/blog/gh-pages/images/gsoc/family/fam_2.png)

### Moving forward

For the upcoming weeks, I plan to continue on improving the layouts of the activity. Along with that, I will look forward to improve the implementation of the activity to a Grid based layout, in which instead of hardcoding the x-y coordinate values of the nodes, we will be creating a `Grid` element and the datasets will contain the (row, column) values of the nodes and the (r1, c1) to (r2, c2) values for the edges.

```