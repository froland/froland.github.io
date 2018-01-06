---
id: 72
title: Modular Java BeJUG conference
date: 2012-06-29T13:25:15+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=72
permalink: /?p=72
image: /images/uploads/2012/06/logo_osgi.jpg
categories:
  - BeJUG conference
  - Java
tags:
  - Java
  - Java Platform Enterprise Edition
  - javaee
  - jigsaw
  - modular java
  - osgi
---
The two presenters were Bert Ertman and Paul Bakker, both working for Luminis, Bert being an official Oracle Java Champion.

They began their talk by stating the following two trends:

  * Applications these days are bigger and bigger and thus are more complex
  * More and more teams adopt, at least partly, agile methodologies

<span style="font-size:14px;line-height:23px;">Those trends bring new challenges along with them:</span>

  * dependency management to manage dependencies of the application on its libraries.
  * versioning of the application that must fit with other enterprise projects which have their own, parallel, lifecycle.
  * maintenance on the long term become difficult (&#8220;it&#8217;s difficult to refactor level 0 of a 20-story skyscraper&#8221;).
  * deployment

<span style="font-size:14px;line-height:23px;">As applications are moving to the cloud, other non functional requirements enter the game too:</span>

  * As your users never sleep, you must deploy with 0 downtime.
  * Deploying a big monolithic application on the cloud takes time.
  * Customer-specific extensions that must be deployed on a Software as a Service application.

_<span style="font-size:14px;line-height:23px;">Modularity is the answer to those issues.</span>_

### What is a module?

Every decent programmer must probably remember this from his classes: a good design has loose coupling but high cohesion.

Coupling is prevented through the use of interfaces and by hiding the actual implementations. Each module has its public and private parts and other modules cannot &#8220;touch its private parts&#8221;.

But then comes the issue of instantiating the actual classes. A popular way to solve that problem is to use dependency injection. But you can also do it with some kind of service registry which you can ask to provide you with an implementation of type X. Each module notifies the registry about the interfaces he provides implementations for and which interfaces he consumes. That service registry can manage multiple versions of interfaces and choos the best match.

With a little help of design-time modularity, we can thus tame the _spaghetti syndrome_ of the most complex application.

### Runtime implementation

When we analyse the same issues from the runtime  view, the JAR file becomes THE unit. So how do we deal with module versioning and intra-module dependency management? How can we replace just 1 module at runtime?

The first part of the answer is: put the architectural focus on modularity! If you don&#8217;t group coherent functionality together, there&#8217;s no point using the second part of the answer. So that second part is: use a modular framework like OSGi (Jigsaw can be seen as an alternative to OSGi but is far less mature). But keep in mind that a modular framework is no guarantee, the key is a modular architecture.

### How well does modular java play in the JavaEE game?

JavaEE is high level. OSGi is low level and provides no enterprise services (transactions, security, remote access, persistence, &#8230;) on its own. So you&#8217;re left with 3 options:

  * Deploying enterprise services as published OSGi services.
  * A hybrid approach with a classic JavaEE part + a modular part + a bridge between the 2 containers.
  * An OSGi &#8220;à la carte&#8221; approach.

<span style="font-size:14px;line-height:23px;">The first option involves application servers that publish their services as OSGi modules. Glassfish does that. There is now a concept of WAB (Web Application Bundle) files that are WAR files whose servlets are deployed as OSGi resources. A similar system exist for EJB bundles but there is no standard yet.</span>

<span style="font-size:14px;line-height:23px;">The second option can be implemented with Weld as CDI container to provide CDI inside the bundle and consume and produce OSGi services.</span>

I&#8217;ll complete the description of the third option when I get access to the Parleys&#8217; video (sorry, I ran out of ink at that moment and writing this article 2 weeks after the conference doesn&#8217;t help either).

### Deploying to the cloud

The main concern here is that a modular application may involve hundreds of bundles. But hopefully, something like Apache ACE platform can help you manage this and gives you the option to redeploy 1 module at a time.

### Demos

I won&#8217;t go into the details of the demo. The easiest is to wait the Parleys&#8217; video as this will be far more interesting than me trying to write what I&#8217;ve seen.

### References

  * Video: <a title="Modular JavaEE in the cloud (part 1)" href="http://www.parleys.com/#st=5&id=3361" target="_blank">part1</a> and <a title="Modular JavaEE in the cloud (part 2)" href="http://www.parleys.com/#st=5&id=3362" target="_blank">part2</a>
  * Demo code: <https://github.com/paulbakker/osgi-demo>
  * MongoDB Jackson mapper
  * [OSGi in action](http://manning.com/hall/ "OSGi in action book") (start with chapter 3, dixit Bert)
  * [Java application architecture](http://www.informit.com/articles/article.aspx?p=1850815 "Java application architecture book")
  * [@BertErtman](https://twitter.com/BertErtman "Bert Ertman twitter account")
  * [@pbakker](https://twitter.com/pbakker "Paul Bakker twitter account")