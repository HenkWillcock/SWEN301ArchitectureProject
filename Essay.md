# SWEN301ArchitectureProject - ChessMaster Project



## 1 Project Overview

Repository: https://github.com/ravijoshiBITS/ChessMaster

ChessMaster is a simple graphical chess game which can be played on a desktop. It supports two-player gameplay and also features a basic minimax AI for single player play.

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

The ChessMaster repository was created in December 2017, along with an initial commit. From December 16th until January 21st, core components and basic units tests were added. These components include classes for the various pieces, the board, the player, and the graphics handler. On January 24th there was a milestone commit which made two player gameplay possible. After this, the focus of development shifted to the AI. A simple minimax algorithm was completed on February 2nd, which was refined over the coming weeks. On February 11th, the core game rules were completed with the addition of castling, pawn promotion, and fixes to the checkmate checker. The last features added were saving and loading of games and an undo button.

The last commit of code was on February 22nd. On March 12th the developer added a README file and hasn't committed since then. As of April, it is unclear whether the development of ChessMaster is ongoing or the project has been discontinued.

### 1.4 Project Domain

The domain of ChessMaster is gaming, or more specifically, board game simulation. The project doesn't feature a powerful AI, only a simple one, so it doesn't fall into the domain of chess engine. 

#### 1.4.1 Board Game Simulations

Board game simulations are a popular type of game, despite the fact that most are based entirely on real-life board games. This is interesting because it seems as though real life board games are cheaper and better experiences than virtual ones. It's clear that board game simulations are popular, however, and this is for several reasons. 

If you already have a computer or smartphone, then the virtual version of a board game will be the cheaper option. The virtual version of a game will usually cost under $10, while the real board game could cost over $100.

Furthermore, virtual games are easier to learn because they can provide in-game tutorials which teach users as they play. The structure of gameplay is also built in and doesn't need to be learnt to the same degree required by a real board game. 

In addition, virtual games remove disputes over the rules of a board game. Complicated board games often have edge cases, ambiguous rules, or rule conflicts which might not be addressed anywhere in the rulebook. This can force board gamers to decide rules for themselves, and if players disagree there might not be a fair way to solve the problem. Virtual games solve this because the rules are built into the code.

Also, virtual games can be more convenient because they remove the need for things like paper money, and can do calculations automatically. This is evident in the game monopoly. In a computer version of monopoly, when you land on a property owned by another player, the correct amount of rent is immediately transferred from your cash supply to the property owners cash supply. In the real version, players first have to check how much rent it is for the number of houses on the property. Then put together the amount using notes of various denominations, this may involve dividing large denominations into smaller ones.

Perhaps the biggest reason board game simulations are so popular, however, is AI computer opponents which allow players to play by themselves. The requirement of other players is a big barrier to playing board games. Solo play is convenient because players can play anytime they want. They can take as long as want with their turns, while their opponents move immediately. They can easily save the game for later. And they can decide the difficulty of their opponents. AI opponents in games can often feel unrealistic and beating them might not be rewarding, so many board game simulators allow for online play against either friends or random players. This allows for real life human gameplay and is more rewarding to win because players know they have beaten a real life opponent.

#### 1.4.2 Platform Domain

ChessMaster is designed for offline use on a desktop or laptop machine. Because of this, it is limited by the hardware of the particular machine it is running on. ChessMaster isn't resource-intensive, however, almost all PCs and laptops would be able to run the program without problem, unless they are either over ten years old or aren't functioning correctly.

ChessMaster doesn't allow for online play, which could potientially be a popular feature of the program. It would provide an alternate to playing against an AI opponent, which can often feel unrealistic and repetitive. It would also allow friends to play each other over the web which is convenient.

#### 1.4.3 Runtime Environment Domain

A real limitation of ChessMaster is that it's written in Java and requires the Java Runtime Environment (JRE) to run. Without the JRE there is simply no way to execute the program. This isn't a big issue however, most machines will have the JRE installed already, and if it's not installed it can be downloaded for free.

