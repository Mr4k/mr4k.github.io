---
layout: post
title:  "Regress the Chess"
date:   2016-07-23 2:50:40 -0800
categories: neural network deep learning chess training
---

<p align="center">
	<img src="/training.png"> 
</p>  

Despite the lack of posts recently I have not given up on the chess engine but it will definitely take longer than I thought. Recently I decide to try to use neural networks to enhance the engine's efficiency. So far I have tried two approaches.  

**Using a Neural Network as the Evaluation Function**  
I mentioned this idea in the last post I made about the chess engine. As I mentioned in [that post](https://mr4k.github.io/chess/project/minimax/algorithm/search/monte/carlo/2016/06/28/search.html) board game engines usually rely on two different components, search and evaluation. The search component looks ahead to try to predict the best move. However with complex games like chess we can not look at all the possible moves so eventually at some point during the search we need to start estimating how good certain far off positions are. The function which estimates how good a given state is for a player is called the evaluation function. This function takes a state (the chess board) as its input and returns a scalar estimate of how good the position is for one of the players. Intuituvely making a more accurate evaluation function will make the engine better as a whole. In state of the art chess engines, the evaluation functions are often large chunks of code, handwritten by expert chess players and tweaked by computers. Becuase I am abysmal at chess I cannot write such a function. But I though that I might be able to train a neural network to be a decent one. 

**Training**  
I downloaded a about a million games from the [fics database](http://www.ficsgames.org/download.html). I then preprocessed them by seperating them into different board positions and counting the number times black(choosen arbitarily) wins, loses and ties after getting to that position. I ended up with about 600,000 unique positions that had been visited 3 times or more. I then calculated the percentage of times black won and trained a network to approximate that function. Unfortunetly it didn't seem to work.  

**What Went Wrong**  
I think the data was a too noisy. The problem is that most games of human versus human chess can be turned around under perfect play so sometimes people should not lose when they do which adds noise to the data. I also used games between players of all ranks which means some of the games were probably just lost due to a low skill level not a bad board position. Finally the way I counted the positions was flawed because it did not take duplicates board positions into account. For example if one game had the same board configuration three times, it would count it three times as a win, loss or tie when it should only count it once.  

**Using a Neural Network to Recommend Moves**  
A couple days ago I decided to try a new approach to the problem. The idea behind this approach is simple. In some board positions there are certain moves that are really never worth making. For example if your Queen is about to be taken it very rarely makes sense to move a pawn on the other side of the board. These useless moves are obvious even to weak humans players (like myself) but usually a computer will search them to figure out that they aren't good. This takes a lot of valuable seach time which could be used to look at the promising positions in more detail. So basically the question is can we build a function which returns a probability of any given move being the right move. Over the past few days I have been attempting to answer this question.  

**The Network**  
Below is a diagram of the network I used. Can you guess where the 4096 comes from?
<p align="center">
	<img src="/network.png"> 
</p>  
The input layer consists of 516 nodes. This is because I use 8 nodes per square on the board (6 nodes to denote piece type + ownership 2 nodes) plus four extra nodes at the end to indicate castling rights. The numbers of nodes for the next few layers were basically decided arbitarily. The last layer represents every conceivable chess move. Any move in chess can be defined by a starting position and an ending position. There are 64 starting positions and 64 ending positions which produces 4096 possible moves in total. Now most of these moves are never possible once the rules are introduced but this structure gives us a nice way to enumerate any move.   
All of these layers are dense and fully connected. But what about convolutional layers? Aren't those great for image processing, a chessboard is like an image isn't it? I decided not to use convolution for two reasons. The first was that I wanted to keep it simple at first and convolution seemed like overkill. The second was that I was worried that small convolutional filters might make the program too locally focused. For instance it might not understand that a bishop can move all the way across the board.  
I also used both L2 regularizarion and dropout so that I would have to worry less about overfitting.  

**Training**  
I used 200,000 games from [KingBase](http://www.kingbase-chess.net/), a database of games between players ranked > 2000, to train the network. I took each board position and figured out the probability distributions over all the moves made from it during those games. Then I used that data to train the network for about 36 hours on my macbook (no gpu).

**Results**  
Before I write about these results I must make the disclaimer that my test set was a too small (only 61 board positions) so take them with a grain of salt. So the question is can it play chess? The answer is to an extent. Out of the 61 training examples, 56 of the networks top choices were legal moves. Not only were they legal, but some of them were recommended by hand tuned chess systems. Sometimes the network also made terrible moves. However these moves seemed to occur at the end of the game. I will post the code to build the data and train the network soon so anyone can try to replicate/analyze the results. I just need to clean it up a little first.

**Improvement**  
I will try to improve this system in a couple ways. First of all I will train it for longer on a gpu with more training data. I'm hoping this will help its early and middle game selections. Finally I will try to train the network using [policy gradients](http://www.scholarpedia.org/article/Policy_gradient_methods) or some similar method. This will allow the network to learn by palying itself and encounter a wider range of board positions that it does now. For example the network does not know how to play from a seriously disadvantaged position because pro games are usually pretty close.

