---
layout:     post
title:      Installing Vim airline using Pathogen (in Ubuntu)
date:       2016-12-24 18:01:18
summary:    Install vim airline
categories: vim
thumbnail: jekyll
tags:
 - vim
 - pathogen
---

[vim-airline](https://github.com/vim-airline/vim-airline) is a "lean and mean status/tabline for vim that's light as air"

![demo](https://raw.githubusercontent.com/wiki/vim-airline/vim-airline/screenshots/demo.gif)

### Installation

Goto the bundle directory, or create one if it is not already created at `.vim/bundle`

```
cd ~/.vim/bundle
```

clone vim-airline from the github repository

```
git clone https://github.com/vim-airline/vim-airline
```

To allow the plugin to open up as soon as a file is started, add this to the `~/.vimrc` file.

```
set laststatus=2
```

Add the following line to make sure to get the colors to work correctly

```
set t_Co=256
```
