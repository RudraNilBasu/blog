---
layout:     post
title:      GCompris - Odd even activity
date:       2017-02-01 15:11:18
summary:    Adding Odd-Even category in GCompris
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

[Commit Link](https://github.com/gcompris/GCompris-qt/commit/db7a4a9b743a3521c7c68f0b2b54719cbc9582db)

The main purpose of this was to add the odd even category to the categorization activity. It seemed to be a pretty straightforward task, but one issue lead to the other and I learned a lot more than what I thought previously.

In the original activity, all the categories were stored in the `qrc:/gcompris/src/activities/categorization/resource/board/` and were named as `categoryxx.qml` were x = 1 to 16, and the number of categories were hardcoded in the activity. My work mainly included: 

* adding the odd-even category
* To make sure that importing the categories dataset is not hardcoded

Adding the odd-even category is pretty simple. We just need to create the dataset, and the background js and qml will take care of it. For the category, I made 6 levels, the first level is just one digit numbers, 2nd level is 2 digit and 3rd level is 3 digit. The next 3 levels is combinations of these, and I chose not to add too many levels to it, since the only thing that matters here is the last digit.

The second point is the most important part. We had a basic idea that importing the datasets should be independent of their names. So, I created a simple C++ class in the `core` that contains a method that would take in a link to a directory and will return the names of all the files in it. I will talk about creating the C++ class and integrating it with qml in the next blog post. For this blog, we consider that we have a class `Directory`, and a method `QStringList getFiles(Qstring)` that takes in the location as `QString` and returns the list of all files as `QStringList`. We also added a variable to each of the datasets, a boolean to check if the dataset is a demo level or not (via the `isEmbedded` variable).

We send the files from the qml to the js via the items as :

```qml
property string datasetURL: ":/gcompris/src/activities/categorization/resource/board/"


QtObject {
	id: items
	property Item main: activity.main
	....
	property var categories: directory.getFiles(datasetURL)
}


Directory {
     id: directory
}
```

In the js, in the start() method, we store all the categories in the `categoryList[]` array first, and store only the ones we need for now (if we are in demo mode, we need only the demo once).

```javascipt

var categoryLists = []
categoryLists = items.categories
var isEmbeddedMode = items.file.exists(fileName) ? true : false

for(var i = 0; i < categoryLists.length; i++) {
  categoriesFilename = boardsUrl + "board" + "/" + categoryLists[i]
  items.categoryReview.categoryDataset.source = categoriesFilename

  if(isEmbeddedMode || (items.categoryReview.categoryDataset.item).isEmbedded ) {
      categoriesData.push(items.categoryReview.categoryDataset.item)
  }
}

```

And that is it. Now, we can add any dataset files in the dataset folder, and it will be imported in the activity.
