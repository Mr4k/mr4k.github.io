---
layout: project
title:  "Impact"
excerpt: Determine when two polyheadra collide.
project: 1
---

<p align="center">
	<img src="/impact-screen.png"> 
</p>

A simple algorithm which checks to see when two polydera will collide.

**Highlights**  
Impact is simple. There are faster algorithms but they are usually concealed behind advanced math. Impact is also continuous which means that objects can never step over each other if they are moving too fast.  

**Basic Idea**  
The basic idea behind Impact is that if two polyhedra are going to collide then one of the following 3 things must happen:  
1) one vertex from one of the polyhedron goes through the face of the other  
2) one edge from one of the polyhedron goes through an edge of the other polyhedron  
3) one vertex from one of the polyhedron goes through an edge of the other polyhedron  
Now all we need to do from here is solve for each of these cases to check if two polyhedra collide. Of course solving those cases takes some thought but I found that they were easier to work with than the other methods I had looked at before.

**Demo**  
You can check out the algorithm in action in [this](https://mr4k.github.io/Impact/) demo and view the source on [github](https://github.com/Mr4k/Impact).
