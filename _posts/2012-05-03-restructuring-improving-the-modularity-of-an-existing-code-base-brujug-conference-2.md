---
title: 'Restructuring: Improving the modularity of an existing code-base BruJUG conference'
date: 2012-05-03T21:03:46+00:00
author: froland
layout: post
publicize_results:
  - 'a:2:{s:7:"twitter";a:1:{i:438989878;a:2:{s:7:"user_id";s:8:"FrRoland";s:7:"post_id";s:18:"198155918242877440";}}s:2:"fb";a:1:{i:819471488;a:2:{s:7:"user_id";s:9:"819471488";s:7:"post_id";s:17:"10150814557666489";}}}'
categories:
  - BruJUG conference
  - Java
tags:
  - BruJUG
  - Code refactoring
  - Code restructuring
  - Java
---
Here is an article about a <a title="BruJUG home page" href="http://www.brussels-jug.be/" target="_blank">BruJUG</a> conference given on 26/04/2012 by the founder of Structure101 company, <a title="@chedgey on Twitter" href="https://twitter.com/#!/chedgey" target="_blank">Chris Chedgey</a>. The complete conference video is available on <a title="Restructuring BruJUG conference by Chris Chedgey" href="https://vimeo.com/41214504" target="_blank">vimeo</a>.

# What is restructuring?

Refactoring and restructuring are both terms that imply non-functional changes.

Refactoring means changing the code to make it more readable. This also means invasive code editing. It usually involves only a few classes.

Restructuring means reorganizing the code-base without change to the code to improve the [modularity](http://en.wikipedia.org/wiki/Modular_programming) and make the code-base easier to understand. This involves minimally invasive code editing but the scope is the whole code-base.

The code-base structure has 3 aspects:

  * the package composition
  * the dependencies between them
  * and the hierarchy of the &#8220;nested levels&#8221; of packages

<span style="font-size:14px;line-height:23px;">The two code-base quality factors to consider here are complexity and modularity.</span>

# And why is it important?

Because a better code-base makes your code more understandable. And understandable code is cheaper to maintain and evolve. Changes have a more predictable impact on the code. And, of course, your code-base have a better testability, reusability. At the end, your code has a _better value_.

## Complexity

Complexity can be measured by different means. Two of them are fatness and tangles and can be used to measure complexity.

Fatness is when you have too much code in one place (number of method in a class, number of class in a package, number of package nested under the same package or in the same component, &#8230;).

Tangles occur when some code in a package reference code in another package which itself references code in the first package (cyclic dependencies).

Both fatness and tangles can be approximated by automatically by metrics which make them good candidate for automatic checking using thresholds (e.g. in your build system).

[<img class="alignnone size-medium wp-image-47" title="tangle-fat schema" src="{{ site.baseurl }}/imagesuploads/2012/05/tangle-fat-schema.jpg?w=300" alt="" width="300" height="190" srcset="{{ site.baseurl }}/images/uploads/2012/05/tangle-fat-schema.jpg 512w, {{ site.baseurl }}/images/uploads/2012/05/tangle-fat-schema-300x190.jpg 300w" sizes="(max-width: 300px) 100vw, 300px" />]({{ site.baseurl }}/imagesuploads/2012/05/tangle-fat-schema.jpg)

This diagram shows the link between tangles and fatness. This means that you can eliminate all tangles pretty easily by moving everything to the same place. But then you get 100% fatness. To the contrary, you can eliminate fatness by partitioning your code-base. But then you create tangles. What you seek is a compromise between the two. What you really don&#8217;t want is a code-base that is both fat and full of tangles.

## Modularity

Modularity is best-defined by the mantra &#8220;_high cohesion, low coupling_&#8220;.

Modularity can show itself by multiple means. One of them is well-defined public interfaces while the remaining internals are kept private. Another one is when your packages have a clear responsibility.

Unfortunately, the best way to assess modularity of the code is to make it checked by a human software architect.

# So how can I work on my code-base structure?

Usually, the methods and classes are OK. But there is almost no logical organisation of classes into higher level modules (= packages in Java). Packages are too often used more like a filesystem and not as an embodiment of module hierarchy.

What you need to have a good code-base structure is to understand neatly the following aspects:

  * package composition and dependencies between packages
  * the flow of dependencies
  * the application business

<span style="font-size:14px;line-height:23px;">Once you understand all of this correctly, you can define and achieve your architectural target.</span>

# Restructuring strategies

There are a lot of strategies you can use. Here are some chosen one.

## Merge parallel structures

If you have parallel structure (one for presentation, one for services, one for persistence, one for extranet, one for intranet, etc.), you&#8217;d better merge them to minimize the dependencies between packages.

## Bust very large class tangles early

You often find yourself with one or a few large classes tangles spanning many packages. Fixing these will improve your code-base rapidly.

## Do as much as you can by only moving classes and packages first

This is a least invasive refactoring you can do to improve your complexity and modularity. Moreover, this requires low effort.

## Bottom-up or top-down approach?

Both are valid but have different impacts.

Top-down approach keeps as much of the existing package hierarchy as possible. This means that the &#8220;psychological&#8221; impact on the application team will be minimized.

Bottom-up approach uses to end far away from the current structure but is often easiest to achieve.

## Tackle complexity before modularity

A structure without tangles is way easier to manipulate.

## Other strategies

  * Split packages that lack cohesion
  * Split fat packages and fat classes
  * Move tangles together
  * Make the restructuring a milestone

# Conclusion

That your code-base is a mess regarding modularity matters is common.
  
Lack of structure costs money.
  
That lack can be salvaged.
  
Restructuring your code-base is not easy but huge returns can happen.

# Examples

Here ends the &#8220;theoretical part&#8221; of the presentation and begin the examples, illustrated by the ReStructure101 software which helps the architect to visualize the current structure of his code-base and allows to simulate structure changes and their impacts.

That tool philosophy is to create a task list reflecting the changes done rather than changing the code-base directly. After a restructuring session, the architects ends up with that task list that he can perform himself or plan to be executed by other developers.

Plugins allows to use that task list easily inside IDE like Eclipse or IntelliJ.

I&#8217;d say I love this philosophy because it gives you the feeling you&#8217;re always in control and that you are not only executing a drag&#8217;n&#8217;drop session in a GUI but really modifying your code-base deeply.

Thanks BruJUG for this enlightening conference.

See you next time.