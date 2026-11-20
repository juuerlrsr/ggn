# Recording games over the board

A guide to [RFC GGN-1](../spec/ggn.md). It adds nothing to the format: it suggests how to record a game played in person, on a physical board, so the result is a normal GGN record.

## Who records: the arbiter

In Game of the Generals only the **arbiter** sees both armies, so the arbiter is the one who can write a `full` record. The players can still keep their own records (`south` or `north`), because every move and every outcome is announced openly.

| Who | Sees | Can write |
| --- | --- | --- |
| Arbiter | Both armies | A `full` record |
| South or North | Their own army, and every outcome | Their own record (`south` or `north`) |
| Spectator | Moves and outcomes only | A `public` record |

## Label the board

Name the squares as in the spec (section 6): files `a`, `b`, `c` … from **South's left**, and ranks `1`, `2`, `3` … from **South's side**. Write the letters and numbers on the board's edges, or put a printed strip beside it, so everyone reads squares the same way.

## The arbiter's checks

The arbiter is the only one who can check the game against both armies, so the record is only as good as these checks. Ticking them off on the scoresheet shows they were done.

Before play:

- **Setups:** each army has exactly 21 pieces of the right ranks (one of each officer, 2 Spies, 6 Privates, the Flag), all in that side's three home rows, and the board matches the setup card square by square.
- **Board:** the files and ranks are labeled from South's side, and both players agree which side is South.
- **Header:** the players, the first mover and the time control are written before the first move.

During play:

- **Each move:** the piece belongs to the side to move and goes one square forward, back, left or right.
- **Each challenge:** the outcome is resolved from the setup cards, not from memory, and announced as "won", "lost" or "both removed" without naming ranks.
- **Promotion and the Flag:** under rules that have them, the arbiter checks the conditions (for example, no enemy piece beside a Flag at the opposite end) before announcing the result.
- **Position checks:** every 20 turns or so, the arbiter checks that the pieces on the board match the scoresheet, by counting each side's pieces and checking a few squares. A mistake found early is easy to put right.
- **Corrections:** a mistake found during play is corrected on the sheet with a short note beside the move, and both players are told.

After the game:

- **Replay:** the arbiter checks that the moves replay from the setups to the final position. Typing the record into a GGN reader does this automatically: a move that doesn't fit (from an empty square, or a `<` or `x` onto an empty square) points straight to a writing mistake.
- **Result:** the result and the way the game ended match the last moves.
- **Signatures:** both players and the arbiter sign the scoresheet.

## Before play: the setups

Each player fills a **setup card** while deploying: a small grid of their own rows with a rank code in each occupied square. They hand it to the arbiter face down. The arbiter copies it into `SouthSetup` or `NorthSetup`, or simply keeps the cards with the scoresheet.

```
North setup card (classic board, North's three rows)

      a    b    c    d    e    f    g    h    i
  8  FLAG PVT   1G  SPY   4G   ·   PVT  CPT   ·
  7  PVT  2LT   2G  COL   ·   LTC  PVT   ·   SGT
  6   ·   1LT  SPY   5G  PVT  MAJ   ·    3G  PVT
```

## During play: the scoresheet

For each move, the arbiter resolves the challenge as usual and writes **from square, symbol, to square**: `>` for a move or a win, `<` when the attacker lost, `x` when both were removed. No ranks are written, so the scoresheet can be shown to anyone.

```
GGN scoresheet          Event: Club night       Round: 3       Date: 1975.06.12
South: A. Cruz          North: B. Santos        First move: South

Turn | South       | North
   1 | e2 > e3     | g6 > g5
   2 | e3 > e4     | h6 > h5
   3 | e4 > e5     | g5 > g4
   …
  10 | b8 > a8 #   |

Result: 1-0      Termination: flag_captured      Arbiter: ______________
```

Optional columns: the clock time after each move, for `[%clk]`, and short notes, written as comments.

## Shorthand on paper

Writing during play is easier with a few shorthands. They are for paper only: the arbiter writes the record in full GGN when typing it up, and a GGN reader does not accept them.

| Shorthand | Means | Full GGN |
| --- | --- | --- |
| `e2e3` | No symbol: moved, or attacked and won | `e2>e3` |
| `d1<d2`, `c4xc5` | Write `<` and `x` as usual; only `>` may be left out | `d1<d2`, `c4xc5` |
| `e3` | Only the target square, when exactly one of the mover's pieces stands next to it | `e2>e3` (from that piece's square) |
| `″` | Ditto: the same piece moves again, from the square it just reached | `e3>e4` |

When in doubt, write both squares: a shortened move that could mean two different moves can't be typed up.

On setup cards, two-character codes fit small boxes (the generals, `5G` to `1G`, are short already):

| Code | Short | Code | Short | Code | Short |
| --- | --- | --- | --- | --- | --- |
| `COL` | `Co` | `CPT` | `Cp` | `SPY` | `Sp` |
| `LTC` | `LC` | `1LT` | `1L` | `PVT` | `Pv` |
| `MAJ` | `Mj` | `2LT` | `2L` | `FLAG` | `F` |
| | | `SGT` | `Sg` | | |

The record itself always uses the full codes.

## Printable sheets

Two sample sheets for the classic 9 × 8 board, sized to stay small at the table: **half-letter** pages (5.5 × 8.5 in, half a short bond sheet, close to A5). Print them two to a letter sheet and cut, or on A5 paper.

- [Arbiter scoresheet (PDF)](print/ggn-arbiter-sheet.pdf), 3 pages: the game's details and both setups, up to 146 turns, a checklist of the arbiter's checks, the result and signatures.
- [Player record (PDF)](print/ggn-player-sheet.pdf), 3 pages: the player's own setup, both sides' moves with a column for notes, a tally of the player's lost pieces, and the result. It shows where the player's pieces are, so it stays out of the opponent's sight during the game.

## After the game

The arbiter types the scoresheet and setup cards into a record: the tags from the sheet's header, the setups, and the moves as movetext (`1. e2>e3 g6>g5 2. …`). Once the game is over the armies are revealed anyway, so the arbiter can share the `full` record with both players. A player's own record is the same movetext with the opponent's ranks written `?`.

## Open questions

- Whether corrections made during play should have their own tag, rather than only a comment on the ply.
- Whether the shorthands should become part of the format, so readers accept them too.
