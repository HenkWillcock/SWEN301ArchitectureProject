# SWEN301ArchitectureProject - ChessMaster Project



## 1 Project Overview

Repository: https://github.com/ravijoshiBITS/ChessMaster

ChessMaster is a simple graphical chess game which can be played on a desktop. It supports two player gameplay, and also features a basic minmax AI for single player play.

### 1.1 Build Platform

ChessMaster is written purely in Java, making it easy to build. It also comes with a runnable jar file which was used to test the project before building. The project was built on a Windows machine using the Eclipse Oxygen IDE.

#### System Specifications:

* CPU: AMD Phenom 9600 Quad-Core 2.3 GHz
* RAM: 4GB
* GPU: NVIDIA GT 1030
* OS: Windows 7 Professional Edition

### 1.2 Proof of Build

<img src="https://github.com/HenkWillcock/SWEN301ArchitectureProject/blob/master/screen_shot.PNG" alt="screen shot" width="400px"/>

It might be a bit difficult to see because all the words overlap, but I've replaced all the column labels with my name (Henk Willcock).

### 1.3 Project History

The ChessMaster repository was created in December 2017, along with an initial commit. From December 16th until January 21st, core components and basic units tests were added. These components include classes for the various pieces, the board, the player, and the graphics handler. On January 24th there was a milestone commit which made two player gameplay possible. After this, the focus of development shifted to the AI. A simple minimax algorithm was completed on Febuary 2nd, which was refined over the coming weeks. On Febuary 11th, the core game rules were completed with the addition of castling, pawn promotion, and fixes to the checkmate checker. The last few features to be added were saving and loading of games, and an undo button.

The last commit of code was on Febuary 22nd. On March 12th the developer added a README file, and hasn't commited since then. As of April, it is unclear whether development of ChessMaster is ongoing or the project has been discontinued.

### 1.4 Project Domain

The domain of ChessMaster is gaming, or more specifically, board game simulation. The project doesn't feature a powerful AI, only a simple one, so it doesn't fall into the domain of chess engine. 

#### 1.4.1 Board Game Simulations

Board game simulations are a popular type of game, despite the fact that most are based entirely on real life board games. This is interesting because it seems as though real life board games are cheaper and better experiences than a virtual ones. It's clear that board game simulations are popular however, and this is for a number of reasons. Firstly, if you already have a computer or smartphone, then the virtual version of a board game will be the cheaper option. The virtual version of a game will likely cost under $10, while the real board game could cost over $100. Secondly, virtual games are usually easier to learn because they can provide tutorials. The structure of gameplay is also built in, and doesn't need to be learnt to the same degree required by a real board game. Thirdly, virtual games remove disputes over the rules of a board game. Complicated board games often have edge cases, ambigous rules, or rule conflicts which might not be addressed anywhere in the rulebook. This can force board gamers to decide rules for themselves, and if players disagree there might not be a fair way to solve the problem. Virtual games solve this because the rules are built into the code. 

#### 1.4.2 Hardware Domain

ChessMaster is designed for offline use on a desktop or laptop machine. Because of this, it is limited by the hardware of the particular machine it is running on. ChessMaster isn't resource intensive however, almost all PCs and laptops would be able to run the program without problem, unless they are either very old or aren't functioning correctly.

#### 1.4.3 Runtime Environment Domain

A real limitation of ChessMaster is that it's written in Java and requires the Java Runtime Environment (JRE) to run. Without the JRE there is simply no way to run the program. Most machines will have the JRE installed already, and if it's not installed it can be downloaded for free.

## 2 Project Design

### 2.1 Component Architecture

#### 2.1.1 Core Objects

These objects are essential to the basic functionality of the program, they create the vast majority of the programs behaviour.

##### Board

##### Game

##### Movement

This class handles most of the behaviour of the program. It's by far the largest class at over 900 lines of code. It has useful methods which do legal moves to the game. These are:

```java
undoMove();
public boolean playMove(String from, String to);
public Move moveTo(Cell from, Cell to);
private Move castle(King king, Cell to);
private Move promotePawn(Pawn pieceToMove, Cell from, Cell to);
```

A major problem with this class is its size. Lots of behaviour implemented in this class could potientially be moved to other classes. If methods could be moved out of this class it would reduce the complexity of this class and of the program as a whole.

Potiential changes:

The methods which return lists of potiential moves for example `getKnightMoves(Knight knight)`, should be moved to their corresponding class. The 'Piece' class should have an abstract method `getMoves()`, and it would be implemented differently in each class which extends 'Piece'. The useful `movesInDir()` method could then be moved to the Piece class to be used by all the pieces. The `recomputeMoves()` and `getAllMoves()` methods then become unnecessary with this change.

The `canCastle()` and `getCastlingMove()` methods could then be incorporated into the King's `getMoves()` method as a special type of move.

##### Graphics Handler

The graphics handler is the front end of the program. It reads data from a reference to the board and draws a visual representation to the screen. A problem with this class is that it also handles the control of the game. A better way to do this would be to create a full model view controller by making this class purely for rendering from a model, and moving all other methods to either the model or controller.

##### AI

A problem with this class is the minimax method which is incredibly large.

#### 2.1.2 Data Storage Objects

These components don't have many methods beyond getters and setters and are primarily used for storing information about various aspects of the chess game.

##### Player
Used for storing player data such as name, wins, losses and total games played.

| Type    | Name         |
| ------- | ------------ |
| String  | Name         |
| Integer | Games Played |
| Integer | Games Won    |
| Integer | Games Lost   |

##### Cell
Used for storing data about a single square on the board

| Type    | Name        |
| ------- | ----------- |
| Integer | Row         |
| Integer | Column      |
| Boolean | Highlighted |

##### Move
Used for storing data about a single move.

| Type    | Name           |
| ------- | -------------- |
| Cell    | Source         |
| Cell    | Destination    |
| Piece   | On Source      |
| Piece   | On Destination |
| String  | Move Type      |
| Queen   | New Queen      |

Problems:
* Most moves won't utilise the 'New Queen' field. There should be a 'PromotionMove' class extending this one which adds this field.
* 'Move Type' shouldn't be a String, it should be an enumerated type. This would remove many oppurtunities for bugs and allow developers to see all the possible move types easily.
* The name 'On Source' isn't very descriptive, it should be renamed 'Moving Piece'.

##### Piece
Used for storing data about a single piece. This is an abstract class and has a concrete class for each type of piece (King, Queen, Bishop, Knight, Rook and Pawn). This architecture has potiential to reduce the complexity of the program, but these piece-specific concrete classes do nothing other than change the 'toString()' method of the 'Piece' class.

### 2.2 Data Structures 

### 2.3 Testing

### 2.4 Known Bugs

* File saving doesn't work on Windows.
* Undo doesn't remove highlighted cells.
* In certain games against AI, one can kill AI's king, and the game still proceeds! Likely due to a bug in generating moves of other pieces, which allows some unallowable moves while the king is in check.
* In one player mode, at times, AI takes longer (up to 50 seconds) to play its move, and human move is not reflected on the board till the AI plays its move.
The reason for this bug is unknown.

### 3 Contributors

* Ravishankar Joshi, for all the code and design of the project.
* Anish Bhobe for some design decisions.
* Lohit Marodia and a few others for play-testing the game and reporting bugs.
