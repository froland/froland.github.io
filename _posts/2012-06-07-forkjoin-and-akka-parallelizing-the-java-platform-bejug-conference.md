---
id: 56
title: 'Fork/Join and Akka: parallelizing the Java platform BeJUG conference'
date: 2012-06-07T11:32:35+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=56
permalink: /?p=56
categories:
  - BeJUG conference
  - Java
tags:
  - Akka
  - BeJUG
  - ForkJoin
  - Java
---
Here is another article on a BeJUG conference. This time, Sander Mak, software developer and architect at Info Support, The Netherlands, gave us an overview of two concurrency frameworks: Fork/Join and Akka.

Fork/Join is a framework that was added to the standard libraries starting with JDK 7. Akka is an open-source alternative with emphasis on the resilience of concurrent process.

### Fork/join

#### Setting the scene

The CPU speed has been growing up these last years until reaching some kind of a technical limit at around 3,5 GHz. Right now, a CPU is mainly idling while waiting for its I/O. That&#8217;s why the new trend is to have multiple CPUs.

But as Sander quote: &#8220;the number of idle cores in my machine doubles every two years&#8221;. There is an architectural mismatch because developers use to believe that the compiler and/or the JVM can handle parallelism on their own. But unfortunately, this isn&#8217;t true.

#### Demos

The first demos are declined around the computation of Fibonacci&#8217;s suite whose definition is

[<img class="aligncenter size-medium wp-image-60" title="Fibonacci's suite equation" src="http://www.froland.be/images/uploads/2012/05/fibonacci.jpg?w=300" alt="" width="300" height="52" srcset="https://www.froland.be/images/uploads/2012/05/fibonacci.jpg 750w, https://www.froland.be/images/uploads/2012/05/fibonacci-300x52.jpg 300w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.froland.be/images/uploads/2012/05/fibonacci.jpg)

Of course, the objective here is not to find an optimal solution to that problem (transforming the recursive definition into an iterative form) but just apply a concurrent computation of the recursive form.

  1. We can solve this problem by creating 1 thread to compute fib(n-1) and another thread for fib(n-2), then wait they have finished their computation and adding the results.
  
    Immediately, the number of thread explodes.
  2. If we implement the same algorithm with 2 [Callable](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Callable.html "Callable interface javadoc") objects and a thread pool, the number grows slowlier but is high anyway.
  
    The problem is that the current thread blocks while its two children finish.
  3. With Fork/Join, the thread dependency is explicit and thus the [_join_](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinTask.html#join() "ForkJoinTask.join() method javadoc") method call doesn&#8217;t block the current thread.

Join/Fork works with an auto-sized thread pool. Each worker thread is assigned a task queue which gets fed by the <a style="font-size:14px;line-height:23px;" title="ForkJoinTask.fork() method" href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinTask.html#fork()"><em>fork</em></a> method calls. The interesting behavior is that a worker is allowed to steal work from the task queue of another worker.

Another, more advanced, demo was also perform, demonstrating how to make a [dichotomic search](http://en.wikipedia.org/wiki/Dichotomic_search "Dichotomic search wikipedia article") on a large set of cities to find which one are within a certain distance from a point. Of course, the algorithm is implemented with Fork/Join.

All the code of those examples is available on <http://bit.ly/bejug-fj>.

#### API & patterns

##### Problem structure

The algorithm must be acyclic (no thread can work with another thread that is already present in its call stack) and CPU-bound. I/O-bound problems wait a lot on blocking system calls and thus prevent those threads to perform other tasks.

##### Sequential cutoff

To avoid that the overhead consumes all the computation time, you must set a threshold to decide whether the problem should be solved sequentially or in parallel. This leads to define work chunks that are processed in parallel but each steps inside the same chunk are processed sequentially.

##### Fork once, fool me twice

Some algorithm implementations allows to reuse the current thread to do come computation instead of forking a new task, thus limiting the overhead.

##### Convenience methods

There exist convenience methods :

  * Method `invoke()` is semantically equivalent to `fork(); join()` but always attempts to begin execution in the current thread.
  * Method `invokeAll` performs the most common form of parallel invocation: forking a set of tasks and joining them all.

#### Future and comparisons

Fork/Join creates threads. It is thus currently forbidden in the EJB spec. When it comes to CDI or servlet specs, we are there navigating in some kind of grey zone. Maybe this could work with JCA work manager. @asynchronous could be used as an alternative.

Anyway, it is foreseen that JavaEE 7 spec may contain java.util.concurrent package.

Compared with Fork/Join, the more classic ExecutorService doesn&#8217;t allow work stealing. It is better suited at coarse-grained independent tasks. Bounded thread pools supports blocking I/O better.

MapReduce implementations they are targeted at cluster and not single JVM. While Fork/Join is targeted at recursive working, MapReduce is often working on a single map. Furthermore, MapReduce has no inter-node communication and thus doesn&#8217;t allow work stealing.

The popular critics about Fork/Join are:

  * The implementation is really complex.
  * The scalability is unknown above 100 cores. Which may seem many for a CPU but is far below current standards for a GPU.
  * The one-to-one mapping between the thread pool size and the core numbers is probably too low-level.

<span style="font-size:14px;line-height:23px;">With the availability of JDK8, the Fork/Join API could be extended with methods on collections working with lambdas. There is also a CountedCompleter ForkJoinTask implementation that is better at working with I/O-based tasks and that is currently contained in JSR-166-extra).<br /> </span>

### AKKA

#### Introduction

Akka is a concurrency framework written in Scala that offers a Java API. I wouldn&#8217;t introduce Akka better than their author so here is what they say about it:

> We believe that writing correct concurrent, fault-tolerant and scalable applications is too hard. Most of the time it’s because we are using the wrong tools and the wrong level of abstraction. Akka is here to change that. Using the Actor Model we raise the abstraction level and provide a better platform to build correct concurrent and scalable applications. For fault-tolerance we adopt the “Let it crash” model which have been used with great success in the telecom industry to build applications that self-heals, systems that never stop. Actors also provides the abstraction for transparent distribution and the basis for truly scalable and fault-tolerant applications.

#### Actors

An actor is an object with a local state, a local behavior and a mailbox to receive messages sent to it. An actor processes only one message at a time. And as they are lightweight (around 400 bytes of memory per actor), you can instantiate many of them in a standard JVM heap.

The receive method of an actor is called when a message processeing begins and it is where all the processing is done. The framework itself is responsible for the actor management (thread pooling, dispatching, &#8230;).

Using a ForkJoinPool with Akka scales very well.

A demo of Akka usage is available on <a title="Akka demo" href="http://bit.ly/bejug-akka" target="_blank">http://bit.ly/bejug-akka</a>.

## References

  * Parleys.com conference screencast: <a title="Parleys.com ForkJoin and Akka screencast part 1" href="http://parleys.com/d/3217" target="_blank">part 1</a> and <a title="Parleys.com ForkJoin and Akka screencast part2" href="http://parleys.com/d/3218" target="_blank">part 2</a>
  * <a title="@Sander_Mak twitter account" href="http://twitter.com/@Sander_Mak" target="_blank">@Sander_Mak</a> twitter account
  * <a title="Sander Mak's blog" href="http://branchandbount.net" target="_blank">Sander Mak&#8217;s blog</a>
  * [Akka homepage](http://akka.io/ "Akka homepage")