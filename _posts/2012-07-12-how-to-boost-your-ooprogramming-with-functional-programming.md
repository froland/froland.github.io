---
id: 83
title: How to boost your object-oriented programming with functional programming
date: 2012-07-12T08:45:48+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=83
permalink: /?p=83
categories:
  - BruJUG conference
  - Java
tags:
  - Functional programming
  - Object-oriented programming
---
This is my summary of a <a title="Brussels JUG website" href="http://www.brussels-jug.be/" target="_blank">BruJUG</a> conference given bar Uberto Barbini about &#8220;How to boost your object-oriented programming with functional programming&#8221;.

This is maybe the first time I disliked a conference this year. Everything that Uberto said is true and relevant to the subject. But having read the title and the introduction, I expected something more practical on how to include functional programming concepts in my daily object-oriented experience. Instead of that, I got a strong advice to study functional programming and some good best practices about immutable objects and avoiding side-effects.

Anyway, I&#8217;ll write what I remember of that presentation, at least because talking in front of specialists is an uneasy exercise that deserves some attention and respect.

### Introduction

Let&#8217;s start with two interesting questions:

  1. What if object-oriented programming was wrong?
  2. Why does code quality matters?

<span style="font-size:14px;line-height:23px;">Even if those questions may seem awkward at first sight, it is worth thinking about it and go beyond the standard answers that were hammered inside our heads while we were learning programming.</span>

<span style="font-size:14px;line-height:23px;">Bugs are inevitable at some point of our development. They cause frustration and delays. Many are easy to fix ans some can cause big troubles. </span>A good code is thus not a code without bugs but rather a code where bugs are easy to find and to fix.

From the presenter point of view, worst bugs are often related to the state. I totally agree with that. So one solution is to defend your state like a castle so that state effects are limited to the object.

But even these days, even if we say or even sometimes think, we are still writing a lot of procedural code that is, code that describe a story like &#8220;do this, then do that, if the result is like this, do this other action&#8221; and so on. Take a look in a presentation layer based on Struts and chances are you&#8217;ll see what I mean.

Some framework seem to induce procedural paradigm which:

  * is easy to write
  * is hard to understand
  * often involves a global state

### <span style="font-size:14px;line-height:23px;">Paradigms history</span>

Lisp is one of the first language to implement the functional paradigm and was published in the early 1960&#8217;s. In the early 1970&#8217;s, Smalltalk introduces the object-oriented paradigm.

But what is object-oriented programming?

  * &#8220;It looks like a record with fields and methods plus keywords to define the scope.&#8221;
  * &#8220;Object-oriented programming is an exceptionally bad idea which could only have originated in California.&#8221; Edsger Dijkstra
  * &#8220;OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things. It can be done in Smalltalk and in LISP. There are possibly other systems in which this is possible, but I&#8217;m not aware of them.&#8221; Alan Kay
  * &#8220;An object has _state_, _behavior_, and _identity_.&#8221; Grady Booch

<span style="font-size:14px;line-height:23px;">The idea of messaging if very present in Smalltalk itself.</span>

A big problem is that a lot of programmers think they do good object-oriented programming because they use a lot of interfaces and design patterns.

To get some good advice about how to design an object-oriented application, one can read &#8220;<a title="Domain-Driven Design book on Amazon" href="http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?ie=UTF8&s=books&qid=1238687848&sr=8-1" target="_blank">Domain-Driven Design</a>&#8220;, the Eric Evan&#8217;s famous book. One of this book concepts is to put emphasis on immutable value objects and avoiding side effects. Those practices makes object-oriented and functional paradigms not far from each other.

### Functional paradigm recommandations

  * use only immutable structures (messages)
  * don&#8217;t use null objects or objects that are are not &#8220;ready&#8221;
  * use pure functions (= without side effects)
  * use closure to decouple behavior from data

### <span style="font-size:14px;line-height:23px;">Object-oriented paradigm recommandations<br /> </span>

  * inject dependencies
  * don&#8217;t provide getters fro mutable state
  * use interfaces for collaboration
  * create simple aggregates
  * use meaningful names

### <span style="font-size:14px;line-height:23px;">Popular object-oriented designs</span>

Rebecca Wirfs-Brock classify software architecture into 3 different designs according to the way they locate data and behavior:

#### Centralized

This is like procedural paradigm: one class controls the behavior and other classes provide the data.

This is also what Martin Fowler calls an anemic domain.

Despite from that, this kind of design has the advantage that all the logic is concentrated in one place. You don&#8217;t have to look everywhere to understand how a process is carried on.

#### Dispersed

Here the logic is spread across every objects. Each one does something very short and with as few dependencies as possible.

The drawback is that to have the complete point on something, you must navigate all the participating classes.

#### Delegated

This kind of design is a compromise between the first two. The logic is spread into some classes that controls the behavior in collaboration with other small classes. They are thus pools of responsibility that can be used in relative isolation.

### Conclusion

Learning new paradigms is interesting because it often leads you to a better understanding of the ones you use daily.

Functional paradigm has some very good best practices to include in your daily object-oriented programming like avoiding side-effects and using immutable objects as often as you can.

But besides those good pieces of advice, I must admit that this conference didn&#8217;t meet my expectations.

Anyway, the section below has very good books to read. Especially, don&#8217;t hesitate to order your own copy of domain-driven design which is a book I really appreciated.

### References

  * <a title="Uberto Barbini's Twitter account" href="https://twitter.com/#!/ramtop" target="_blank">@ramtop</a>
  * <a title="&quot;How to boost your OOP with FP&quot; conference video" href="http://vimeo.com/45010048" target="_blank">Vimeo recording of the conference</a>
  * <a title="Domain-Driven Design book on Amazon" href="http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?ie=UTF8&s=books&qid=1238687848&sr=8-1" target="_blank">Domain-Driven Design</a> by Eric Evans
  * <a title="Object Design book on Amazon" href="http://www.amazon.com/Object-Design-Responsibilities-Collaborations-Addison-Wesley/dp/0201379430" target="_blank">Object Design</a> by Rebacca Wirfs-Brock