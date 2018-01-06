---
id: 177
title: 'Practical RESTful persistence by @shaunmsmith at #devoxx'
date: 2013-11-15T08:48:31+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=177
permalink: /?p=177
publicize_facebook_url:
  - https://facebook.com/10152048815811489
publicize_twitter_user:
  - FrRoland
publicize_twitter_url:
  - http://t.co/qsP0HJM6T5
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=13385405&stype=M&topic=5807036203325095936&type=U&a=VdXY'
categories:
  - Software development
tags:
  - Devoxx
  - dv13
  - Java
  - jpa-rs
---
# Practical RESTful persistence {#restful-persistence}

EclipseLink provides a JPA-RS implementation. Let&#8217;s see what hides behind this.

The use case is a web application with the main logic inside the browser and a backend providing only persistency. We want to provide a REST API to that persistence.<!--more-->

To do this, let&#8217;s create a stack with the browser at the top and the database at the botom:

  1. browser
  2. jax-rs
  3. EclipseLink MOXy
  4. java
  5. EclipseLink JPA
  6. database

The bottom part from database to Java is well-known. So we won&#8217;t discuss too much about it.

EclipseLink uses the binding part of its JAXB implementation to bind to JSON (JSON-P specification only supports parsing and generation but no binding).

The challenges of translating database data into JSON content are the following:

  * composite keys/embedded key classes
  * byte code weaving that is done by JPA comes into the way when it comes to the JAXB marshalling
  * bidirectional/cyclical relationships because one of the side must be cut off when marshalling, which means that the unmarshalling will lose one-side of the relationship

To fix the last of these three issues, EclipseLink introduces a new annotation 0XmlInverseReference which induces EclipseLink to recreate the missing side of the relation base on the other direction.

JPA-RS is capable of creating the JAX-RS endpoint, creating a default JAXB mapping.

EclipseLink JPA-RS comes as a JavaEE fragment, which means that integrating it is just a matter of making it available in the classpath.

**Roadmap**

Current approach of fetching one entity level at a time and providing links to the next level entities is kinda naive.

It should be able to select only the wanted information and to provide linked entites if wanted.

Currently, security is not implemented with enough grain: all entities are exposed.

As a conclusion, this tool allows to gain a lot of time if all what is needed is to provide a JAX-RS layer above an existing persistence unit.