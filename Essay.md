# SWEN301ArchitectureProject - ChessMaster Project



## 1 Project Overview

Repository: https://github.com/ravijoshiBITS/ChessMaster
ChessMaster is a simple graphical chess game which can be played on a desktop. It features a basic AI for single player play.

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

The ChessMaster repository was created in December 2017, along with an initial commit. ChessMaster was commited to regularly until March 2018, development then slowed down. The last commit was on March 12th, and it's unclear if their are any active commiters as of April 2018.

### 1.4 Project Domain

ChessMaster is a simple GUI chess game. It features a basic MinMax AI for single player games. ChessMaster is designed for offline use on a desktop or laptop machine. Because of this, it is limited by the hardware limitations of the particular machine it is running on. 

## 2 Project Design

There are 2 packages. The Piece package provides all dummy pieces, their icons and unit-tests for their functionality. The chess package provides most of the functionality and unit-tests of the game. AI class plays against a human player in 1 player mode. Movement class handles movements of all pieces. Game and user classes retrieve and store respective statistics from files. The GUI functions are also handled. 

The project has been documented using javadocs inline with the code.

Currently known bugs are opened as issues on this repo. Please report any new 
bugs or issues that you face.

### 2.1 Component Architecture

#### 2.1.1 Core Objects

These objects are essential to the basic functionality of the program, they create the vast majority of the programs behaviour.

##### Board

##### Game

##### Movement
This class handles most of the behaviour of the program. It's by far the largest class at over 900 lines of code.

Methods:

```java
undoMove()
public boolean playMove(String from, String to)
moveTo()
castle()
promotePawn()
```

##### Graphics Handler

##### AI

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
