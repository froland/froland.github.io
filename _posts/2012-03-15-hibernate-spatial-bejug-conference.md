---
id: 6
title: Hibernate Spatial BeJUG conference
date: 2012-03-15T22:48:46+00:00
author: froland
layout: post
guid: http://frroland.wordpress.com/?p=3
permalink: /?p=6
publicize_results:
  - 'a:2:{s:7:"twitter";a:1:{i:438989878;a:2:{s:7:"user_id";s:8:"FrRoland";s:7:"post_id";s:18:"180425343071555584";}}s:2:"fb";a:1:{i:819471488;a:2:{s:7:"user_id";s:9:"819471488";s:7:"post_id";s:17:"10150683202056489";}}}'
categories:
  - Java
tags:
  - BeJUG
  - Geographic data
  - Geographic information system
  - GIS
  - hibernate
---
Hello,

Tonight I attended my first [BeJUG](http://www.bejug.org "BeJUG") [conference about Hibernate Spatial](http://www.bejug.org/confluenceBeJUG/display/BeJUG/Hibernate+Spatial "Hibernate Spatial"). So here is a summary of this conference.

Hibernate Spatial is a Hibernate extension for storing and querying GIS data. Thus for those like me who have heard from GIS but don&#8217;t really know what hides behind the acronym, the first part was a nice introduction.<!--more-->

GIS is about geographic data. The most basic form of a geographic data is a _point_, determined by its coordinates (latitude/longitude, GPS coordinates, &#8230;). Those coordinates take their meaning by projecting a 3D point on a surface (a sphere or a plane for example). Their exists many projection system which have pros and cons. But what is most important is that the majority of coordinate systems are only valid locally. Which means a first concern about GIS data is using a good coordinate system. This coordinate system is important, for example, when computing distances between two points.

To a basic point defined on a surface, it is possible to add other information. The most current is the Z coordinate which add elevation to the point. But it must be noted that the majority of GIS tools and libraries today only work with the projection of that point, not taking the elevation into account.

Basic points can of course be linked in sequence, building a _line_. And if the line is closed, we get a _polygon_.

With those different geometries, we can start defining operations and queries. For example, test if a point is inside a polygon. Or get the polygon that is the intersection of two others. And so on. There exists standard specifications for those operators, like the one defined by the [OGC](http://www.opengeospatial.org "OGC"), Simple Feature Specification or its successor Simple Feature Access.

This is where specialized databases or database extensions come into play. They give a syntax to make all those requests in an SQL-like way. And if you use hibernate to manage the persistency inside your application, [hibernate-spatial](http://www.hibernatespatial.org/ "Hibernate spatial website") gives you such a syntax for both criteria and HQL queries.

The currently working implementations of databases are <a class="zem_slink" title="PostgreSQL" href="http://www.postgresql.org" rel="homepage" target="_blank">PostgreSQL</a> extension, namely <a class="zem_slink" title="PostGIS" href="http://www.postgis.org" rel="homepage" target="_blank">PostGIS</a>. By far one of the more stable and advanced; <a class="zem_slink" title="MySQL" href="http://www.mysql.com" rel="homepage" target="_blank">MySQL</a> GIS extension; <a class="zem_slink" title="Oracle Spatial" href="http://www.oracle.com/technology/products/spatial/index.html" rel="homepage" target="_blank">Oracle spatial</a> extension and <a class="zem_slink" title="Microsoft SQL Server" href="http://www.microsoft.com/sqlserver" rel="homepage" target="_blank">Microsoft SQL Server</a>. About MySQL, that implementation is tricky: it looks like it support a lot of the standard operators for GIS data, but indeed it only uses the bounding box of the geometries, leading to unexpected results where isolated polygons behave as if they had an intersection, amongst other side effects.

Open source libraries for Java to work with GIS data are:

  * [Java Topology Suite](http://tsusiatsoftware.net/jts/main.html "Java Topology Suite") (kind of a common ancestor)
  * [GeoTools](http://www.geotools.org/ "GeoTools") (which is rather complete but with varying code quality between its modules and strong design impacts)
  * [geolatte](http://www.geolatte.org/ "geolatte") (which was presented in more details during the conference)

There also exist complete GIS frameworks like [Geomajas](http://www.geomajas.org/) which was presented at the end of the conference.

Thanks for reading this post. As it is a first post, unleash yourself and post me comments both on style and contents. I am always willing to improve.

See you in two weeks for another conference. And thanks to [BeJUG](http://www.bejug.org "BeJUG").