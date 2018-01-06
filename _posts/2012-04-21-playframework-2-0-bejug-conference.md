---
id: 15
title: PlayFramework 2.0 BeJUG conference
date: 2012-04-21T15:42:46+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=15
permalink: /?p=15
publicize_results:
  - 'a:2:{s:7:"twitter";a:1:{i:438989878;a:2:{s:7:"user_id";s:8:"FrRoland";s:7:"post_id";s:18:"193726482609209345";}}s:2:"fb";a:1:{i:819471488;a:2:{s:7:"user_id";s:9:"819471488";s:7:"post_id";s:17:"10150768849226489";}}}'
categories:
  - Java
tags:
  - BeJUG
  - Java
  - Play Framework
  - PlayFramework
  - Scala
  - Web application framework
---
So here is my second post. This time, I cheated a little. As I was sick at the conference time, I had to catch up with the recording of the presentation that BeJUG team put on parleys.com. Big thanks to them!

So, what is PlayFramework?

PlayFramework is a web framework. It features Java and Scala as programming languages for both controllers and page template. As you&#8217;ve probably already figured out from the terms I used, this framework is more action-based than component-based.

With the framework comes a philosophy.

First, everything is put in place so that the developper can keep focused on what he&#8217;s doing. Meaning that you can use only a text editor (or an IDE if you prefer) and a browser once the PlayFramework server has been launched. Every feedback the framework gives you (and it gives you plenty of feedback) appears in the browser. And every change you make in the code is reflected immediately in the browser.

Second, the framework uses a compiler to validate all your files (even page templates and configuration files). If there is a syntax error in your Scala or Java file, a compilation error is displayed in the browser with an indication of the error and the line it occured. This is kind of standard behaviour. Nothing special about it. When it becomes really cool is when you get the same type of error for your template pages with indications relative to the original file (and not the generated class). So here we get to something similar to what we can get with a JSP interpreter. That&#8217;s cool. What is really cool is that we even get the same feedback detail level for configuration files. That rocks!

Third, the framework tries to not fight the HTTP protocol. That means that it doesn&#8217;t hide the inner bits of the protocol. Quite the contrary. You will thus find methods to ease the usage of HTTP response codes, the usage of cookies, &#8230;

From the given demo, it really seems to be a framework easy to use and easy to learn. And as it doesn&#8217;t try to hide technical details of the HTTP protocol and the browser rendering, I have good hope that everything you know about web development is directly useable with this framework. Good point thus for a framework that lessens the amount of plumbing needed to get started and still succeeds to not get into the way.

After that first impressive demo, came a description of the Enumerator &#8211; Iteratee pattern. This pattern actually comes first from Haskell (<http://hackage.haskell.org/package/enumerator>). It is composed of:

  * iteratee: a data sink that consumes input values and generate a single output value.
  * enumerator:  a data source that generates input values (from reading a file, a socket for example) and is passed an iteratee.
  * enumeratee: a data transformer that read from an enumerator and provide transformed data to an iteratee.

<span style="font-size:14px;line-height:23px;">Instead of controlling the information flow from the source, in this pattern it is the iteratee that is in charge of telling the enumerator if it wants to get more data or not.</span>

That pattern is featured heavily in Play. You can use it for example to manage Comet connections (HTTP connections that &#8220;never&#8221; closes and that streams data to which the client browser can react in real time). You just define your enumerator on the server and tell him what callback method to call on the client when it has data to process. This really ease the concurrency management and allows the server to keep up with thousands of connections with a reasonable hardware.

The presentation ends with the show of the sample applications embedded within the PlayFramework 2.0 distribution.

This is only a small summary of the presentation. If you wants to have a look at it, you can watch the complete presentation on Parleys.com (<a title="PlayFramework 2.0 part 1" href="http://www.parleys.com/d/3143" target="_blank">http://www.parleys.com/d/3143</a> for part 1 and <a title="PlayFramework 2.0 part 2" href="http://www.parleys.com/d/3144" target="_blank">http://www.parleys.com/d/3144</a> for part 2).