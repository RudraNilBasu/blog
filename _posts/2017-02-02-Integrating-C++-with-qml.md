---
layout:     post
title:      GCompris - Integrating C++ with qml
date:       2017-02-02 15:11:18
summary:    Integrating C++ with qml
categories: KDE
thumbnail: jekyll
tags:
 - kde
 - gcompris
---

While working with adding the odd even category in the categorization activity, I found a need to integrate the qml with a C++ class. The class contains a method which returns the list of all files in a given directory, using ~~the [dirent](https://github.com/tronkko/dirent) header~~ QDir.

### The C++ code

*Directory.h*

```c++
#ifndef DIRECTORY_H
#define DIRECTORY_H

#include <QString>
#include <QStringList>
#include <QObject>

/**
 * @class Directory
 * @short A helper component to get the names of files
 *        present in a given location
 * Use - call files.getFiles(":input/path/")
 */

class Directory : public QObject
{
    Q_OBJECT

public:
    /**
     * Constructor
    */
    Directory();

    /**
      * Returns the names of all the files in a given path
      *
      * @param location : The path of the directory
      *
      * @returns Names of all the files present in the
      *          given location, separated by a space
      */
    Q_INVOKABLE QStringList getFiles(const QString& location);
    static void init();

};

#endif

```

*Directory.cpp*

```c++
#include "Directory.h"

#include <QString>
#include <QStringList>
#include <QObject>
#include <QDir>
#include <QQmlComponent>

Directory::Directory()
{
}

QStringList Directory::getFiles(const QString& location)
{
    QStringList fileNames;
    QDir dir(location);
    QFileInfoList files=dir.entryInfoList();
    foreach (QFileInfo file, files){
        if (file.isDir()){
        } else{
            // if it is a file
            fileNames.append(file.fileName());
        }
    }
    return fileNames;
}

void Directory::init()
{
    qmlRegisterType<Directory>("GCompris", 1, 0, "Directory");
}
```
The above two files are placed in `gcompris/src/core` and are added in `CMakeLists.txt`, as follows: 

```
set(gcompris_SRCS
   ActivityInfo.cpp
   ActivityInfo.h
   ActivityInfoTree.cpp
   ActivityInfoTree.h
   ApplicationInfo.cpp
   ApplicationInfo.h
   ApplicationSettings.cpp
   ApplicationSettings.h
   File.cpp
   File.h
   DownloadManager.cpp
   DownloadManager.h
   Directory.cpp
   Directory.h
)
```

From here, there are two ways to approach this: 

### Procedure #1

Then, in `main.cpp`, we integrate it with the qml . For this, we do the following:

* Necessary imports: 


```c++

#include <QQmlContext>

#include "fileName.h"

```

* Then, we set the contextProperty in the engine as follows:

Added Header - `#include <QQmlContext>`


```

engine.rootContext()->setContextProperty("files", new Directory);

```

### Calling from the activity

Now, with the fileName integrated, we can call the methods of the class by calling `files`, as we set the context to the engine.

```
files.getFiles(":/gcompris/src/activities/categorization/resource/board/")
```

### Procedure #2

In this case, the `main.qml` will only contain the call to the `init()` function we declared on `Directory`:

```c++

Directory::init();

```

On the qml, we create an `id` for Directory as follows: 

```qml
Directory {
	id: directory
}
```

And we call the function using the following way:

```
directory.getFiles(":/gcompris/src/activities/categorization/resource/board/")
```

### Output

And as an output, for both the cases, we get the list of files present in the directory in the console

```
category12.qml
category4.qml
category7.qml
category16.qml
category8.qml
category19.qml
category2.qml
category11.qml
category14.qml
category5.qml
category15.qml
category13.qml
category17.qml
category18.qml
category3.qml
category9.qml
category1.qml
category6.qml
category10.qml
```

For more, check out this video (for procedure #1) : 

<iframe width="560" height="315" src="https://www.youtube.com/embed/CR2qQebqv6I?list=PLfQnJyNyt15FrjkBl6zXwKyvrH2sOFKuI" frameborder="0" allowfullscreen></iframe>
