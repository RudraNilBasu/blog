---
layout:     post
title:      Vim filetype templates
date:       2016-09-10 12:32:18
summary:    VIM Filetype templates
categories: vim
thumbnail: jekyll
tags:
 - vim
 - templates
---

![HEAD](https://rudranilbasublog.files.wordpress.com/2016/09/myimage.gif)

This one is just a short article to demonstrate one of the powers of autocmd, with which we can set a default template for a particular file type which will open up whenever we open a file of that type.

With the number of C++ codes I write using vim, I got tired of writing the same default headers every time I open up a new cpp file. Vim has a great solution to this problem, it allows us to save templates for a specific file type, and when we open a new file of that type, the default template will show up.

### Creating a default template for cpp file

Open up the terminal, and go to .vim folder in the home directory. Create a folder named “templates” and create the cpp file which will hold the template for cpp file type. Let’s call that ```template.cpp```

 


![scr](https://rudranilbasublog.files.wordpress.com/2016/09/screenshot-from-2016-09-10-134635.png)

Open the template.cpp file using your text editting software (vim in this case), and insert the default template you want to keep for the cpp file types.

Close it up and open your ```~/.vimrc``` file and paste the following code in it –

```
augroup templates
	autocmd!
	autocmd BufNewFile *.cpp 0r ~/.vim/templates/template.cpp
aurogroup END
```

![hue](https://rudranilbasublog.files.wordpress.com/2016/09/hue.jpg)

And that’s it! The next time you open a new cpp file, the default template will load up as shown below.

![final](https://rudranilbasublog.files.wordpress.com/2016/09/myimage.gif)

This is how my template for cpp files looks like :-

<script src="https://gist.github.com/RudraNilBasu/8b42bb48233424c3cbb54cfc58a6ca20.js"></script>