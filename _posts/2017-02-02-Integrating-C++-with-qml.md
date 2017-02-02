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

While working with adding the odd even category in the categorization activity, I found a need to integrate the qml with a C++ class. The class contains a method which returns the list of all files in a given directory, using the [dirent](https://github.com/tronkko/dirent) header.

### The C++ code

*fileName.h*

```c++
#ifndef FILENAME_H
#define FILENAME_H

#include <dirent.h>
#include <QString>
#include <QObject>

class fileName : public QObject
{
    Q_OBJECT
public:
    fileName();
    Q_INVOKABLE QString getFiles(QString location);
};

#endif
```

*fileName.cpp*

```
ffsds 
#include "fileName.h"

#include <dirent.h>
#include <QString>
#include <string.h>
#include <QObject>

#include <QDebug>

fileName::fileName()
{
}

QString fileName::getFiles(QString location)
{
    const char *s=location.toUtf8().constData();
    DIR *dpdf=opendir(s);
    struct dirent *epdf;

    QString files;

    if(dpdf!=NULL) {
        while(epdf = readdir(dpdf)) {
            if(strcmp(epdf->d_name, ".")!=0 && strcmp(epdf->d_name,"..")) {
                // next file name = epdf->d_name
                qDebug() << epdf->d_name;
                files.append(QString::fromLocal8Bit((epdf->d_name)));
                files.append(" ");
            }
        }
    }
    return files;
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
   fileName.cpp
   fileName.h
)
```

Then, in `main.cpp`, we integrate it with the qml . For this, we do the following:

* Necessary imports: 
```c++
#include <QQmlContext>

#include "fileName.h"
```

* Then, we set the contextProperty in the engine as follows:

```c++
QScopedPointer<fileName> files(new fileName);
engine.rootContext()->setContextProperty("files", files.data());
```

### Calling from the activity

Now, with the fileName integrated, we can call the methods of the class by calling `files`, as we set the context to the engine.

```
files.getFiles("../../GCompris-qt/src/activities/categorization/resource/board/")
```

And as an output, we get the list of files present in the directory in the console

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
