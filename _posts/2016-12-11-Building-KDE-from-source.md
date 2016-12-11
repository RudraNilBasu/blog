---
layout:     post
title:      Starting off with KDE in Kubuntu 16.04
date:       2016-12-11 03:29:18
summary:    Build KDE from source
categories: KDE
thumbnail: jekyll
tags:
 - KDE
 - Open source
---

I was thinking about shifting from competitive programming to open source contrbution for a while now. Found KDE an interesting organisation,and decided to start with it. Little did I know that I would have to spend the next 1 week trying to debug the list of errors generated while building KDE softwares, dealing with Ubuntu crasing on boot up and finally moving from [Ubuntu 14.04](https://www.ubuntu.com/) to [Kubuntu 16.04](http://www.kubuntu.org/getkubuntu/). 

To build KDE softwares from source, we need *kdesrc-build*.

We use git to clone kdesrc-build to our local machine. You can install git using the command: 
```
sudo apt-get install git
```

Learn more about [configuring git here](https://help.github.com/articles/set-up-git/).

### Installing dependencies

* Installing Qt

Download Qt using ```wget```: 
```
wget http://download.qt.io/official_releases/qt/5.7/5.7.0/qt-opensource-linux-x64-5.7.0.run
```
Install Qt

```
chmod +x qt-opensource-linux-x64-5.7.0.run
./qt-opensource-linux-x64-5.7.0.run
```

Qt was installed in ```/opt/``` directory:
```
/opt/Qt5.7.0
```

* Install the KF5 Frameworks

For Kubuntu, the following commands were needed to run to install the dependencies

```
$ sudo apt-get build-dep qtbase5-dev

$ sudo apt-get install libbz2-dev libxslt-dev libxml2-dev shared-mime-info oxygen-icon-theme libgif-dev libvlc-dev libvlccore-dev doxygen gperf bzr libxapian-dev fontforge libgcrypt20-dev libattr1-dev network-manager-dev libgtk-3-dev xsltproc xserver-xorg-dev xserver-xorg-input-synaptics-dev libpwquality-dev modemmanager-dev libxcb-keysyms1-dev libepoxy-dev libpolkit-agent-1-dev libnm-util-dev libnm-glib-dev libegl1-mesa-dev libxcb-xkb-dev libqt5x11extras5-dev libwww-perl libxml-parser-perl libjson-perl libboost-dev libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev libarchive-dev liblmdb-dev cmake git extra-cmake-modules "libkf5.*-dev" llvm llvm-3.6 libclang-3.6-dev bison flex
```
For more, [read here](https://community.kde.org/Guidelines_and_HOWTOs/Build_from_source/Install_the_dependencies).

### Installing kdesrc-build

```
mkdir -p ~/kde/src
cd ~/kde/src
git clone kde:kdesrc-build
cd kdesrc-build
cp kdesrc-buildrc-kf5-sample ~/.kdesrc-buildrc

# Install a symlink of kdesrc-build to a location in PATH
mkdir ~/bin
ln -s $PWD/kdesrc-build ~/bin
export PATH=~/bin:$PATH
```

To make sure ```kdesrc-build``` is available in PATH whenever we open the terminal, we need to add the following line to the ```~/.bashrc```

``` export PATH=~/bin:$PATH```

### Configuring kdesrc-build

We need to edit the ```~/.kdesrc-buildrc``` and replace ```/path/to/kdesrc-build/kf5-qt5-build-include``` with ```~/kde/src/kdesrc-build/kf5-qt5-build-include```. Also, add ```ignore-kde-structure true``` and ```make-options -jN``` to the global section.

We aldo need to point *kdesrc-build* to the correct source, build, log and install directories.

```
global
  ...
  # Do not introduce an extra directory hierarchy
  ignore-kde-structure true

  # set the number of jobs to 5; this should usually
  # be (number-of-cpu-cores + 1)
  make-options      -j5

  # Stop kdesrc-build
  #stop-on-failure  true

  # Directory structure
  source-dir        ~/kde/src
  build-dir         ~/kde/build
  log-dir           ~/kde/log
  kdedir            ~/kde/usr
  ...
end global
```

### Build using kdesrc-build

For building, we just have to run: 
```
kdesrc-build <package-name>
```

I was building *kmines*, and the command is: 
```
kdesrc-build kmines
```

Successful build looks like this: 
```Updating kde-build-metadata (to branch master)
 * Downloading projects.kde.org project database...

Building kmines from kdegames (1/1)
	Updating kmines (to branch master)
	Local changes detected
	Module updated
	No source update, but the last configure failed
	Source update complete for kmines: no files affected
	Preparing build system for kmines.
	Removing files in build directory for kmines
	Old build system cleaned, starting new build system.
	Running cmake...
	Compiling... succeeded (after 15 seconds)
	Installing.. succeeded (after 1 second)

<<<  PACKAGES SUCCESSFULLY BUILT  >>>
kmines
 
Removing 4 out of 5 old log directories...
:-)
Your logs are saved in /home/rudra/kde/log/2016-12-11-04```

**NOTE**: If you are getting error messages stating files such as ```qt5-config.cmake``` was not found, then add the directory of the qt installation (in my case it is ```/opt/Qt5.7.0/5.7/gcc_64```) to the *CMakeLists.txt* file in the source code (in my case the location is ``` ~/kde/src/kmines/CMakeLists.txt```) to set the prefix path of Qt installation.
Add the following line: 
```
set (CMAKE_PREFIX_PATH "/opt/Qt5.7.0/5.7/gcc_64")
```