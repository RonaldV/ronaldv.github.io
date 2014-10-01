---
layout: post
title: Thoughts on BDD
comments: true
categories: [BDD, development]
---

I had been reading up on BDD a little while ago. Which stands for Behavior Driven Development and is initially an improvement on TDD. Or better said an improvement on the way to think about TDD. It’s not as much a completely new way of writing tests as an eye opener on how to correctly write tests. Instead of writing tests you’re writing behaviors or specifications of behaviors. The idea of behaviors is much more intuitive then tests.

I mean when I first started to write tests, I had a hard time to get into it. I had a lot of questions about where to start writing tests, how much to test, what was the best way to name your tests and so on… If I had known about BDD back then I would have had a much easier time to get into TDD. Because by only just thinking about it as behaviors it answers some of the questions by itself.

But this post is not an introduction into BDD as there are a lot of good resources on the net about it already.

For example

* [Introduction to BDD](http://dannorth.net/introducing-bdd) by the man who dreamed up BDD Mr. Dan North himself.
* [BDD](http://www.code-magazine.com/article.aspx?quickid=0805061) by Scott Bellware a big promoter of BDD in .NET land.

What I wanted to talk to you about are in my opinion a few misconceptions about the subject. I have seen people say “Wohow what is this? There are multiple ways to do BDD.”. And yes that is partially correct, because Dan North’s views on it have grown to encompass a much larger idea then just testing. A part of it contains analysis as wel and in such the tests have changed to reflect these new views. 

If you have read the introductory by Dan North or sat at a presentation about BDD you would know that BDD takes a bigger part of Agile software developement then just tests. Among other it forms an ubiquitous language where the “tests” form an extention of it.

On the other hand there is Dave Astels who took the former ideas of BDD that only focus on improving TDD. This is the so called Dave Astels version of BDD that is also known as Context/Specification so not to be confused with BDD. 

These aren’t mutually exclusive ideas, one is just a subset of the other.

You can use the full BDD for writing acceptance tests through stories or use the subset for state/interaction testing. As for the difference in testing framework… wel that’s just like every framework it’s a matter of choice and opinion.

On another note, when writing specifications (or tests) for a library you can use common developer language like dictionary or thread in the naming if it is absolutely necessary to understand what the specification is doing. Because the group you’re targeting are developers I think it is allowed to use programming terminology.

On the other hand I think it is best to keep it at a minimum to make the specifications more easily readable. Also when your specifications are going to be read by non-programmers it is best to prohibit such terminology.