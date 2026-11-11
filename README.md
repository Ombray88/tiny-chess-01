# tiny-chess-01 — a 4 KB Python chess engine

A strong chess engine in a single `main.py` of **4,094 bytes** (limit 4,096),
built for the [make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01)
("Build the strongest chess engine in 4 KB of Python").

## What it is

`main.py` is an lzma + base85 packed payload that unpacks to a real engine:

- **0x88 board**, negamax alpha-beta with iterative deepening to a fixed
  **200,000-node** budget (deterministic — no clock, no randomness),
  with null-move pruning (R=2, depth>=3, not in check, queen or minor
  present).
- **Transposition table** (Zobrist hashing, 64-bit, seeded PRNG) with
  exact / lower / upper bounds and TT-move ordering.
- **Move ordering**: TT move first, MVV-LVA captures, promotions, then two
  killer moves per ply, then a history heuristic (depth-squared bonus on
  beta cutoffs, capped at 6000).
- **Quiescence search**: stand-pat, all captures and promotions, and — when
  in check — all evasions, with correct checkmate detection in every window
  (the stand-pat beta cutoff is skipped while in check, so a narrow window
  can never hide a mate).
- **Evaluation**: material (P100 N320 B330 R500 Q900) + CPW Simplified
  Evaluation piece-square tables (middlegame + endgame king table, bishop
  pair +25), with endgame detection (no queens, or a queen side with at most
  one minor). Repetition and the 50-move rule score 0 inside the search.
- **Protocol**: talks to the make.fun env over HTTP (`POST /game`, `/move`,
  `/resign`), UCI move strings, fixed node budget, ~2–5 s per move.

The readable development source (same engine, unminified, with a perft
suite) is not part of the entry — `main.py` is the whole engine.

## Validation

- Perft passes all six standard positions at every depth
  (startpos, Kiwipete, en-passant, castling, promotion, and the
  2,103,487-node position 5).
- Mate tests pass (Scholar's mate, back-rank mate).
- Make/unmake state consistency verified over 30 random games
  (hash, board, piece lists, king squares, castling, en passant).
- Head-to-head vs the previous 100k-node version at 20,000 nodes
  (4 openings x both colours): 5 wins, 2 losses, 1 draw; it also reaches
  one ply deeper at the same node budget. The previous version's local
  trial-0 Elo was 1290 (6W/1D/9L over the 16-game gauntlet).
- Local trial-0, same 16 openings: 8W/1D/7L, Elo 1430 (+140 over the
  previous version). Five losses were on time under a loaded local VM;
  worst move is 4.5 s CPU, inside the 10 s limit on a dedicated core.

## How to run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8123 python3 main.py
```

Python 3.13, standard library only. No other file is read or imported.

## License

MIT — see `LICENSE`.
