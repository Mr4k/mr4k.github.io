---
layout: post
title:  "Brain Pain"
date:   2016-09-09 2:50:40 -0800
categories: brain teaser puzzle construction
excerpt: A simple but beautiful puzzle.
---

<p align="center">
	<img src="/riddle.jpg"> 
</p>  

This semester I am studying abroad at the [Budapest Semesters in Mathamatics](http://www.budapestsemesters.com/) program. This means that I am always surrounded by people who enjoy solving puzzles also known as mathmaticans. One of my roommates gave me this problem and I thought that it was worth sharing.

**The Problem**

<p align="center">
	<img src="/rect-problem.png"> 
</p>  

Using only a straight edge and a pencil construct a line which divides the shaded area in half. The straight edge has no markings so you cannot measure anything.  

<details>
  <summary>View the solution</summary>
  The key insight is that any line which passes through the center of a rectangle divides it in half. We can think of the area of the big rectangle as positive and the area of the little rectangle as negative. The area of the shaded region is the sum of the positive and negative areas. So if we can make a line which divides both rectangles in half we will have equal amounts of positive and negative area on both sides of the line.      

  <p>First we construct the center of one of the rectangles. We can do this by finding the intersection of the diagonals.</p>
  <p align="center">
	<img src="/rect-centers-1.png"> 
  </p>  
  Then we construct the center of the other rectangle.
  <p align="center">
	<img src="/rect-centers-2.png"> 
  </p>  
  Finally we constuct the dividing line so that it goes through both centers.
  <p align="center">
	<img src="/rect-dividing-line.png"> 
  </p>  
  Note that this solution works for any orientation/position/scale of the rectangles.
</details>



