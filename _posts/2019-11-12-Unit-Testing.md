---
layout:     post
title:      Unit testing
date:       2019-11-12 12:32:18
summary:    Unit Testing
categories: Programming
thumbnail: jekyll
tags:
 - unit-testing
 - programming
---


https://twitter.com/Jonathan_Blow/status/1194078100885164032?s=09
https://threadreaderapp.com/thread/1194078100885164032.html

---------

Because so many people are confused when I say I don't like unit tests very much, I'll try to explain why.


A large part of what I am objecting to is the dogma attached to the phrase "unit testing": that you should write many tests for each piece of functionality you program, that those tests determine the API and "keep it stable", that they are things you automatically run to ensure your system keeps working. These tests usually comprise a large amount of code -- some multiple of the actual code that implements the functionality you are trying to produce. (At some companies this ratio is 10x or higher).

The tests take a long time to write, and a long time 
to run, and you run them a lot, e.g. before each check-in. So the total cost of writing, maintaining, and constantly running these tests is very high. You want to make sure you are getting enough benefit to offset that cost. 
(There are additional costs I haven't mentioned that are quite high -- for example, the more code you have, the bigger the momentum of the ship you are trying to steer. If all those unit tests are expecting your API to be a certain way, it is a giant pain in the butt to change it to the thing that you discover to be better later on, which happens 100% of the time. Most people won't bother. If they do bother, they go through a soul-killing process and will end up tired and lacking energy to actually program real code.) 


So what are the benefits? Well, most of the benefits of unit testing specifically, such as "ease of refactoring", are being evangelized by people working in the so-called "dynamic" languages, where making a typo in a variable name means your program has broken, and type errors are only detected at runtime. The better solution: use a language that doesn't have these problems, and you'll recover all that benefit, plus much much more. (It is worth pointing out that many of the people evangelizing unit tests for these purposes don't even realize that these things are non-problems in serious programming languages, which gives you a clue as to whether their advice is based on deep and thorough experience).


The rest of the benefit of "unit testing" is also given by just standard vanilla software testing, as people have done for decades, long before Hacker News dudes heard of terms like "unit testing" and "DevOps" and decided they were cool. 
So yes, test your software, absolutely. If there are convenient tests that you can put down by leaf code, with which you achieve a high benefit to cost ratio, by all means go for it. But the "unit testing" dogma encourages you do high-cost things without self-awareness. 


But there's a deeper reason why unit testing is not so good, and it has to do with the basic nature of software. 
When you combine pieces, in the simplest case they can all line up in a row, so that if you put N things together, the resulting software is complexity N. But this is very, very rare. The nature of our universe is that things like to interact with each other and spiral off into complexity, and this is true with software just as with the physical world. So if you put together N software components, you will get something closer to N-squared. Using good engineering principles, we can try to keep the exponent down close to 1, but very often the basic nature of the problem we are solving, and the basic nature of the machines we run on, conspire to push that exponent upward. Let's say we keep the exponent to 1.6 (pretty good!), and N is 15. pow(15, 1.6) is 76. Think about this. 


If you unit tested all your N pieces, very thoroughly, you pay a very high cost to reduce the bug rate of 15 units of your program's complexity. But there are 76 units total. What about the other 61 units? What are you doing about those? Quite possibly nothing, depending. So unit testing *doesn't even address the majority of the bug problem*, because the exponent is almost always significantly greater than 1. (In some programs, like games, the exponent is inherently close to 2 and there is nothing you can do about it). 


So unit testing is one of these techniques that doesn't pass the smell test. Whenever people are promoting it heavily, I can tell that they haven't shipped real-world, large, complex software, at a productivity level that I would find acceptable. They probably don't even program in languages I would find acceptable. 


Also, to remove further ambiguity: I think testing is critically important. If you do not test your program, it doesn’t work. If you don’t test it doing a specific thing, it won’t work doing that thing. But, some ways of testing are dramatically more efficient than others.