Being a jar file means ChessMaster is currently incompatible with web browsers, mobile devices, and tablets. All of which are platforms where games are popular. This is made evident by the website and mobile app 'Chess.com', which can be found here: https://www.chess.com/. This website and app allows for cross-platform online play against friends, and also against random, equally-skilled opponents. Chess.com is popular with over 15 million members.

## 2 Project Design

### 2.1 Component Architecture

##### *UML Diagram*

<img src="https://github.com/HenkWillcock/SWEN301ArchitectureProject/blob/master/class_diagram.png" alt="screen shot"/>

##### *Core Objects*

These objects are essential to the basic functionality of ChessMaster. They implement the vast majority of the program's behaviour.

#### 2.1.1 Board

This object represents the game board. Its main component is a 2D array of 'Cell' objects. It acts as the model component of a model view controller architecture. 

A problem with this class is that it also handles some of the control of the game. It is sent mouse click information by another object which it then has to use to move pieces. If this responsibility was given to a new 'Controller' class it would significatly reduce the complexity of this 'Board' class. The 'Board' class would essentially become a data storage class only which is simple.

#### 2.1.2 Game

This object is used primarily for saving the game. It has a method which can utilise the stack of moves stored in the 'Movement' object and save them to file. Given the known starting position of chess game, the program can quickly perform all the moves listed in order to get back to the position of the saved game.

#### 2.1.3 Movement

This class handles most of the behaviour of the program. It's the largest class, at over 900 lines of code. It has useful methods which do legal moves to the game. These are:

```java
undoMove();
public boolean playMove(String from, String to);
public Move moveTo(Cell from, Cell to);
private Move castle(King king, Cell to);
private Move promotePawn(Pawn pieceToMove, Cell from, Cell to);
```

A major problem with this class is its size. Lots of behaviour implemented in this class could potentially be moved to other classes. If methods could be moved out of this class it would reduce the complexity of this class and of the program as a whole.

Potential changes:

The methods which return lists of potential moves for example `getKnightMoves(Knight knight)`, should be moved to their corresponding class. The 'Piece' class should have an abstract method `getMoves()`, and it would be implemented differently in each class which extends 'Piece'. The useful `movesInDir()` method could then be moved to the Piece class to be used by all the pieces. The `recomputeMoves()` and `getAllMoves()` methods then become unnecessary with this change.

The `canCastle()` and `getCastlingMove()` methods could then be incorporated into the King's `getMoves()` method as a special type of move.

#### 2.1.4 Graphics Handler

The graphics handler is the front end of the program. It reads data from a reference to the board and draws a visual representation to the screen. This includes highlighting the selected piece and highlighting in a different colour all the squares the piece can move to in the next turn.

A problem with this class is that it also handles some of the control of the game. It has a 'Mouse Handler' object and a method `clicked(int x, int y)` which is called by the MouseHandler. The 'clicked' method takes the screen location of the mouse and determines which square has been clicked, it then tells this to the board. A better way to do this is to create a full model view controller, by making this class purely for rendering the game from a model of the board. All the other methods would then be moved to either the model or controller.

#### 2.1.5 Main

This object is the only object created by the `main()` method of the program. It handles all the GUI for setting up the chess game. First it allows choosing between a 1-player and 2-player game, and choosing which colour to play as in a solo game. It also allows the creation of a player save file, or the selection of a previously created one. The player save files feature a username which is displayed in-game, and a record of wins, losses and draws.

The 'Main' object contains fields for a Board object, a Game object, a Movement object, and a Graphics Handler object. When the game is initialised it creates instances of all of these and connects them together in the appropriate way. For example it gives the graphics handler a reference to the board so it can render it to the screen after every move. One the game begins the Main class has no further function until the game ends.

#### 2.1.6 AI

