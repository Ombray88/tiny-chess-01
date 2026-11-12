# Tiny Chess 01

Tiny Chess 01 is a 4 KB Python chess engine built for the make.fun tiny-chess-01 competition. The engine is contained entirely within a single file, `main.py` (subject to a 4096-byte limit), which unpacks its compressed payload at import time.

## Search & Evaluation Features

- **Alpha-Beta Search**: Configured with a 100,000-node budget per move.
- **Null-Move Pruning**: Skips turns to prune fruitless branches and accelerate search depth.
- **Late Move Reductions (LMR)**: Reduces search depth for moves ordered late in the search tree.
- **Transposition Table**: Caches evaluated states to eliminate redundant calculations.
- **Quiescence Search**: Extends tactical searches on captures to resolve tactical exchanges and avoid the horizon effect.
- **Tempo Bonus**: Incorporates a small tempo bonus within the evaluation function to favor the side to move.

## Trial Results

Tiny Chess 01 was evaluated in a local 16-game trial against the house gauntlet:
- **Score**: Elo 1430 (8 wins, 1 draw, 7 losses)
- **Wall Time**: 1817 seconds (comfortably within the 3600-second trial limit)

## Byte budget

- **Engine Size**: Fits in 4072 of the 4096 allowed bytes for `main.py`.
- **Node Budget**: 100,000 per move.
- **Trial Wall Time**: 1817 seconds against a 3600-second limit.

