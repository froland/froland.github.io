---
id: 172
title: 'Java 8 and beyond #devoxx #dv13 by @mreinold and @BrianGoetz'
date: 2013-11-15T08:39:41+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=172
permalink: /?p=172
publicize_facebook_url:
  - https://facebook.com/10152048803566489
publicize_twitter_user:
  - FrRoland
publicize_twitter_url:
  - http://t.co/2iDG8CWvz4
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=13385405&stype=M&topic=5807033972857151488&type=U&a=zfQc'
categories:
  - Software development
tags:
  - Devoxx
  - dv13
  - Java
---
# Java 8 and beyond {#java-8-and-beyhond}

Keynote performed at Devoxx 2013 by Mark Reinhold and Brian Goetz

Java exists for 18 years. Nothing can stay so long in the field without evolving.

One of the latest evolution in computer science is multiplication of cores and pipelines in processor.

For that reason it is intersting to look at problems from a perspective where everything is splitted in parallel data flows which split and split and split until complex problems resolve themselves as a multitude of simple parallel calculation.

To support that perspective, closures were added.

<!--more-->There exists a lot of 

_dynamic axis_ in Java. Even if we go back to 1995. At that time, Java was considered a language to solve problems (blue-collar language), not an academic language, being both radical and conservative. GC, JIT, dynamic linkage, threading, etc were features needed by customers. But customers wanted something less scary than the languages with those features at that time. That&#8217;s why it was made similar to C++. To prevent customers&#8217; fear.

In the design of Java, there are design tensions between reasons to change and reasons not to change. However, there are also core language principles:

  * reading code is more important than writing code
  * simplicity matters
  * one language, the same everywhere

When planning Java 8:

  * developpers have more choiceces in programming languages (even inside the JVM)
  * default and static methodes in interfaces
  * java.util.stream package for aggregate/data-parallel operations  
    All these having a big impact on developer productivity.

Lambda expressions are anonymous methods. They enable some productivity-related shortcuts and are key to parallel processing. In Java, lambda expressions, although they often replace previous inner class, aren&#8217;t inner classes under disguise. A lot of things have been made to make them both faster to write and execute like type inference.

    int heaviestGuy = people.parallelStream()
                            .filter(p -> p.getGender() == MALE)
                            .mapToInt(Person::getWeight())
                            .max()

The previous excerpt shows how lambda expressions support the data parallel stream interfaces. The last method is call an aggregate and allows developers to write easily map-reduce constructs pretty easily starting from standard and already used data structures.

Although lambda expressions may look like &#8220;just another language feature&#8221;, they actually allow developpers to take the path of more functional-oriented paradigm.

In order to make Collections API evolve without breaking backward compatibility, default methods were introduced. That way, old interfaces could be evolved by adding new methods and providing default implementations (like the stream() method).

To summarize, such changes make it possible to make Java evolve without breaking compatibility, allowing developer productivity improvements.