This class is responsible for determining the next move for the computer player to do. It only has one important public method `playNextMove()`. This method utilises private methods to both determine the next move and execute it. This is great encaptulation because almost all of the AI class is internal. This means interacting with the 'AI' class is very simple because there is only one method to worry about which has a clear purpose. This encaptulation also allows the developer to change the AI class completely, as long as they keep the `playNextMove()` method.

A problem with the 'AI' class is the `minimax()` method, which is incredibly large. The method is over 100 lines long and contains very complex nested loops and logic. This complexity makes alterations to the method difficult, especially for outside developers.

##### *Data Storage Objects*

These components don't have many methods beyond getters and setters and are primarily used for storing information about various aspects of the chess game.

#### 2.1.7 Player

Used for storing player data such as name, wins, losses and total games played.

| Type    | Name         |
| ------- | ------------ |
| String  | Name         |
| Integer | Games Played |
| Integer | Games Won    |
| Integer | Games Lost   |

#### 2.1.8 Cell

Used for storing data about a single square on the board

| Type    | Name        |
| ------- | ----------- |
| Integer | Row         |
| Integer | Column      |
| Boolean | Highlighted |

#### 2.1.9 Move

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
* 'Move Type' shouldn't be a String, it should be an enumerated type. This would remove many opportunities for bugs and allow developers to see all the possible move types easily.
* The name 'On Source' isn't descriptive, it should be renamed 'Moving Piece'.

#### 2.1.10 Piece

Used for storing data about a single piece. This is an abstract class and has a concrete class for each type of piece (King, Queen, Bishop, Knight, Rook, and Pawn). This architecture has potential to reduce the complexity of the program, but these piece-specific concrete classes do nothing other than change the 'toString()' method of the 'Piece' class.

### 2.2 Data Structures 

#### 2.2.1 Game Moves Stack

The 'Movement' class utilises a stack to store the past moves of the game which it uses to undo moves. This is useful because it only allows the program to access the most recent move, the one it will be undoing. After this move has been undone it can then access the move underneath it in the stack and so on.

#### 2.2.2 Board 2D Array

The 'Board' class features a 2D array of 'Cell' objects which represents the game board. This 2D array of the board is used by many other objects in the program. The graphics handler uses it to render the board. The 'Movement' object uses it to determine all the legal moves of a given piece. And the AI uses it to determine its next move.

#### 2.2.3 Copy-On-Write Array List

The 'Board' class features a 'CopyOnWriteArrayList' to store the various pieces, including the captured pieces. A CopyOnWriteArrayList is a thread safe variant of ArrayList. It achieves thread safety by having all mutative operations such as add and set create a fresh copy of the underlying array. This approach to dealing with thread concurrency has various costs associated with it. While it doesn't add any overhead when reading from the Array, it adds a large overhead when writing to it.

### 2.3 Testing

#### 2.3.1 Board Tests

These tests are for the 'Board' class and test its more complex methods.

#### 2.3.2 Cell Tests

These tests are for the 'Cell' class and test its more complex methods.

#### 2.3.3 Move Tests

These tests are for the 'Movement' class and test its more complex methods.

#### 2.3.4 Piece Tests

These tests are specific to each piece, there are several for each type of piece. These test various methods from the 'Piece' classes, such as the `toString()` method, `getMoves()` method, and the constructor of the piece.

### 2.4 Known Bugs

* File saving doesn't work on Windows.
* Undo doesn't remove highlighted cells.
* In certain games against AI, one can kill AI's king, and the game still proceeds! Likely due to a bug in generating moves of other pieces, which allows some unallowable moves while the king is in check.
* In one player mode, at times, AI takes longer (up to 50 seconds) to play its move, and the human player's move is not shown on the board till the AI plays its move.
The reason for this bug is unknown.

## 3 Contributors

* Ravishankar Joshi, for all the code and design of the project.
* Anish Bhobe for some design decisions.
* Lohit Marodia and a few others for play-testing the game and reporting bugs.
