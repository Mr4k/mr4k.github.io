---
layout: post
title:  "Tangled Up in the Web"
date:   2016-06-23 11:17:30 -0800
categories: chess project javascript python
---

<p align="center">
	<img src="/webdev.png"> 
</p>

While I am trying to figure out how to actually build a chess algorithm I am working on other parts of the project. While googling the word chess (always a good way to start) I found several nice javascript frameworks conviently called [chessboard.js](http://chessboardjs.com/) and [chess.js](https://github.com/jhlywa/chess.js). So feature creep kicked in and I decided to build a web interface to my chess engine. There are a couple nice things about this including the fact that I can easily send a link to my friend in South Africa when it's done. It will also look pretty which is nice. Currently the application I am envisioning looks something like this:

<p align="center">
	<img src="/overview.png"> 
</p>

**Why am I not doing it all client side?**
I can make excuses. Machine learning can be slow and complicated to run in a browser. Tensorflow doesn't have an offical javascript port. I need easy access to the gpu. But honestly it mostly comes down to the fact that I really want to try to build a (simple) full stack web application. I have not had much experience with web development and I want get some more experience.    
**Hows it been going?**
Well today I started by asking some basic questions. Where do I host my application. How do I communicate between the server and the client? What open source javascript projects are out there? And then some more specific questions. What's a RESTful api? Should I make my server in python or node? Finally some angry questions. What do you mean I can't send an unencrypted ajax request from a page hosted over https to the server on my laptop? What the hell is https? So overall it went well. Hopefully I'll finish something that works over the weekend.