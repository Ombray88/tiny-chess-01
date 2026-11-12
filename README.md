# Tiny Chess 01

Tiny Chess 01 is a competition entry featuring a robust 4 KB Python chess engine designed to play intelligent chess under extremely strict size constraints. Contained entirely within `main.py` (4074 bytes packed), the engine communicates with local environment servers to compete against house opponents. Distributed under the MIT License, it demonstrates how advanced search algorithms and heuristic evaluations can be packed into a micro-footprint without sacrificing competitive performance.

## How it Plays

The engine leverages a sophisticated suite of search and evaluation techniques:
- **Search**: Iterative deepening alpha-beta search capped at 200,000 nodes per move. It incorporates null-move pruning, late move reductions (LMR) with re-search, a transposition table for memoization, and quiescence search to resolve tactical captures and avoid the horizon effect.
- **Evaluation**: A balanced evaluation function combining material values, piece-square tables for positional understanding, and a small tempo bonus rewarding the side to move.

## How to Run

To run the engine and connect it to the local environment server, set the `MAKEFUN_ENV_URL` environment variable pointing to your server instance and execute `main.py`:

```bash
export MAKEFUN_ENV_URL="http://localhost:8000"
python main.py
```

## Practice Results

During local practice trials prior to submission, Tiny Chess 01 demonstrated consistent performance across matches:
- **Trial-0**: 1530 Elo
- **Trial-1**: 1564 Elo
- **Pooled Average**: 1547 Elo (measured over 32 games)

## Design Insights: Fitting Depth in 4 KB

Building a competitive chess engine within a 4 KB budget requires strict minimalism in code structure and data representation. By stripping away external dependencies, utilizing Python's built-in data structures efficiently, and writing concise, functional evaluation heuristics, Tiny Chess 01 achieves tournament-level search depth while keeping the packed source code compact enough to fit comfortably in a single code block.

## Byte budget

The competition caps the packed entry file at 4096 bytes, and this entry's `main.py` comes in at 4074 bytes packed, leaving 22 bytes to spare. The engine source is compressed using LZMA and encoded so that the entire program fits into just two lines. Maintaining a small buffer of spare bytes is essential during final tuning, as it allows for minor optimizations or bug fixes without inadvertently breaching the strict size limit.
