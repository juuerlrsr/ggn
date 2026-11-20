# RFC GGN-1: Generals Game Notation (GGN)

| | |
| --- | --- |
| Request for Comments | GGN-1 |
| Category | Proposed standard |
| Status | Draft |
| Version | 1.0.0-draft.3 |
| Author | juuerlrsr |
| Date | November 2026 |
| License | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public domain) |

## Abstract

GGN is a plain-text format for recording games of **Game of the Generals** (Salpakan). It is inspired by two chess notations. From [Portable Game Notation (PGN)](http://www.saremba.de/chessgml/standards/pgn/pgn-complete.htm) it takes the shape of a record: a list of tags followed by the moves, easy for people to read and simple for programs to write. From [algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_%28chess%29) it takes the names of squares: a file letter and a rank number, such as `e4`.

The **core standard** (Part 1) describes the classic game. Other rule sets are **extensions** that a record declares with a `Variant` tag; Part 2 defines the extension for **0GG**. A reader that knows only the core can still replay any record, because setups and moves are always written the core way. Part 3 describes how 0GG uses the format, and Part 4 proposes how to record games played on a physical board.

GGN is **independent of how a game looks**: it works the same for any board (wooden, paper, printed or on screen), any colors for the two sides, and any design of pieces.

## Status of This Memo

This document is a **draft Request for Comments**: a proposed standard, published for review. While it is a draft it may change in any way, from one draft to the next; the first final version will be 1.0.0, and from then on a published version never changes. Comments and suggestions are welcome; send them to the author.

Parts 1 and 2 are **normative**: they define the format. The Abstract, "At a glance", Part 3 (how 0GG uses the format) and Part 4 (recording games on a physical board) are **informative**: they explain and describe practice, and add no requirements.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 ([RFC 2119](https://www.rfc-editor.org/rfc/rfc2119), [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174)) when, and only when, they appear in all capitals, as shown here.

## At a glance

A record has a **tag section** (`[Key "value"]` lines), a blank line, and the **movetext**:

```
[Event "Example"]
[Site "0GG"]
[Date "2026.11.19"]
[Round "-"]
[South "Ana"]
[North "Ben"]
[Result "0-1"]
[GGN "1.0.0-draft.3"]
[Variant "0GG"]
[Board "5x6"]
[FirstMover "South"]
[Visibility "full"]
[SouthSetup "a1:SGT b1:PVT c1:FLAG d1:2LT a2:MAJ c2:SPY e2:CPT"]
[NorthSetup "b5:COL c5:SPY d5:1LT e5:LTC a6:PVT c6:FLAG e6:SGT"]

1. c2>c3 d5>d4 2. c3>c4 d4>d3 3. c4xc5 d3>d2 4. d1<d2 d2>d1 5. e2>e3 d1>c1# 0-1
```

- The tags name the players, the result, the rules and **where every piece started**.
- Each move names two squares, and one symbol says how it ended: `>` moved or won, `<` the attacker lost, `x` both pieces were removed.
- Moves never contain ranks. That keeps hidden pieces hidden, and still lets any program replay the game exactly.

Files **SHOULD** use the extension `.ggn` and **MUST** be encoded in UTF-8. A file **MAY** hold several records one after another, each starting with its `Event` tag.

## Part 1: Core standard (classic Game of the Generals)

### The classic game, in brief

Two players each have 21 pieces on a **9 × 8 board** and deploy them, hidden, in their first three rows. An arbiter resolves each challenge without revealing ranks: the higher rank removes the lower, equal ranks remove each other, the Spy removes every officer (Sergeant and up) but loses to the Private, and any piece removes the Flag (a Flag challenging the enemy Flag wins). A player wins by capturing the enemy Flag or by bringing their own Flag to the far row (under the traditional rule, a Flag that arrives next to an enemy piece must survive one more turn).

GGN records what happened; it does not enforce rules. Where rule sets differ, the `Variant` tag says which one applies.

### Pieces

| Code | Piece | Classic count |
| --- | --- | --- |
| `G5` | Five-Star General | 1 |
| `G4` | Four-Star General | 1 |
| `G3` | Three-Star General | 1 |
| `G2` | Two-Star General | 1 |
| `G1` | One-Star General | 1 |
| `COL` | Colonel | 1 |
| `LTC` | Lieutenant Colonel | 1 |
| `MAJ` | Major | 1 |
| `CPT` | Captain | 1 |
| `1LT` | First Lieutenant | 1 |
| `2LT` | Second Lieutenant | 1 |
| `SGT` | Sergeant | 1 |
| `PVT` | Private | 6 |
| `SPY` | Spy | 2 |
| `FLAG` | Flag | 1 |

A rank that the record doesn't reveal **MUST** be written `?`.

Records **MUST** use the codes above, including `G5` to `G1` for the generals. How an app labels pieces on screen is its own choice and does not affect records (0GG shows `5G` to `1G`).

### Independent of presentation

A record holds only **squares**, **sides by position** (South and North) and **rank codes**. It says nothing about how the game looked, so the same record describes a game played on any set:

- **Board:** any material, size or style: a wooden board, a printed sheet, a digital board.
- **Colors:** either color may play either side; the colors of the pieces, the squares or the two armies are not part of the record.
- **Pieces:** any design (traditional insignia, icons, numbers or text, wooden pieces or printed tiles), and any way of labeling them on screen (0GG, for example, labels the generals `5G` to `1G`).

Standard tags **MUST NOT** be used to record such details. A writer **MAY** note them in a comment or in an extra tag of its own, which readers keep but need not understand.

### Board and sides

- Squares **MUST** be named as in chess's algebraic notation: file letter from `a`, rank number from `1`. The classic board runs `a1` to `i8`.
- **South** plays from the low rows (its far row is the last row); **North** plays from the top rows. GGN names sides by position, not by piece color, since sets differ in color and either side may move first.

The 0GG board, 5 × 6, with the armies from the example above (North at the top):

```
  6 │ PVT   ·   FLAG   ·   SGT
  5 │  ·   COL  SPY   1LT  LTC
  4 │  ·    ·    ·     ·    ·
  3 │  ·    ·    ·     ·    ·
  2 │ MAJ   ·   SPY    ·   CPT
  1 │ SGT  PVT  FLAG  2LT   ·
    └───────────────────────────
       a    b    c     d    e
```

### Tags

The first seven are PGN's Seven Tag Roster. Tags marked "yes" under Required are **REQUIRED**; the others are **OPTIONAL**. Readers **MUST** keep tags they don't know.

| Tag | Required | Value |
| --- | --- | --- |
| `Event` | yes | What the game was, e.g. `Club night`, `Private match`; `?` if unknown |
| `Site` | yes | Where it was played; `?` if unknown |
| `Date` | yes | Start date, `YYYY.MM.DD` (UTC when known); unknown parts as `??` |
| `Round` | yes | Round in an event, or `-` |
| `South`, `North` | yes | Player names; `?` if unknown |
| `Result` | yes | `1-0` South won, `0-1` North won, `1/2-1/2` draw, `*` no result (unfinished, aborted) |
| `GGN` | yes | The version of this standard the record follows, e.g. `1.0.0-draft.3` |
| `Variant` | no | The rule set, if not classic, e.g. `0GG`. **No `Variant` tag means classic** |
| `Board` | no | `columns x rows`; default `9x8` |
| `FirstMover` | yes | `South` or `North` |
| `TimeControl` | no | As in PGN: base seconds `+` increment seconds, e.g. `600+0`; `-` for no clock |
| `UTCTime` | no | Start time, `HH:MM:SS` UTC |
| `Termination` | no | Why it ended, e.g. `flag_captured`, `flag_reached`, `resign`, `timeout`, `agreed`, `abandoned` |
| `Visibility` | yes | Which ranks the record shows: `full`, `south`, `north` or `public` (see below) |
| `SouthSetup`, `NorthSetup` | yes | Each piece where it stood when play began, as `square:rank`, e.g. `a1:FLAG e2:G5` |
| `Annotator` | no | Who added comments |

### Visibility

Ranks are secret during play, so a record states whose knowledge it holds:

- `full`: every rank (a record written after the game, with both armies revealed).
- `south` / `north`: one player's own record: their ranks; the opponent's are `?`.
- `public`: no ranks; every setup rank is `?` (a spectator's record).

The same game, as South's own record:

```
[Visibility "south"]
[SouthSetup "a1:SGT b1:PVT c1:FLAG d1:2LT a2:MAJ c2:SPY e2:CPT"]
[NorthSetup "b5:? c5:? d5:? e5:? a6:? c6:? e6:?"]
```

and as a spectator's:

```
[Visibility "public"]
[SouthSetup "a1:? b1:? c1:? d1:? a2:? c2:? e2:?"]
[NorthSetup "b5:? c5:? d5:? e5:? a6:? c6:? e6:?"]
```

The movetext is the same in all three. Moves never contain ranks, so **every record replays exactly, square by square**, whatever its visibility.

### Movetext

Plies **MUST** alternate strictly, starting with `FirstMover`. Each turn is numbered and holds the first mover's ply, then the other's: `1. e2>e3 g6>g5 2. …`.

| Token | Meaning |
| --- | --- |
| `e2>e3` | Moved to an empty square, or challenged and won (the defender is removed). Which one follows from whether `e3` was occupied |
| `e5<e6` | Challenged and lost: the attacker is removed |
| `e5xe6` | Equal ranks: both pieces are removed |
| `#` | Suffix: this move ended the game on the board |
| `--` | Pass: the player had no legal move (if the rule set allows passing) |
| `{…}` | Comment, after the ply it describes. `[%clk m:ss]` is the mover's clock after the move; `[%think s]` the seconds they took |
| `; …` | Comment to the end of the line |
| `N.` | Turn number (readers **SHOULD** ignore it when replaying) |
| `1-0` `0-1` `1/2-1/2` `*` | Result, at the end; **MUST** match the `Result` tag |

Moves **MUST NOT** contain ranks; the only exception is the promotion token of the 0GG extension (Part 2), which is governed by `Visibility`. Readers **MUST** ignore comments and annotations they don't understand. Writers **SHOULD** wrap movetext at 80 columns and keep each turn number on the same line as its move.

### Reading moves, step by step

The example game from the top, ply by ply:

| Ply | Token | What happened |
| --- | --- | --- |
| 1 | `c2>c3` | South's Spy steps forward to an empty square |
| 1 | `d5>d4` | North's First Lieutenant steps forward |
| 2 | `c3>c4` | South's Spy again |
| 2 | `d4>d3` | North's First Lieutenant again |
| 3 | `c4xc5` | South's Spy attacks North's Spy: equal ranks, both removed |
| 3 | `d3>d2` | North's First Lieutenant reaches South's side |
| 4 | `d1<d2` | South's Second Lieutenant attacks it and loses: only the attacker is removed |
| 4 | `d2>d1` | North's First Lieutenant takes the empty square |
| 5 | `e2>e3` | South's Captain steps forward |
| 5 | `d1>c1#` | North's First Lieutenant attacks the Flag and wins; the game ends, `0-1` |

A spectator sees exactly these squares and symbols, but none of the ranks.

### Comments and clocks

Comments go in braces after the ply they describe. Clock (`[%clk]`) and thinking time (`[%think]`) annotations go inside them:

```
1. c2>c3 {[%clk 2:54] [%think 5.9]} d5>d4 {[%clk 2:52] [%think 7.8]}
2. c3>c4 {a bold Spy} d4>d3 ; North heads for the Flag
```

### Replaying

From the two setups, apply each ply in order to the side whose turn it is:

- `>`: remove any enemy piece on the target square, then move the piece there.
- `<`: remove the attacker.
- `x`: remove the attacker and the defender.

No ranks or rules are needed. A `<` or `x` whose target square is empty, or a ply from a square the mover doesn't occupy, makes the record invalid; readers **MUST** reject it.

### Example (classic)

```
[Event "Club night"]
[Site "Manila"]
[Date "1975.06.12"]
[Round "3"]
[South "A. Cruz"]
[North "B. Santos"]
[Result "1-0"]
[GGN "1.0.0-draft.3"]
[FirstMover "South"]
[Termination "flag_captured"]
[Visibility "full"]
[SouthSetup "a1:FLAG b1:PVT c1:PVT d1:SPY e2:G5 f2:G4 g2:G3 h3:G2 i3:G1"]
[NorthSetup "a8:FLAG e7:COL f7:SPY g6:PVT h6:LTC"]

1. e2>e3 g6>g5 2. e3>e4 h6>h5 3. e4>e5 g5>g4 4. e5>e6 f7>f6
5. e6>e7 {beats the Colonel} f6>f5 6. e7>e8 h5>h4 7. e8>d8 g4>g3
8. d8>c8 f5>f4 9. c8>b8 h4<h3 10. b8>a8# 1-0
```

(An illustrative record with the setups shortened; a real classic setup lists all 21 pieces a side. At `9. … h4<h3`, North's Lieutenant Colonel attacks South's Two-Star General and loses.)

## Part 2: Extension 0GG

0GG is a fast game based on Game of the Generals: a 5 × 6 board, small armies chosen before each game, optional rules and clocks. Its records carry `[Variant "0GG"]` and `[Board "5x6"]`, use the core tags and movetext unchanged, and add the tags and token below.

### Rules that differ from classic

- **Ranks:** Colonel and below (`COL` … `SGT`, `SPY`, `PVT`, `FLAG`); no Generals.
- **Armies:** 6 or 7 pieces, chosen before the game in one of several ways (see `Mode`), with at most 2 Spies.
- **Flag on the far row** wins at once if no enemy piece is beside it, otherwise the moment none is.
- **Repetition draws:** the same position three times, or the same move three times within a player's last five.
- **Passing:** a player with no legal move passes (`--`).
- **Optional rules** set per game (`Rules`): the Flag can beat a Private when it attacks, and Private promotion.

### Extension tags

Writers **SHOULD** write them after the core tags, in this order.

| Tag | Required | Value |
| --- | --- | --- |
| `Mode` | yes | How armies were made: `draft4` (Draft), `common4` (Open draft), `random4` (All random), `mirror6` (Mirror). Readers **SHOULD** also accept the earlier modes `draft3`, `common3`, `random3` and `mirror5` |
| `Rules` | yes | Every rule toggle: `FlagAttacksPrivate`, `Promotion`, `ReplaceLostRanks`, `PromoteToSpy`, each `=on` or `=off`; then, for All random and Mirror games with pieces every army is sure to get (the game's **Always include** setting), `FixedTiles=` and the ranks, comma-separated (for example `FixedTiles=SPY,PVT`), left out when there are none; then, for All random games, `Reveal=` and the number of pieces each player shows the opponent before any reroll, `0` to `4` (for example `Reveal=3`); `?` when unknown |
| `Timers` | no | Custom timers only: `mulligan=` (the redraw or reroll step) `draft= pick= setup= idle= grace=`, in seconds, or `off` |
| `Pool` | open draft | The face-up pool, e.g. `COL SPY PVT …` |
| `Picks` | open draft | Picks in order, side and rank: `N:SPY S:COL S:LTC …` (`S` South, `N` North) |
| `SouthDeal`, `NorthDeal` | draft, random, mirror | The first hand dealt |
| `SouthMulligan`, `NorthMulligan` | draft, random | The new hand, only if that side redrew (Draft) or rerolled (All random) |
| `SouthKeep`, `NorthKeep` | draft | The pieces kept from the final hand |
| `SouthShown`, `NorthShown` | random | The pieces each side showed the opponent (public) |
| `MatchId`, `Room` | no | Server archive identifiers |

A tag whose Required column names modes is used only in those modes, and is **REQUIRED** in them, with two exceptions: the `Mulligan` tags appear only when that side redrew or rerolled, and a side's private tags (`Deal`, `Mulligan`, `Keep`) **MUST** appear only when `Visibility` shows that side. In mirrored modes both sides' `Deal` is the same set.

`TimeControl` **MUST** be written (`-` with no game clock). A game with no time limits at all is written with `TimeControl` `-` and every timer `off`:

```
[TimeControl "-"]
[Timers "mulligan=off draft=off pick=off setup=off"]
```

Setup ranks **MUST** be the ranks when the battle began: a Private promoted later is still `PVT` in its setup.

A classic game played in 0GG is written as a plain core record: no `Variant`, no extension tags.

### Extension token: promotion

| Token | Meaning |
| --- | --- |
| `=` | Suffix after the target square: the Private was promoted, rank not shown, e.g. `e5>e6=` |
| `=RANK` | The same, with the rank it became, e.g. `e5>e6=MAJ` |

That a promotion happened is **public**: the game shows it to both players, so every record **MUST** write at least `=`. The rank chosen is **secret** until the game ends, so `=RANK` **MUST** appear only when `Visibility` shows the promoting side (any `full` record, or that player's own). Promotion follows the challenge (a Private fights as a Private), so `=` **MUST NOT** follow `<` or `x`. When replaying, the piece's rank becomes `RANK`, or `?` if not shown. Staying a Private is written as a plain move, without `=`.

The same ply in three records:

| Record | Written as |
| --- | --- |
| `full`, or South's own | `4. e5>e6=MAJ` |
| North's own, or `public` | `4. e5>e6=` |

### Example: Draft

Each player keeps 4 of 6 dealt pieces, in secret, on top of a Flag, a Spy and a Private.

```
[Event "Example"]
[Site "0GG"]
[Date "2026.11.19"]
[Round "-"]
[South "Ana"]
[North "Ben"]
[Result "0-1"]
[GGN "1.0.0-draft.3"]
[Variant "0GG"]
[Board "5x6"]
[FirstMover "South"]
[TimeControl "180+0"]
[UTCTime "10:00:00"]
[Termination "flag_captured"]
[Visibility "full"]
[SouthSetup "a1:SGT b1:PVT c1:FLAG d1:2LT a2:MAJ c2:SPY e2:CPT"]
[NorthSetup "b5:COL c5:SPY d5:1LT e5:LTC a6:PVT c6:FLAG e6:SGT"]
[Mode "draft4"]
[Rules "FlagAttacksPrivate=on Promotion=off ReplaceLostRanks=on PromoteToSpy=on"]
[SouthDeal "MAJ CPT 2LT SGT COL PVT"]
[SouthKeep "MAJ CPT 2LT SGT"]
[NorthDeal "COL LTC 1LT SGT MAJ SPY"]
[NorthKeep "COL LTC 1LT SGT"]

1. c2>c3 d5>d4 2. c3>c4 d4>d3 3. c4xc5 d3>d2 4. d1<d2 d2>d1 5. e2>e3 d1>c1# 0-1
```

### Example: Open draft, with a promotion

Both armies start with a Flag and 2 Privates, then take turns picking from a face-up pool. North moves second in the battle, so North picks first and last. Promotion is on: South's Private walks up the empty e-file and becomes a Major.

```
[Event "Example"]
[Site "0GG"]
[Date "2026.11.19"]
[Round "-"]
[South "Ana"]
[North "Ben"]
[Result "*"]
[GGN "1.0.0-draft.3"]
[Variant "0GG"]
[Board "5x6"]
[FirstMover "South"]
[TimeControl "180+0"]
[UTCTime "10:00:00"]
[Visibility "full"]
[SouthSetup "a1:FLAG b1:PVT c1:LTC d1:CPT a2:SGT c2:SPY e2:PVT"]
[NorthSetup "a5:PVT b5:COL c5:SPY d5:MAJ a6:FLAG b6:PVT c6:1LT"]
[Mode "common4"]
[Rules "FlagAttacksPrivate=on Promotion=on ReplaceLostRanks=on PromoteToSpy=on"]
[Pool "COL LTC MAJ CPT 1LT 2LT SGT SPY SPY PVT"]
[Picks "N:COL S:SPY S:LTC N:SPY N:MAJ S:CPT S:SGT N:1LT"]

1. e2>e3 a5>a4 2. e3>e4 a4>a3 3. e4>e5 a3>b3 4. e5>e6=MAJ b3>b2 *
```

(Unfinished: `*`.)

### Example: All random, with a reroll

Each army is a Flag plus 6 random pieces. With `Reveal=3` (the default), each player sees 3 of the other's; South rerolled, so South shows 4. North moves first here.

```
[Event "Example"]
[Site "0GG"]
[Date "2026.11.19"]
[Round "-"]
[South "Ana"]
[North "Ben"]
[Result "*"]
[GGN "1.0.0-draft.3"]
[Variant "0GG"]
[Board "5x6"]
[FirstMover "North"]
[TimeControl "180+0"]
[UTCTime "10:00:00"]
[Visibility "full"]
[SouthSetup "a1:PVT c1:FLAG e1:PVT a2:CPT b2:COL c2:SPY d2:SGT"]
[NorthSetup "b5:MAJ c5:2LT d5:SPY e5:1LT a6:PVT c6:FLAG e6:PVT"]
[Mode "random4"]
[Rules "FlagAttacksPrivate=on Promotion=off ReplaceLostRanks=on PromoteToSpy=on Reveal=3"]
[SouthDeal "MAJ PVT PVT SGT 1LT PVT"]
[SouthMulligan "PVT COL SPY SGT PVT CPT"]
[NorthDeal "MAJ 2LT SPY PVT PVT 1LT"]
[SouthShown "COL SGT PVT CPT"]
[NorthShown "SPY MAJ PVT"]

1. c5>c4 c2>c3 2. c4<c3 *
```

(At `2. c4<c3`, North's Second Lieutenant attacks South's Spy and loses.)

## Part 3: How 0GG is built on GGN (informative)

0GG uses GGN as its one format for finished games: what the server keeps, what players save and share, and what the analysis board opens.

### One record per game

- **Server archive.** Every online game is logged as it happens and written as a `full` record when it ends, with `MatchId` and `Room`, the private draft tags and each move's clock (`[%clk]`) and thinking time (`[%think]`).
- **Your own copy.** On the end screen, the move history's share button offers **Copy game record** and **Download .ggn**. Both armies are shown once a game is over, so this is also a `full` record.
- **Classic** games are written as plain core records, readable by any GGN reader.

### Hidden information

During a game each device knows only its own ranks; the server never sends the opponent's. GGN fits this: moves carry no ranks, so a record replays square by square whatever its `Visibility`, and ranks are added only where they are known. Promotion follows the same rule: that it happened is public (`=`), the new rank is written only where it may be seen (`=RANK`).

### Replay and analysis

- **Analyze game** on the end screen opens the finished game's record on the analysis board.
- The **analysis board** opens any `.ggn` file, or pasted text, and steps through it move by move. It can hide one side's ranks or swap sides, and shows the pieces each side lost.
- Replaying needs no rules: the board is rebuilt from the setups and the `>`, `<` and `x` moves alone. The rules (from `Variant`, `Mode` and `Rules`) are used only to check the record and to show what each piece could beat.

### Settings in the record

A record holds everything needed to play the same game again: the mode (`Mode`), the rule toggles (`Rules`), the clock (`TimeControl`) and any custom timers (`Timers`). A record with an unknown `Variant` can still be replayed; tags 0GG doesn't know are kept.

## Part 4: Recording games over the board (informative)

> **Suggested practice.** This part adds nothing to the format. It proposes how to record a game played in person, on a physical board, so the result is a normal GGN record.

### Who records: the arbiter

In Game of the Generals only the **arbiter** sees both armies, so the arbiter is the one who can write a `full` record. The players can still keep their own records (`south` or `north`), because every move and every outcome is announced openly.

| Who | Sees | Can write |
| --- | --- | --- |
| Arbiter | Both armies | A `full` record |
| South or North | Their own army, and every outcome | Their own record (`south` or `north`) |
| Spectator | Moves and outcomes only | A `public` record |

### Label the board

Name the squares as in Part 1: files `a`, `b`, `c` … from **South's left**, and ranks `1`, `2`, `3` … from **South's side**. Write the letters and numbers on the board's edges, or put a printed strip beside it, so everyone reads squares the same way.

### The arbiter's checks

The arbiter is the only one who can check the game against both armies, so the record is only as good as these checks. Ticking them off on the scoresheet shows they were done.

Before play:

- **Setups:** each army has exactly 21 pieces of the right ranks (one of each officer, 2 Spies, 6 Privates, the Flag), all in that side's three home rows, and the board matches the setup card square by square.
- **Board:** the files and ranks are labeled from South's side, and both players agree which side is South.
- **Header:** the players, the first mover and the time control are written before the first move.

During play:

- **Each move:** the piece belongs to the side to move and goes one square forward, back, left or right.
- **Each challenge:** the outcome is resolved from the setup cards, not from memory, and announced as "won", "lost" or "both removed" without naming ranks.
- **Promotion and the Flag:** under rules that have them, the arbiter checks the conditions (for example, no enemy piece beside a Flag on the far row) before announcing the result.
- **Position checks:** every 20 turns or so, the arbiter checks that the pieces on the board match the scoresheet, by counting each side's pieces and checking a few squares. A mistake found early is easy to put right.
- **Corrections:** a mistake found during play is corrected on the sheet with a short note beside the move, and both players are told.

After the game:

- **Replay:** the arbiter checks that the moves replay from the setups to the final position. Typing the record into a GGN reader does this automatically: a move that doesn't fit (from an empty square, or a `<` or `x` onto an empty square) points straight to a writing mistake.
- **Result:** the result and the way the game ended match the last moves.
- **Signatures:** both players and the arbiter sign the scoresheet.

### Before play: the setups

Each player fills a **setup card** while deploying: a small grid of their own rows with a rank code in each occupied square. They hand it to the arbiter face down. The arbiter copies it into `SouthSetup` or `NorthSetup`, or simply keeps the cards with the scoresheet.

```
North setup card (classic board, North's three rows)

      a    b    c    d    e    f    g    h    i
  8  FLAG PVT   G1  SPY   G4   ·   PVT  CPT   ·
  7  PVT  2LT   G2  COL   ·   LTC  PVT   ·   SGT
  6   ·   1LT  SPY   G5  PVT  MAJ   ·    G3  PVT
```

### During play: the scoresheet

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

### Shorthand on paper

Writing during play is easier with a few shorthands. They are for paper only: the arbiter writes the record in full GGN when typing it up, and a GGN reader does not accept them.

| Shorthand | Means | Full GGN |
| --- | --- | --- |
| `e2e3` | No symbol: moved, or attacked and won | `e2>e3` |
| `d1<d2`, `c4xc5` | Write `<` and `x` as usual; only `>` may be left out | `d1<d2`, `c4xc5` |
| `e3` | Only the target square, when exactly one of the mover's pieces stands next to it | `e2>e3` (from that piece's square) |
| `″` | Ditto: the same piece moves again, from the square it just reached | `e3>e4` |

When in doubt, write both squares: a shortened move that could mean two different moves can't be typed up.

On setup cards, two-character codes fit small boxes:

| Code | Short | Code | Short | Code | Short |
| --- | --- | --- | --- | --- | --- |
| `G5` | `5G` | `COL` | `Co` | `SGT` | `Sg` |
| `G4` | `4G` | `LTC` | `LC` | `SPY` | `Sp` |
| `G3` | `3G` | `MAJ` | `Mj` | `PVT` | `Pv` |
| `G2` | `2G` | `CPT` | `Cp` | `FLAG` | `F` |
| `G1` | `1G` | `1LT` | `1L` | | |
| | | `2LT` | `2L` | | |

The record itself always uses the full codes.

### Printable sheets

Two sample sheets for the classic 9 × 8 board, sized to stay small at the table: **half-letter** pages (5.5 × 8.5 in, half a short bond sheet, close to A5). Print them two to a letter sheet and cut, or on A5 paper.

- [Arbiter scoresheet (PDF)](print/ggn-arbiter-sheet.pdf), 3 pages: the game's details and both setups, up to 146 turns, a checklist of the arbiter's checks, the result and signatures.
- [Player record (PDF)](print/ggn-player-sheet.pdf), 3 pages: the player's own setup, both sides' moves with a column for notes, a tally of the player's lost pieces, and the result. It shows where the player's pieces are, so it stays out of the opponent's sight during the game.

### After the game

The arbiter types the scoresheet and setup cards into a record: the tags from the sheet's header, the setups, and the moves as movetext (`1. e2>e3 g6>g5 2. …`). Once the game is over the armies are revealed anyway, so the arbiter can share the `full` record with both players. A player's own record is the same movetext with the opponent's ranks written `?`.

### Open questions

- Whether corrections made during play should have their own tag, rather than only a comment on the ply.
- Whether the printable sheets should also come for the 0GG board, or as a single card for short games.
- Whether the shorthands should become part of the format, so readers accept them too.
- How to record penalties, such as an illegal move or a time forfeit decided by the arbiter (`Termination` covers only the most common endings).

## Versioning

The `GGN` tag holds the version of this standard, following [Semantic Versioning](https://semver.org). Drafts carry a pre-release tag (`1.0.0-draft.1`, `1.0.0-draft.2`, `1.0.0-draft.3`, …); the final version will be `1.0.0`. A reader **MUST** refuse a record whose major version is newer than its own, ignoring any pre-release tag in that comparison.

### Changelog

- **1.0.0-draft.3**: Wording only; the format is unchanged. US English throughout (color, labeling); "pieces" for what was sometimes called tiles; "reroll"; the redraw and reroll step explained where the `Mulligan` tags and the `mulligan=` timer keep their names.
- **1.0.0-draft.2**: All random games write their reveal count (`Reveal=` in `Rules`); the default is 3 pieces, up to 4.
- **1.0.0-draft.1**: First draft of Request for Comments GGN-1.
