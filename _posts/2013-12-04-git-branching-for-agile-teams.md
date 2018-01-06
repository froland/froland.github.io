---
id: 209
title: Git Branching for Agile teams
date: 2013-12-04T12:36:27+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=209
permalink: /?p=209
image: /images/uploads/2013/12/119365398262159379.jpg
categories:
  - Software development
tags:
  - Branching (revision control)
  - ci
  - Continuous integration
  - git
---
In this webinar, Sven Peters @svenpet shared some thoughts about git workflows used at Atlassian.<!--more-->

The base workflow takes place inside a development environment using Git as a SCM, with a sufficient unit test coverage and CI builds to run those unit tests quite often.

For every issue (issue means here something smaller than a complete feature) he works on, a developper create a new branch as soon as he starts working on that issue. For even better traceability, the created branch can use the issue identifier (for example, a Jira issue number) as a name. Concurrently, a new CI build is created, pointing to that branch.

The developer does his job as usual: test, code, refactor, rinse, repeat. As usual with Git, he can commit as often as he wants, pushing his changes to the remote branch whenever he wants to get a CI build report on his work.

Once he&#8217;s done and his CI build passes, he can then merge his changes to the master branch and make sure the master CI build passes.

The base workflow allows the team to enrich it with various additions:

  * a shared integration branch to preserve the master branch in case your build failure tolerance is very low
  * release branches if the team must maintain concurrent versions of its product
  * asking for code reviews via pull requests
  * isolating a particularly sensible product via forks
  * etc.

This workflow is a nice evolution of the recommanded git workflow. It leverages the great git merging capabilities. As experience may show, it is often easier to merge sooner than late. And this conditions holds true in this case, thus bringing a compromise that both lessens the risk of build failures on the master branch by providing isolation to the developer work and maintains ease of use.

Of course, the webinar goes in more details than what I wrote. If your interested or just curious, watch the page <a title="Atlassian webinars" href="https://confluence.atlassian.com/display/WEBINAR/Atlassian+Webinars+Home" target="_blank">https://confluence.atlassian.com/display/WEBINAR/Atlassian+Webinars+Home</a> where the webinar recording shoud be published in the coming days.

Also, if you tweet about this, don&#8217;t forget to use the #code4acause hashtag as Atlassian will send 1$ for each of your tweet with it to help the society in one or the other ways.