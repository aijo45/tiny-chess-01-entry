# tiny-chess-01 entry

Compact alpha-beta chess engine in 4,092 bytes of Python (main.py).

## Design
- 0x88 board, pseudo-legal move generation with king-safety filter.
- Alpha-beta + iterative deepening to a FIXED total node budget (40,000
  nodes/move; no clock, so live and replay agree).
- Quiescence over captures, promotions and check evasions.
- Transposition table (Zobrist, fixed seed), MVV-LVA captures, killer
  moves, history heuristic, null-move pruning (R=3, depth>=3),
  late-move reductions, check extension.
- Eval: material P100 N320 B330 R500 Q900 + piece-square tables +
  bishop pair (+30); insufficient material (no heavy piece and <=1
  minor) scores 0.
- main.py is an lzma+base85 payload of the compact source
  (dev source: ~/workspace/makefun/tiny-chess-01-dev/engine_c.py).
  It reads only MAKEFUN_ENV_URL and talks to the env alone.

## Practice results (local env, trial-0/1, 6 games each)
- Readable engine, 40k nodes, trial-0: 2W 2D 2L = 3.0 pts, Elo 1279.
- + null-move pruning + LMR, 40k nodes, trial-0: 1W 3D 2L = 2.5 pts,
  Elo 1191. Trial-1: 2W 2D 2L = 3.0 pts, Elo 1279. Mean ~1235.
- Final main.py (compact, same logic), trial-0: 1W 3D 2L = 2.5 pts,
  Elo ~1191. No time losses, no crashes, max move ~3 s (limit 10 s).
- Floor is 1154 (enters final); house baseline (sunfish 4k) ~1365.
  This entry is above the floor but below the baseline, so it enters
  the final but is unlikely to take a paid place without more strength.

## Files
- main.py (4,092 bytes) — the entry. Python 3.13 stdlib only.
