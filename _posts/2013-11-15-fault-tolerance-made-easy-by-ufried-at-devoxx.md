---
title: 'Fault tolerance made easy by @ufried at #devoxx'
date: 2013-11-15T08:09:12+00:00
author: froland
layout: post
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=13385405&stype=M&topic=5807026282172944384&type=U&a=YR9z'
publicize_facebook_url:
  - https://facebook.com/
publicize_twitter_user:
  - FrRoland
publicize_twitter_url:
  - http://t.co/1GcmOvliLa
categories:
  - Software development
tags:
  - Architecture
  - Devoxx
  - dv13
  - fault-tolerance
  - patterns
---
# Fault tolerance made easy {#fault-tolerance-made-easy}

Given by Uwe Friedrichsen at Devoxx 2013.

It describes a series of patterns applicable to vast majority of applications in order to support a user-friendly degradation of functionalities when a resource becomes unavailable or that the load becomes too high to cope with every request in a satisfactory way.

<!--more-->

## Timeouts {#timeouts}

Callable or ImpleTimeLimiter from guava can be used to introduce timeouts inside the time-intensive calls. Introducing timeouts need some more reflection:

  * determine timeout duration
  * make timeout duration configurable
  * create self-adapting timeouts by combining with a monitoring system and react with a percentile-based rule
  * use timeouts in JavaEE containers but pay attention not to mess with the thread management of the container

## Circuit breaker {#circuit-breaker}

When you know that a resource is unavailable, there is no interest in waiting each time for the timeout.

A circuit breaker goes between a caller and a callee. Under nominal conditions, it just forwards to the callee. If some defined number of method calls fail, the circuit broker then stops forwarding and starts replying immediately with some ResourceUnavailableException.

Some attention points:

  * thread safety
  * deal with different failure types
  * tuning the circuit breaker (failure count threashold, reset timeout)
  * investigate existing available implementations

## Fail Fast {#fail-fast}

If a client is going to make use of a lot of expensive actions, it is better to check since the beginning wether the system is in a state healthy enough to carry on the expensive operations.

A FailFastGuard takes basically a list of circuit breakers and check that they are in the correct state. Otherwise, it fails fast.

## Shed Load {#shed-load}

When too many clients make too many requests to an overloaded server (a famous band concert sale for example).

A solution is to shed the request by the intervention of some kind of gate keeper. The gate keeper works in collaboration with the monitoring system to decide when he has to shed some requests so that the forwarded requests get a fair chance to be successful.

A way to implement it is with a filter. It is even better to implement this on a front facing reverse proxy that will limit.

A random shedding limiter is enough to limit requests to a stateless service. Otherwise, the limiting strategy has to be smarter, often involving caching of load factor numbers etc.

## Deferrable work {#deferrable-work}

Sometime, the load is not caused by online requests but also by routine work. Reducing even further the ability of the system to stand high load situations.

With deferrable work, the system is able to cut out the routine work to be able to process more online transactions. The cut out work is then deferred to be run when the load is lower.

## Leaky bucket {#leaky-bucket}

Imagine we&#8217;ve got some resources that sometime responds, sometime not, but mainly is erratic. Resetting the connection to it generally fix the issue, but it comes back after some time. But at the same time resetting the connection is expensive.

To cope with that erratic behaviour that the circuit breaker will fail to identify, let&#8217;s use a leaky bucket wich fills each time the action fails and that drains according to a time-based rule. If the bucket overflows, reset the connection.

## Limited retries {#limited-retries}

This pattern is best used when a retry can usually achieve what you want to do. Contrary to the previous pattern, the behaviour doesn&#8217;t degrade over time.

In that case, a simple retry loop can do the job. Please consider that retrying more than once before escalating the error is probably a bad idea.

## Other patterns {#other-patterns}

  * Complete parameter checking
  * Marked data
  * Routine audits

All the patterns explained during the presentation are pretty easy to write. But used carefully, they make the system far more resilient

## Readings {#readings}

  * Release it!
  * Patterns for Fault Tolerant Software
  * On designing and Deploying Internet-scale services
  * Tanenbaum, Distributed systems &#8211; (&#8230;)

Slides are available at: <http://slideshare.net/ufried>