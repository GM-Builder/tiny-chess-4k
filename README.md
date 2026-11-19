# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/GM-Builder/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/GM-Builder/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4,096 bytes of Python, entered in make.fun's
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) bounty ("Build the
strongest chess engine in 4 KB of Python").

`main.py` is the whole engine: an LZMA-packed Python source (written by
GM-Builder's agent, packed with the make.fun kit's `compress` command) that
plays the house gauntlet over the trial env's HTTP routes. Standard library
only, Python 3.13.

How it plays:

- 0x88 mailbox board, incremental Zobrist hashing and material/PST state
- Tapered middlegame/endgame evaluation using PeSTO's piece-square tables
  (Ronald Friederich, chessprogramming.org), quantized to fit the size cap,
  plus a passed-pawn advancement bonus
- Iterative deepening PVS with transposition-table move ordering, null-move
  pruning, late move reductions, check extensions, MVV-LVA ordering and a
  quiescence search over captures, promotions and check evasions
- Root-level repetition contempt: winning lines avoid threefold repeats
- Fixed node budget per move — no clock, no unseeded randomness — so every
  trial replays move for move

Local gauntlet: 6/6 wins vs the practice house engines (depth-2 through
nodes-500k); 3 wins + 1 draw vs nodes-1M/2M (the draw was a forced perpetual
check); zero losses.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8700 python3.13 main.py
```

The engine polls the trial env's `/game` route and answers `/move` until the
gauntlet is done. The readable engine source is uploaded with the entry's
transcripts, as the bounty's authorship rules require.

MIT license.

---

<sub>Entered on [make.fun](https://make.fun/?ref=pbermgka&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>