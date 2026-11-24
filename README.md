# tiny-chess v2

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/fynix39/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/fynix39/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in 4 KB of Python for the make.fun tiny-chess-01 bounty —
second iteration of the submitted v1 engine. It plays the house gauntlet
through the env at `MAKEFUN_ENV_URL`, searching with a deterministic
node budget per move (no clock, no randomness), so every trial replays
move for move.

## What v2 changes over v1

- **Null-move pruning** (R=2, R=3 at depth 6+) with a material guard
  against zugzwang endgames, skipped while in check.
- **Late-move reductions**: quiet moves tried late at depth 3+ are first
  searched a ply shallower on a null window and re-searched at full
  depth only if they look good.
- **Richer evaluation**: passed-pawn bonuses by rank, rook bonuses for
  open and semi-open files, a pawn-shield term for king safety, and
  doubled / isolated pawn penalties.
- **Delta pruning** in quiescence: captures that cannot reach alpha even
  with a margin are skipped.
- **Adaptive node budget**: 90k nodes in the middlegame, 70k in the
  endgame (v1 used a flat 100k); a forced move with a single legal reply
  is played at once, and a found mate ends the iterative deepening early.
- Net effect on a test middlegame: v2 reaches depth 7 where v1 reached
  depth 5 on the same position, with fewer nodes.

## Run it

```sh
MAKEFUN_ENV_URL=http://localhost:8080 python3 main.py
```

Python 3.13, standard library only, no network beyond the env, no model
calls. `main.py` (3,525 bytes) is the LZMA-packed entrypoint produced by
the kit's `makefun compress` from the readable `engine.py`; the packed
file is the whole engine and reads no other file.

## Practice results

Graded locally on the kit's three published practice instances
(env + scorer, direct trial runs, one clean run each, 24 Nov 2026):

| instance | Elo | W / D / L | wall |
| --- | --- | --- | --- |
| trial 0 | 1,364 | 3 / 1 / 2 | 1,008 s |
| trial 1 | 1,364 | 3 / 1 / 2 | 984 s |
| trial 2 | 1,451 | 2 / 4 / 0 | 979 s |
| **mean** | **1,393** | **8 / 6 / 4** | — |

(v1: 1,451 / 1,451 / 1,545, mean 1,482, 11W / 3D / 4L.) v2 loses no
more games than v1 (4 in 18) but converts fewer wins — six draws,
five of them by threefold repetition, including draws against the
250k- and 500k-node rungs in trial 2, where it went unbeaten.

Head-to-head against v1 over 10 games with alternating colours:
**v2 wins 6.5–3.5** (4W / 5D / 1L) — 3.5/5 as White, 3.0/5 as Black.
Slowest clean move ≈ 4.7 s (10 s limit); no timeout in any of the
18 practice games. Determinism verified: identical moves on three
test positions across two runs each of the packed `main.py`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=jvcw07av&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
