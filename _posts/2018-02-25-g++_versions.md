---
layout:     post
title:      Managing multiple g++ versions
date:       2018-02-25 19:58:00 - 5:30:00
summary:    Managing multiple g++ versions
categories: unix
thumbnail: jekyll
tags:
 - g++
 - unix
---

# Managing multiple g++ versions

Multiple versions of g++ are maintained by creating a symlink of the g++ version you wish to have with `/usr/bin/g++`.

```
sudo ln -s /usr/bin/g++-7 /usr/bin/g++
```
