---
layout: post
title: Wrap up: "Deep Learning for coders" by fast.ai
date: 2020-01-16
---

A few weeks ago I finished the Practical Deep Learning for Coders MOOC, by fast.ai. I really found it amazing: it is very well designed and at the end you get a very good overview of what you can do with DL as well as a good amount of practice. This post is a summary of what I liked.

## Top-Down approach
I heard about this “fast.ai approach” from a few people that had already taken the course and honestly it’s one of the aspects that stoked my curiosity. Moreover Jeremy Howard (the main teacher in the course, with the help of Rachel Thomas) spends some time in the introductory lesson talking about it. Using their own words:
> We’ll discuss the overall approach of the course, which is somewhat unusual in being top-down rather than bottom-up. So rather than starting with theory, and only getting to practical applications later, we start instead with practical applications, and then gradually dig deeper and deeper into them, learning the theory as needed. 

As an example, in the very first lesson they show you how to (and make you) train a model (to recognize dog and cats, of course) and starting from there in the following lessons you iterate over and over, every time expanding a little bit, and therefore touching all the various usual topics needed to understand what is going on.

This is kind of a big change in my usual way of learning things: my own attitude (maybe fostered with my education as a physicist) is to always start from the basics and then scale up. This proved to be a very effective method when I was a student as the lessons at the University were structured in this bottom-up fashion, but I found it hard to apply in other environments where the pace is much faster and you are expected to deliver quickly, e.g. at work. I often ended up with a very detailed understanding of the basics, but being unable to solve a real problem :\
Following the top-down approach they propose instead is very practical and really gives you the “OK this is doable” feeling, instead of the “Oh, I really have a lot to study before being able to do this”. I want to stress that they do not suggest that things are easy or trivial by neglecting the study needed to really own the topic, but you really get started with something working and the underlying complexity is gradually unveiled at every subsequent round.
I am definitely taking this top-down approach with me!

## Further research
At the end of each chapter, in addition to the couple dozen questions aimed at fixing some key concepts, there are some longer term assignments called “further research”. These are extremely useful since they immediately pushed me to go beyond simply redoing the example in the lesson from scratch. The goal is to really make you get your hands dirty and get a bit out of your comfort zone. As an example, the introductory chapter on computer vision is based around the MNIST dataset, of course. Interestingly - and very smartly - the initial focus is on only two digits, i.e. the problem is reduced to distinguishing 3s from 7s. One of the assignments in the “Further research” section is to scale the model up to a full digit recognizer, and in order to do that you have to of course start from everything that was shown in the chapter, but also make your own research and experimentation. Btw, while doing this assignment I found out something that I felt was so worth sharing that pushed me to start this blog :).
When studying specific topics in the past I often struggled to find a reasonably-sized personal project (or side project or whatever you want to call it) which was big and complex enough to be challenging but which was at the same time small enough to be realistically doable. People at fast.ai really managed to find the right balance with the “further research” assignments (and I can imagine how much work they put into finding this balance)!

## Forum
One of the great resources for the course (and for the “Further research” section) is the fast.ai forum: it includes discussions about the lessons and the book as well as discussions from users who use fast.ai for their own projects and post their questions. I had a number of interesting exchanges over there and I found many useful info spanning from theoretical questions to way more practical ones, e.g. why is my code crashing with this weird error?
Always compare against a baseline
Say you built a model to work on some data and in the end you get 87% accuracy. Is it good or bad? Here is one of the many good tricks of the course which is not strictly related to Deep Learning (or Machine Learning in general): you always need a baseline to compare against. Again, using their own words:
> [A baseline is] a simple model which you are confident should perform reasonably well. It should be very simple to implement, and very easy to test, so that you can then test each of your improved ideas, and make sure they are always better than your baseline. Without starting with a sensible baseline, it is very difficult to know whether your super-fancy models are actually any good.

Nothing really mind blowing here, but a really good piece of advice to include in my usual work: you don’t want to spend time and resources building something super fancy if the gain on a way simpler approach isn’t that much.

## Put things into perspective
One of the strengths of this course is that it is not only about getting you started with DL from the strictly technical point of view, but it also touches on a number of different related topics which really make you understand that being able to build and train (and …) a model is not the whole story: from being sure you are really getting the right kind of data to the pitfalls a model can face when deployed in the real world, from strengths and weaknesses of ML to being able to think about your model as a complete usable product (they make you set up a simple web application which uses a model you just built, I found it brilliant). In this context it is worth mentioning that a full chapter is dedicated to data ethics, including an impressive collection of examples of how models can reflect and perpetrate discriminations and biases (Garry Kasparov has a very interesting point of view on this topic).
All of this to say that in this course it is made very clear that “from great power comes great responsibility”. DL is an extremely powerful tool, and guess who has the responsibility to understand if, how and when to use it?

## Conclusions
Summarizing, people at fast.ai managed to set up an excellent Deep Learning MOOC. What makes it excellent is that while teaching one of the most powerful tools that we have it also gives the feeling that learning how to use is only a part of being a good practitioner in the field. If you are looking for something to get started with Deep Learning, this is where you should start.