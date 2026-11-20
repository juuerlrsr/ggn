# GGN: Generals Game Notation

**RFC GGN-1 · Draft · Version 1.0.0-draft.3 · Author: juuerlrsr · License: [CC0 1.0](LICENSE)**

GGN is a plain-text format for recording games of **Game of the Generals** (Salpakan), inspired by
two chess notations: [Portable Game Notation (PGN)](http://www.saremba.de/chessgml/standards/pgn/pgn-complete.htm),
for the shape of a record, and [algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_%28chess%29), for naming squares (`e4`). A record is a list of tags followed
by the moves:

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

Moves name two squares and how the move ended (`>` moved or won, `<` the attacker lost, `x` both
removed). They never contain ranks, so hidden pieces stay hidden and any record still replays
exactly. GGN works for any board, side colors and piece design.

## Contents

| Path | What it is |
| --- | --- |
| [`spec/ggn.md`](spec/ggn.md) | The specification, RFC GGN-1 |
| [`spec/print/`](spec/print/) | Printable sheets for games on a physical board: an [arbiter scoresheet](spec/print/ggn-arbiter-sheet.pdf) and a [player record](spec/print/ggn-player-sheet.pdf) (half-letter, close to A5) |
| [`examples/`](examples/) | Example records: classic, the 0GG modes, a player's own view, several records in one file |

## Status

This is a **draft**. It may change in any way between drafts (`1.0.0-draft.3`, `-draft.4`, …)
until the first final version, `1.0.0`. After that, versions follow
[Semantic Versioning](https://semver.org) and a published version never changes.

## Comments

Comments are welcome: questions, problems, missing cases, and how you record games today. See
[CONTRIBUTING.md](CONTRIBUTING.md). The open questions are listed at the end of Part 4 of the
spec.

## Implementations

- **0GG** (not yet public), a fast game based on Game of the Generals: writes and reads GGN for
  every game, and replays records on its analysis board. Its records use the `0GG` extension
  defined in Part 2.

## License

The specification, examples and sheets are dedicated to the public domain under
[CC0 1.0 Universal](LICENSE): use them for anything, with or without credit.
