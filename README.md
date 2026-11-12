# Tiny Chess Engine (v4)

A compact, highly competitive 4 KB Python chess engine built for the make.fun tiny-chess competition. 

## Overview

Version 4 represents a major algorithmic leap for the engine. The primary entry point, `main.py`, is a fully self-contained packed script measuring 4,072 bytes—comfortably within the strict 4,096-byte submission limit. Using a compression pipeline combining LZMA and Base85 encoding, the entire rule set, move generator, evaluation function, and search machinery are bundled into a single file with zero external dependencies.

## How It Works

At its core, v4 runs an optimized 200,000-node alpha-beta search enhanced with industry-standard chess programming heuristics:

- **Late Move Reductions (LMR) & Re-search:** New in v4, LMR intelligently reduces the search depth for moves deemed less promising, successfully elevating the engine's peak effective search depth from 6 to 7 ply.
- **Null-Move Pruning:** Dynamically skips turns in favorable positions to aggressively prune non-tactical subtrees.
- **Transposition Table:** Caches previously evaluated board states using Zobrist hashing to eliminate redundant computations.
- **History Heuristic:** Prioritizes quiet moves that have historically caused alpha-beta cutoffs.
- **Quiescence Search:** Extends tactical evaluations past the horizon limit to resolve pending captures and prevent the horizon effect.

## Validation & Performance

Rigorous local evaluations against the 8 house anchor engines demonstrate exceptional strength:

- **Rating:** Elo 1497
- **Match Score:** 9 wins, 1 draw, and 6 losses
- **Progress:** Delivers a +67 Elo gain over v3 (Elo 1430) and an impressive +207 Elo improvement over the v1 baseline (Elo 1290).

## How to Run

The engine reads a FEN or position string from standard input and outputs the chosen best move:

```bash
python3 main.py
```

## Example

To run the engine from the starting chess position, feed the standard starting FEN via standard input:

```bash
echo "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1" | python3 main.py
```

Sample output:
```text
e2e4
```

Each move is chosen within a 200,000-node search budget, and the engine needs only Python 3 with no extra packages.
