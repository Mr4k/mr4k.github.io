---
layout: post
title:  "Mind Sweeper"
date:   2017-01-29 2:50:40 -0800
categories: mindsweeper video game algorithm python
excerpt: Sometimes coding is just fun and games
---

<p align="center">
	<img src="/minesweeper.jpg"> 
</p>  
  
This weekend I braved the cold and visted my brother in upstate New York. While I was there he showed me minesweeper and told me he hadn't beaten it yet. Although I had heard of minesweeper I had never played it so I gave it a couple (many) tries but I just did not have the patience to win. Several failed attempts later I decided that it might be easier to write a [program](https://github.com/Mr4k/sweeper) which used my general strategy but had an unlimited attention span rather than continuing to accidently click bombs I meant to flag. Over the next couple hours I was able to write a simple program which beat the game in a couple of tries. Below is a gif of the program playing minesweeper (located [here](http://minesweeperonline.com/)).


<p align="center">
	<img src="/minesweeper-play.gif"> 
</p>  

The program can be broken down into three main parts; translating the pixels on the screen into an internal representation of the board (vision), picking the next best move (general strategy), and moving the cursor to click on the chosen tile (input).  

**Vision**  
In order for our program to be able to play minesweeper, it first must be able to understand what's on the screen. First we take a screenshot of the play area. Then we need to translate those pixels into a 2d array representing the board. Luckily we don't need fancy computer vision algorithms here. It turns out we need to do is check the color of the center of each tile and compare it to the colors of each of the numbers. We can march along the board tile by tile and check which number is in each one. To compare two colors we can just take the Euclidean distance and check if it is under a certain threshold.  
The only catch is that it turns out that the empty tile is the same color as the un checked tile. This means that we don't know if a tile has zero mines around it or an unknown number of mines. What I ended up doing was flood filling the board with zeros starting from the tile that was last clicked. This is similar to how minesweeper decides how many tiles to unravel so it produces an accurate picture of the board.  

**General Strategy**  
After we process the screen we must decide where best to click. Luckily minesweeper is not GO so we don't need any fancy machine learning methods. Instead I did the following.  
First I labeled all the tiles we know to be bombs. To do this we check for each tile if the amount of unknown bombs surrounding it equal to the amount of unknown tiles around it. If they are we know all of those tiles are bombs and can label them accordingly on our board. Then we recursively perform the same check on all the explored neighbors of those tiles. After that we look at each tile and see if there are any unexplored tiles that we can be certain are not bombs. Using these methods we can solve large chunks of the minesweeper board.  
However eventually we will run out of information and we must guess. As far as I know there is no perfect way to guess. I decided to use a simple system in which we just pick a tile around the explored tile which has the lowest ratio of unknow tiles to bombs around it. I am sure that this heurtistic could be improved and would probably dramatically increase the win rate.  
  
**Input**  
Finally after we have chosen our next move we need to click the position. For this task I used [pyautogui](https://pypi.python.org/pypi/PyAutoGUI) which makes gui automation simple.  
  
If you would like more detail you should check out the [source](https://github.com/Mr4k/sweeper) on github.







