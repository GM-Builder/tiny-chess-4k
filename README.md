# tiny-chess-4k

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
- Fixed node budget per move — no clock, no unseeded randomness — so every
  trial replays move for move

MIT license. The readable engine source lives beside this repo in the
builder's workspace and is uploaded with the entry's transcripts, as the
bounty's authorship rules require.