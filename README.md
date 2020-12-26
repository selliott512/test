# random-uci

## Overview

An open source simple UCI engine that moves randomly.

## UCI Options

In addition to moving randomly there are UCI options that alter its behavior. They are:

### Deterministic

If set to **true** then the random move is a function of the board state, so the same move will always be made in the same situation.

### Filter

* Item 1
* Item 2
  * Item 2a
  * Item 2b

A subset of the moves may be chosen. If the filter fails to find a valid move then it reverts to randomly choosing from all possible moves. Valid filter values are:

* **none**
*  Apply no filter so that all moves are considered. This is the default.
* **first**
*   Play the first move after sorting the move alphabetically. This may be "a2a3", for example.
* **last**
*   Play the last move after sorting the move alphabetically. This may be "h7h6", for example.
* **mirror**
*   Mirror the opponent's moves when possible.
* **rotate**
*   Rotate the opponent's move about the vertical axis when possible.

### Promotion


### Seed

See the "doc" directory for license and version information.

### Installation

The random-uci.py file found in the bin directory can be run in place. For example, if the downloaded ZIP file were expanded in the /opt derctory then
```shell
/opt/random-uci-0.9.0/bin/random-uci.py
```
random-uci.py is a UCI chess engine that requires a chess GUI that speaks the UCI protocol, or an adapter, such as [Polyglot](https://github.com/sshivaji/polyglot), that can convert from the XBoard protocol to the UCI protocol. XBoard with Polyglot as [PyChess](https://github.com/pychess/pychess) was tested.

[pychon-chess](https://github.com/niklasf/python-chess) is needed. It can be installed via pip:
```shell
https://github.com/niklasf/python-chess
```
