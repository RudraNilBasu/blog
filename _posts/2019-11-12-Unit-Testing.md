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
https://twitter.com/Jonathan_Blow/status/1194289482469502976

https://threadreaderapp.com/thread/1194078100885164032.html
https://threadreaderapp.com/thread/1194289482469502976.html

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

-------------------

One more thing to say, both about test-driven development and about languages that are "too static". 

When you're building something that is complicated, and is new, you won't know exactly what you are building. If you make a meticulous plan in advance, this plan will turn out to be wrong and will have to change; it can serve as an aid to team communication, but can't be taken 
too seriously or else it will prevent you from getting to the goal. You don't know the details of what the goal looks like until you explore the territory around the goal; early in development, you aren't trying to build the final thing, you're just visiting the neighborhood 
to see whether it's a good neighborhood, and which house you might want to buy.

This phase is critically important; it's how you build expertise. If you are naive about a new topic (and this is a new topic if it is not extremely similar to something you've built before), you can't make seasoned decisions about what is right. So your decisions will be 
at least somewhat wrong. So the goal here is to iterate and experiment and gain experience so that you can then make well-informed decisions that get you to the final product. 

It's also like rough-drafting a novel or storyboarding a film; you want to be able to lay things out in a non-finished way, so that you can see kind of what it looks like, and make big decisions that improve the thing structurally, without paying a big cost to do so. 

If the cost is too big, it will limit the amount of rough-drafting or storyboarding you can do within your time/money budget, which limits how much expertise you can build and how close you can get to the right answer, before you are forced to go in and solidify the final thing.

If you say I should do "test-driven design" with lots and lots of unit tests, you're saying that the way to write each piece of code is to take it very seriously, not only doing the base work required to make that function, but all this extra work to test it, and solidify it, 
as soon as it exists. You're telling me I can't rough-draft or storyboard, instead I should be trying to draw finished cels of my animated movie from day one. The cost of changes is very high, so I won't end up changing very much, and I can't iterate the structure of the whole 
solution without tremendous cost, so I won't do it. But I'll think I am being a Good Programmer because on the surface I am being very robust about making sure this API works, and these implementations work. The probelm is, it's the wrong API and wrong implementations. 

This is also how I feel about languages that I think are "too static" like Rust. If I am just rough-drafting, I don't care if things are quite correct. Let me leak memory right now, it doesn't matter, I will probably throw away this procedure 4 times anyway; when I get to the 
5th and final one, *then* let's make sure it doesn't leak memory. If you force me to be too correct up front, you remove the possibility of low-cost experimentation. 

I think that software quality today is very poor, and correctness-proving systems are key to fixing this. I think the idea that Rust has that you should have annotations in your program, that ensure whatever dimensions of correctness are possible to ensure, is a good one.

But the flaw in Rust's approach is that I am forced to do this from day one (and forced to pay for it with time on every compilation). Let me experiment, let me gain expertise in this domain, then when it's time to get serious, let me layer in these checks. 

(In theory you can mark an entire Rust program 'unsafe' and slowly remove this over time, but the language doesn't seem to really want you to do this, and the all-or-nothing mechanics are impractical. Let me layer in one check at a time, build it up.) 

I don't mean to only address Rust here, it's just a concrete example of this class of languages. If you let me iterate and experiment, but then also provide me tools with which to ensure my program is correct, but I don't have to pay the costs of this on day one, that's great.

Iteration speed is very important for productivity. There's a big difference between compilation being instant, and taking 5 seconds. There's a big difference between 5 seconds and a minute. There's a big difference between 1 minute and 20 minutes. 

If you have been paying 20 minutes every compilation for years, it's likely you no longer have a feel for what you have lost. If you get back to 1 minute, that'd be amazing. But also the person who was at 1 minute person no longer has a feel for what they have lost... !

Computers today are amazingly fast. They should be able to compile programs almost instantly, for any reasonable program size. If they aren't doing that, it's the sign of a problem. 
