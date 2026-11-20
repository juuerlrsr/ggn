# GGN: Generals Game Notation

**RFC GGN-1 · Draft · Version 1.0.0-draft.4 · Author: juuerlrsr · License: [CC0 1.0](LICENSE)**

GGN is a plain-text format for recording games of **Game of the Generals** (Salpakan). It is
based on chess's [Portable Game Notation (PGN)](http://www.saremba.de/chessgml/standards/pgn/pgn-complete.htm),
and names squares as in chess's [algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_%28chess%29)
(`e4`). A record is a list of tags followed by the moves:

```
[Event "Club night"]
[Site "Manila"]
[Date "1975.06.12"]
[Round "3"]
[South "A. Cruz"]
[North "B. Santos"]
[Result "1-0"]
[GGN "1.0.0-draft.4"]
[FirstMover "South"]
[Termination "flag_captured"]
[Visibility "full"]
[SouthSetup "a1:FLAG b1:PVT c1:PVT d1:SPY e2:5G f2:4G g2:3G h3:2G i3:1G"]
[NorthSetup "a8:FLAG e7:COL f7:SPY g6:PVT h6:LTC"]

1. e2>e3 g6>g5 2. e3>e4 h6>h5 3. e4>e5 g5>g4 4. e5>e6 f7>f6
...
```

Moves name two squares and how the move ended (`>` moved or won, `<` the attacker lost, `x` both
removed). They never contain ranks, so hidden pieces stay hidden and any record still replays
exactly. GGN works for any board, side colors and piece design.

## Contents

| Path | What it is |
| --- | --- |
| [`spec/ggn.md`](spec/ggn.md) | The specification, RFC GGN-1 |
| [`guides/over-the-board.md`](guides/over-the-board.md) | How to record a game played on a physical board, with an [arbiter scoresheet](guides/print/ggn-arbiter-sheet.pdf) and a [player record](guides/print/ggn-player-sheet.pdf) to print (half-letter, close to A5) |
| [`examples/`](examples/) | Example records: a classic game, and the same game as one player saw it |

## Status

This is a **draft**. It may change in any way between drafts (`1.0.0-draft.4`, `-draft.5`, …)
until the first final version, `1.0.0`. After that, versions follow
[Semantic Versioning](https://semver.org) and a published version never changes.

## Comments

Comments are welcome: questions, problems, missing cases, and how you record games today. See
[CONTRIBUTING.md](CONTRIBUTING.md). The open questions are listed at the end of the spec.

## Implementations

- **0GG** (not yet public), a fast game based on Game of the Generals: writes and reads GGN for
  every game, and replays records on its analysis board. Its records use its own extension,
  `Variant "0GG"`, published by 0GG.

## License

The specification, examples and sheets are dedicated to the public domain under
[CC0 1.0 Universal](LICENSE): use them for anything, with or without credit.
