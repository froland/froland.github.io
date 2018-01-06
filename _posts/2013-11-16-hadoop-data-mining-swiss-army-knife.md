---
id: 195
title: 'Hadoop data-mining swiss army knife by @plopezFr and @BertrandDechoux at #devoxx #DV13-HadoopCode'
date: 2013-11-16T13:34:55+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=195
permalink: /?p=195
categories:
  - Java
tags:
  - Big data
  - Cascading
  - Devoxx
  - dv13
  - Hadoop
  - Hive
  - Java
  - MapReduce
  - Pig
---
# <a class="zem_slink" title="Hadoop" href="http://hadoop.apache.org/" target="_blank" rel="homepage">Hadoop</a> data-mining swiss army knife {#hadoop-data-mining-swiss-army-knife}

<span style="color:#444444;line-height:1.7;">The website voyages-sncf.com sells half of the Thalys tickets in France and is one of the most visited websites in Europe. That high load triggered a huge amount of logs that, at first, were not used.</span> After some time, they wanted to sek value in those logs and started investigating solutions for distributed computing. <!--more-->Hadoop was chosen to ease the task of implementing a 

<a class="zem_slink" title="MapReduce" href="http://en.wikipedia.org/wiki/MapReduce" target="_blank" rel="wikipedia">map-reduce</a> algorithm to extract data from those log files. Hadoop works in 3 phases:

  1. Transform data into key-value records. This is the _map_ operation.
  2. Shuffle and sort is the next step which is entirely taken care of by Hadoop. It is similar to a group by operation that gathers all records with the same key.
  3. Finally, the reduce step works on one group of label at a time to compute the wanted aggregated result.

Of these, only the 1st and 3rd operation require the developer to write some code. The native Hadoop API requires the developer to write two classes, one for the mapper (1st step) and another for the reducer (2nd step). The encountered issues with the native API were:

  * it&#8217;s java with all its well-known limitations
  * humans don&#8217;t ussually think in terms of map/reduce terms
  * it ended up writing a lot of boilerplate
  * it is a very low level api which means that a lot of things need to be written (file splitting, mapper components, reducer components) all of which prevented usage of the hadoop cluster to business users.

Then come the Hive and Pig projects. Instead of specifying the algorithm in terms of map and reduce operations, let&#8217;s define the wanted computations with a query language similar to SQL. For example, with Hive:

    CREATE TABLE myinput (line STRING);<br />LOAD DATA LOCAL INPATH '/tmp/devoxx.txt' INTO TABLE myinput;<br />CREATE TABLE wordcount AS
      SELECT word, count(1) AS count
      FROM (
        SELECT EXPLODE(SPLIT(line, ' ')) AS word FROM myinput
      ) words
      GROUP BY word
      ORDER BY count DESC, word ASC;

This example counts the words inside a text file. Pros:

  * SQL is well known by data analysts
  * Metastore / HCatalog keeps a catalog of definiton of table abstractions
  * there exist external tools that are able to visualize Hive results just like any SQL database table.

Cons:

  * Complex SQL
  * There exist standard operations provided by Hive. This collection can be extended with custom operations. This custom operations have to be coded in Java and it can thus be difficult to manage two different views of the same operation.
  * Testability is poor, especially as it takes a long time to test just one Hive SQL statement
  * Only a subset of the standard SQL 92

Pig leverages the same concept of defining your expectations but in another language, <a class="zem_slink" title="Pig (programming tool)" href="http://pig.apache.org/" target="_blank" rel="homepage">Pig latin</a>, which is close to what you often find in data extraction tools, and then translate that logical plan into map/reduce operations. Both of these solutions have the major issue that they are really dificult to debug because of both the lack of tooling and the fact that they translate their language into another &#8220;paradigm&#8221;, rendering the mapping of their language constructs to operations a debugger can catch impossible. Furthermore, the business customers, which were the target of those type of abstraction, don&#8217;t have enought time to cope with the learning curve and didn&#8217;t invested, at least in this case, enough to be able to work with the tools. Thus, opening the Hadoop cluster to those customers failed and the decision was made to go through the developers&#8217; team. Then came to the rescue <a class="zem_slink" title="Cascading" href="http://www.cascading.org/" target="_blank" rel="homepage">Cascading</a>, which is a higher level API on top of Hadoop. The gains were that Cascading is using Java as a definition language, thus easing the debugging, while taking care of a lot of the hassle and boilerplate. (Cascading is also available in other languages.)