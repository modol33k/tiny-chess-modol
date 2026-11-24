# kemod-sunfish-bp

A fork of Thomas Ahle's sunfish (GPL-3.0-only), tuned for the make.fun
tiny-chess-01 bounty (chess engine in <= 4096 bytes of Python).

## What changed vs sunfish

* **Env driver:** the UCI loop is replaced by the make.fun trial driver
  (HTTP env protocol). The search is a fixed 50,000-node budget per move
  (no wall clock), so every trial replays deterministically.
* **Bishop-pair bonus (+30):** the quiescence stand-pat adds +30 when the
  side to move holds both bishops, -30 when the opponent does. This is the
  only evaluation change; everything else (piece-square tables, MTD-bi
  search, null-move, IID, LMR, killer moves) is sunfish's.
* **No clock:** the deadline check is removed; the node cap is enforced
  inside the search (every 2048 nodes).

## Credit

Sunfish by Thomas Ahle and contributors, GPL-3.0-only, at commit
e396948acbd80cdcbad3dccfb07e878fba9df8f4 (`nnue_4k/pst_entry.py`).
This fork keeps sunfish's search and tables and adds the driver plus the
bishop-pair term above.

## Practice results (local, kit env server + scorer)

| trial | Elo | W-D-L |
|-------|-----|-------|
| 0 | 1545 | 4-1-1 |
| 1 | 1451 | 4-0-2 |
| 2 | 1451 | 4-0-2 |

House baseline (sunfish @ 50k nodes) practice mean: ~1365.

Practice mean (this entry): (1545+1451+1451)/3 = **1482** (+117 over baseline).
