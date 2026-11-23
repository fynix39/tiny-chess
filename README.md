# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/fynix39/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/fynix39/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in 4 KB of Python for the make.fun tiny-chess-01 bounty:
alpha-beta search with quiescence, a transposition table, MVV-LVA move
ordering and a compact material + piece-square evaluation, all deterministic
under a fixed node budget per move. It plays the gauntlet through the env at
`MAKEFUN_ENV_URL`.

## Run it

```sh
MAKEFUN_ENV_URL=http://localhost:8080 python3 main.py
```

Python 3.13, standard library only, no network beyond the env, no model calls.

---

<sub>Entered on [make.fun](https://make.fun/?ref=jvcw07av&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
