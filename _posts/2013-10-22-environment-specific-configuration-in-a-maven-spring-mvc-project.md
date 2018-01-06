---
title: Environment-specific configuration in a maven-spring-mvc project
date: 2013-10-22T09:05:16+00:00
author: froland
layout: post
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=13385405&stype=M&topic=5798343085951836160&type=U&a=mTVV'
publicize_facebook_url:
  - https://facebook.com/
publicize_twitter_user:
  - FrRoland
publicize_twitter_url:
  - http://t.co/FCwvYLBfhi
categories:
  - Software development
tags:
  - Java
  - liquibase
  - maven
  - spring
  - spring-mvc
---
Hi,

For once, this article is not about a conference but about some issues I thought about recently.

The context is a training session I give in my company. In that training session, I use a web app to illustrate some points and let the attendees perform some exercises. The web app project uses maven, spring-mvc and multiple databases (mysql and hsqldb) for different purposes (local playing, local integration tests, CI integration tests and &#8220;production&#8221;).

Those different purposes have different needs. Ideally, I would like to have something like:

  1. local playing &#8211;> local mysql database
  2. local integration tests &#8211;> hsqldb in-memory database
  3. CI integration tests &#8211;> dedicated mysql database running on the CI server
  4. &#8220;production&#8221; &#8211;> dedicated mysql database running on the &#8220;production&#8221; server

To update schema updates, I use liquibase. I would like to run it in update mode on environments 1, 2 and 4 and in drop-and-create mode on environment 3.

I know a lot of different solutions that can achieve this situation, but the solution must be neat (it&#8217;s a beginner training application and thus shouldn&#8217;t attract the focus on themes I don&#8217;t want to talk about). Currently, I still haven&#8217;t found THE perfect solution. So I&#8217;ll describe the ones I&#8217;ve already tried with some kind of success in coming articles. Feel free to propose anything in comment. I&#8217;ll be glad to investigate your propositions.