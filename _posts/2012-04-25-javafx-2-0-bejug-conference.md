---
title: JavaFX 2.0 BeJUG conference
date: 2012-04-25T07:00:37+00:00
author: froland
layout: post
publicize_results:
  - 'a:2:{s:7:"twitter";a:1:{i:438989878;a:2:{s:7:"user_id";s:8:"FrRoland";s:7:"post_id";s:18:"195044656722083840";}}s:2:"fb";a:1:{i:819471488;a:2:{s:7:"user_id";s:9:"819471488";s:7:"post_id";s:17:"10150777449216489";}}}'
categories:
  - Java
tags:
  - BeJUG
  - Java
  - javafx
---
# History and status

The presentation started with a quick history of Java. How it started as a desktop application programming language. How that rich client facet of Java almost disappeared completely behind the Java EE web application years ago for economic reasons. And then how it may come back in a near future with frameworks like JavaFX 2.

But what is JavaFX 2? JavaFX 2 is “a modern Java environment designed to provide a lightweight, hardware-accelerated UI platform that meets tomorrow’s needs”. In this assertion, every word is important. And if JavaFX 2 may address tomorrow’s needs, today, it is still a work in progress. Only the Windows platform has attained the General Availability stage. Mac OS X implementation should get out of the dark in the next months and you can download a quite useable Linux implementation from <a class="zem_slink" title="OpenJDK" href="http://openjdk.java.net/projects/jdk7/" rel="homepage" target="_blank">OpenJDK</a> sources. But unfortunately, iOS and Android implementations of JavaFX are not yet available. This is really unfortunate.

It is foreseen that JavaFX will replace Swing and AWT as a standard graphic library starting with Java 8. But that won’t happen before <a class="zem_slink" title="JavaFX" href="http://javafx.com/" rel="homepage" target="_blank">Java FX</a> becomes JavaFX 3.

So, pragmatically speaking, JavaFX 2 must be considered as experimental at the moment (and that’s also what the demo done during the second part of the presentation confirms). It is the advice of the first presenter too : “wait until Java 8 and JavaFX 3 before reaching production with a JavaFX application”.

# API

There are 2 APIs to JavaFX 2: the FXML API which is a declarative XML interface; the second API is a Java API similar to the Swing interface.

A big difference from the former Swing API is the ability to render HTML content through the use of the WebView component. This makes it possible to enrich a classical web application with behaviours (such as Near Field Communication or eID reader integration) that are only possible in a rich client.

The first part of the presentation is ended by a Hello World demo which looked to me a lot like the tutorials I’ve made with Swing. At least, JavaFX looks more polished than its predecessor and simple things seem simple to program.

# Real-World demo

The real-world demo features healthcare software to manage patient’s dossier and made by the HealthConnect firm.

A lot of CSS was used to style UI controls. The CSS properties are proprietary to JavaFX (they all begin with -fx- …) but look similar to HTML properties.

The accent is put on the calendar and dropup (with auto-complete) controls developed and heavily customized by HealthConnect.

It is also put on the observer pattern which allows binding a UI control to a model property. Once done, every change on one of the UI or the model is reflected instantly on the other. Unfortunately, to achieve this, JavaFX 2 has created its own JavaBean-like API with, for example, SimpleObjectProperty and ObservableList.

There also exists a Task API to ease concurrency management while running service callbacks only in the main UI thread.

Here are the various lessons learned from the development of the application:

## What has been easier with JavaFX 2?

  * great look and feel
  * customization thanks to CSS
  * the binding between the model and the view thanks to the observer pattern

## What has been more difficult with JavaFX 2?

  * hard to change the default behaviour of controls
  * fighting against the JavaFX rules of engagement leads to weird results (but which framework doesn&#8217;t behave like that?)

<span style="font-size:14px;line-height:23px;">Here are now some resources mentioned during the presentation:</span>

  * JemmyFX (<http://fxexperience.com/2012/02/announcing-jemmyfx/>) which is a testing framework for JavaFX.
  * Devoxx conference &#8220;Pro JavaFX &#8211; Developing Enterprise Applications&#8221; by Stephen Chin (<http://www.parleys.com/d/1593>) talking among other stuff about unit testing of JavaFX UI.
  * JFXtras (<http://code.google.com/p/jfxtras/>) JavaFX components.
  * DataFX (<http://www.javafxdata.org/>) data source adapters to ease the integration with the JavaFX components.

I hope you enjoyed this third article. See you soon for the next one.