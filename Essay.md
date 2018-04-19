# SWEN301ArchitectureProject

## ChessMaster Project

ChessMaster is a simple graphical chess game which can be played on a desktop.

### Project Design

There are 2 packages. 
The Piece package provides all dummy pieces, their icons and unit-tests for 
their functionality.
The chess package provides most of the functionality and unit-tests of the game. 
AI class plays against a human player in 1 player mode. Movement class handles movements 
of all pieces. Game and user classes retrieve and store respective statistics 
from files. The GUI functions are also handled. 

The project has been documented using javadocs inline with the code.

Currently known bugs are opened as issues on this repo. Please report any new 
bugs or issues that you face.

### Bugs

* File saving doesn't work on Windows.
* Undo doesn't remove highlighted cells.
* In certain games against AI, one can kill AI's king, and the game still proceeds! Likely due to a bug in generating moves of other pieces, which allows some unallowable moves while the king is in check.
* In one player mode, at times, AI takes longer (up to 50 seconds) to play its move, and human move is not reflected on the board till the AI plays its move.
The reason for this bug is unknown.

### Contributors

* Ravishankar Joshi, for all the code and design of the project.
* Anish Bhobe for some design decisions.
* Lohit Marodia and a few others for play-testing the game and reporting bugs.
