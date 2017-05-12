---
layout:     post
title:      Adding custom feed in jekyll
date:       2017-05-08 15:11:18
summary:    Add custom feed for specific topics in jekyll
categories: jekyll
thumbnail: jekyll
tags:
 - jekyll
---

This is a short post to show how we can add a custom feed for a specific topic in a jekyll blog.

For creating a blog on planet kde, I needed to provide a custom feed just for kde on my blog

The procedure is very simple. Every jekyll blog comes with a `feed.xml` file, which is to show all the posts in the blog. For my blog, the contents of the file are:

If we look closely, the following line is where the solution to our problem lies:

```
for post in site.posts limit:10 
```

This is to filter out all the posts with limitation set to change. To filter out posts with specific tags, first we create a duplicate `.xml` file (let's call it `feed.kde.xml`) and change the following line:

```
for post in site.posts limit:10
```

to 

```
for post in site.tags["kde"]
```

And now, if we open the new xml file from our browser (which is `rudranilbasu.me/blog/feed.kde.xml` in my case), it will filter out only the posts with `kde` tags in it.


[Source Link](https://devblog.dymel.pl/2017/02/09/category-rss-feed-in-jekyll/)
