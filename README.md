# random-uci

## Overview

An open source simple UCI engine that moves randomly.

## UCI Options

In addition to moving randomly there are UCI options that alter its behavior. They are:

### Deterministic

If set to **true** then the random move is a function of the board state, so the same move will always be made in the same situation.

### Filter

A subset of the moves may be chosen. If the filter fails to find a valid move then it reverts to randomly choosing from all possible moves. Valid values are:

* **none**
  * Apply no filter so that all moves are considered. This is the default.
* **first**
  * Play the first move after sorting the move alphabetically. This may be "a2a3", for example.
* **last**
  * Play the last move after sorting the move alphabetically. This may be "h7h6", for example.
* **mirror**
  * Mirror the opponent's moves when possible.
* **rotate**
  * Rotate the opponent's move about the vertical axis when possible.

### Promotion

The action to take when a pawn is promoted. Valid values are:

* **random**
  * Promote to a random piece. This is the default.
* **knight**
  * Promote to a knight.
* **bishop**
  * Promote to a bishop.
* **rook**
  * Promote to a rook.
* **queen**
  * Promote to a queen.

### Seed

When **Deterministic** is set to **true** this allows an alternate set of random moves to be played. All strings are valid values. All values result in a complete different set of random moves.

See the "doc" directory for license and version information.

### Installation

The following dependencies are required:

* Python 3
* python-chess

`python-chess` can be installed by:
```shell
pip install chess
```

