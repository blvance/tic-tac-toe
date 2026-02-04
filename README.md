### File Overview

classes/TicTacToe.cpp implements the TicTacToe class declared in classes/TicTacToe.h. It extends the engine’s Game base class and fills in the main gameplay hooks:

- `setUpBoard()` creates the 3x3 grid, loads `square.png`, and starts the game.
- `actionForEmptyHolder()` places a piece for the current player when a square is clicked.
- `checkForWinner()` detects all 8 winning triples (rows, columns, diagonals).
- `checkForDraw()` returns true when the board is full without a winner.
- `stateString()` and `setStateString()` serialize/restore the board as a 9‑character string.
- `updateAI()` selects a move for the AI using negamax + alpha‑beta pruning.
- `stopGame()` cleans up any `Bit` objects stored in the grid.

### Board Setup

`setUpBoard()` configures a 3x3 grid and positions each `Square` with an ImVec2 offset. Each square uses `square.png` from `resources/`. The Game base class uses `_gameOptions.rowX` and `_gameOptions.rowY` so the engine knows the grid dimensions.

### Piece Placement

`actionForEmptyHolder()` is the main interaction entry point:

- Validates the target `BitHolder` is non‑null and empty.
- Builds an X or O using `PieceForPlayer(playerNumber)`.
- Positions the `Bit` at the holder’s location and assigns it to the square.

The engine calls `endTurn()` after this returns `true`, so `actionForEmptyHolder()` does not end the turn directly.

### Win/Draw Rules

`checkForWinner()` uses the helper `ownerAt(index)` to evaluate each winning triple. `checkForDraw()` reports true only when all 9 squares are occupied and no winner exists.

### State Serialization

`stateString()` and `setStateString()` convert board state to/from a 9‑character string:

- `0` = empty
- `1` = player 1 (X)
- `2` = player 2 (O)

Ordering is left‑to‑right, top‑to‑bottom (index 0 is top‑left, index 8 is bottom‑right).

### AI (MegaMax)

The AI plays as player 2 (O) and evaluates moves using:

- `negamax()` with alpha‑beta pruning.
- `aiTestForTerminalState()` to detect wins or full boards.
- `aiBoardEval()` to score terminal positions.

The AI prefers faster wins and slower losses by adjusting the score using search depth.
