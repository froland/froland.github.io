---
title: 'Devoxx 2012 Oracle keynote: Make the Future Java'
date: 2012-11-27T16:29:21+00:00
author: froland
layout: post
publicize_twitter_user:
  - FrRoland
categories:
  - Devoxx 2012
  - Java
tags:
  - Devoxx
  - Devoxx2012
  - Java
  - Oracle
---
# Oracle stated success factors {#oracle-stated-success-factors}

  * technology innovation
  * community participation
  * Oracle leadership

The current work focus is currently put on JavaFX and embedded Java development.

# What’s new in JavaSE 8? {#whats-new-in-javase-8}

## Closures {#closures}

The first big change is the inclusion of closures. These adopt the form of

>     (x, y) -> x + y

Those closures will come with a whole new set of methods, specially on collections, in order to provide some kind of fluent API. There will also be a new keyword `default` which will allow developers to provide a default implementation on an interface.

## Type annotations {#type-annotations}

Type annotation give further information to the compiler and thus allow it to check some invariants at compile time. Such checks can be nullability checks, immutability checks and so on.

## Compact profile {#compact-profile}

This new profile defines a subset of essential libaries that will be part of a reduced Java Platform aimed at embedded JVMs where memory consumption and file space are concerns.

# JavaEE news {#javaee-news}

The JavaEE 7th version will focus on simplicity and support of HTML5. The JavaEE 8th version will be more oriented towards cloud support and modularity.

# And after? {#and-after}

The next goals will be embedding on more and more devices and platforms (e.g. iOS or ARM processors) on the one hand and providing “embedded suites” containing a JVM, an application server and a database together.