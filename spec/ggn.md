# RFC GGN-1: Generals Game Notation (GGN)

| | |
| --- | --- |
| Request for Comments | GGN-1 |
| Category | Proposed standard |
| Status | Draft |
| Version | 1.0.0-draft.4 |
| Author | juuerlrsr |
| Date | November 2026 |
| License | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public domain) |

## 1. Abstract

GGN is a plain-text format for recording games of **Game of the Generals** (Salpakan). A record is a list of tags followed by the moves.

GGN is based on chess's [Portable Game Notation (PGN)][pgn]. From PGN come the tags and their syntax, the Seven Tag Roster, numbered moves, comments, results, and the rule that readers are lenient while writers use one exact layout. Sections taken from PGN say so. Squares are named as in chess's [algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_%28chess%29) (`e4`).

What is new is how GGN keeps ranks hidden: **moves never contain ranks**. One record can hold everything, or only what one player or a spectator saw, and still replay exactly.

GGN describes the classic game; other rule sets add to it as **extensions** (section 12). It works for any board, any colors for the two sides, and any design of pieces.

[pgn]: http://www.saremba.de/chessgml/standards/pgn/pgn-complete.htm

## 2. Status of this memo

This is a **draft**: a proposed standard, published for review. It may change in any way from one draft to the next. The first final version will be 1.0.0, and from then on a published version never changes. Comments are welcome; see the repository's CONTRIBUTING file.

Sections 4 to 15 are **normative**. The abstract, section 3, the examples and section 16 are **informative**: they add no requirements.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 ([RFC 2119](https://www.rfc-editor.org/rfc/rfc2119), [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174)) when, and only when, they appear in all capitals, as shown here.

## 3. At a glance

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
5. e6>e7 {beats the Colonel} f6>f5 6. e7>e8 h5>h4 7. e8>d8 g4>g3 8. d8>c8 f5>f4
9. c8>b8 h4<h3 10. b8>a8# 1-0
```

- The tags name the players and the result, and say **where every piece started**.
- Each move names two squares, and one symbol says how it ended: `>` moved or won, `<` the attacker lost, `x` both pieces were removed.
- At `9. … h4<h3`, North's Lieutenant Colonel attacks South's Two-Star General and loses. The move names no rank; the setups do, where the record shows them.

(The setups are shortened here; a real classic setup lists all 21 pieces a side.)

## 4. Files

- A file **MUST** be encoded in UTF-8 and **SHOULD** use the extension `.ggn`. (PGN uses Latin-1; UTF-8 lets any name be written.)
- A file **MAY** hold several records. As in PGN, they are separated by a blank line, and each starts with its tags.
- Readers **MUST** accept any line ending (LF, CRLF or CR).

## 5. Pieces

| Code | Piece | Classic count |
| --- | --- | --- |
| `5G` | Five-Star General | 1 |
| `4G` | Four-Star General | 1 |
| `3G` | Three-Star General | 1 |
| `2G` | Two-Star General | 1 |
| `1G` | One-Star General | 1 |
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

- Records **MUST** use these codes. A rank the record doesn't show **MUST** be written `?`.
- Readers **SHOULD** also accept `G5` … `G1`, the generals' codes before draft 4.
- A record holds only squares, sides and rank codes. How the game looked (the board's material, the colors, the pieces' design, labels on screen) is not part of it, and standard tags **MUST NOT** record it.

## 6. Board and sides

- Squares are named as in chess's algebraic notation: a file letter from `a`, then a rank number from `1`. The classic board runs `a1` to `i8`.
- **South** plays from the low ranks, **North** from the high ranks. Sides are named by position, not color: sets differ in color, and either side may move first.
- Files run from **South's left**.

## 7. Tag syntax

*From PGN (section 8.1).* A record starts with its tags, one per line:

```
[Name "value"]
```

- A name is letters, digits and underscores, starting with a letter. Names are case-sensitive.
- A value is a string in double quotes. Inside it, a quote is written `\"` and a backslash `\\`; there are no other escapes. A value **MUST NOT** contain a line break.
- A tag name appears at most once in a record.
- Readers **MUST** keep tags they don't know, and **MAY** ignore them.

For example, `[South "Ana \"Ace\" Cruz"]` names the player `Ana "Ace" Cruz`.

## 8. Tags

The first seven are PGN's **Seven Tag Roster**, with `South` and `North` for `White` and `Black`. Tags marked "yes" are **REQUIRED**; the rest are **OPTIONAL**. An unknown value is written `?`.

| Tag | Required | Value |
| --- | --- | --- |
| `Event` | yes | What the game was, e.g. `Club night` |
| `Site` | yes | Where it was played |
| `Date` | yes | Start date, `YYYY.MM.DD` (UTC when known); unknown parts as `??`, as in PGN |
| `Round` | yes | Round in an event, or `-` |
| `South`, `North` | yes | Player names |
| `Result` | yes | `1-0` South won, `0-1` North won, `1/2-1/2` draw, `*` no result, as in PGN |
| `GGN` | yes | The version of this standard the record follows, e.g. `1.0.0-draft.4` |
| `Variant` | no | The rule set, if not classic (section 12). **No `Variant` tag means classic** |
| `Board` | no | `columns x rows`; default `9x8` |
| `FirstMover` | yes | `South` or `North` |
| `TimeControl` | no | As in PGN: base seconds `+` increment seconds, e.g. `600+0`; `-` for no clock |
| `UTCTime` | no | Start time, `HH:MM:SS` UTC |
| `Termination` | no | How the game ended (below) |
| `Visibility` | yes | Which ranks the record shows (section 9) |
| `SouthSetup`, `NorthSetup` | yes | Each piece where it stood when play began, as `square:rank`, separated by spaces, e.g. `a1:FLAG e2:5G` |
| `Annotator` | no | Who added comments |

**Termination.** Writers **SHOULD** use one of these values. Extensions **MAY** add more, and readers **MUST** accept values they don't know.

| Value | The game ended because |
| --- | --- |
| `flag_captured` | a Flag was captured |
| `flag_reached` | a Flag reached the far row and won under the rules in use |
| `resign` | a player resigned |
| `timeout` | a player ran out of time |
| `agreed` | the players agreed a draw |
| `repetition` | a draw by repetition, as the rules in use define it |
| `no_moves` | the player to move had no legal move, and the rules end the game there |
| `abandoned` | a player left the game |
| `adjudicated` | an arbiter decided the result |
| `aborted` | the game was called off without a result (`*`) |

## 9. Visibility

Ranks are secret during play, so a record states whose knowledge it holds:

| Value | Shows |
| --- | --- |
| `full` | Every rank: a record written after the game, with both armies revealed |
| `south`, `north` | That player's ranks; the opponent's are `?` |
| `public` | No ranks: every setup rank is `?` (a spectator's record) |

The moves are the same whatever the visibility, so **every record replays exactly**. As South's own record, the example in section 3 has:

```
[Visibility "south"]
[SouthSetup "a1:FLAG b1:PVT c1:PVT d1:SPY e2:5G f2:4G g2:3G h3:2G i3:1G"]
[NorthSetup "a8:? e7:? f7:? g6:? h6:?"]
```

## 10. Movetext

*Turn numbers, comments and results as in PGN (section 8.2).* After the tags come a blank line and the moves. Plies **MUST** alternate strictly, starting with `FirstMover`. Each turn is numbered and holds the first mover's ply, then the other's: `1. e2>e3 g6>g5 2. …`.

| Token | Meaning |
| --- | --- |
| `e2>e3` | Moved to an empty square, or challenged and won (the defender is removed). Which one follows from whether `e3` was occupied |
| `e5<e6` | Challenged and lost: the attacker is removed |
| `e5xe6` | Equal ranks: both pieces are removed |
| `=RANK`, `=` | Suffix after the target square: the piece's rank changed (a promotion, under rules that have one), e.g. `e5>e6=MAJ`; `=` alone when the record doesn't show the new rank |
| `#` | Suffix, last: this move ended the game on the board |
| `--` | Pass: the player had no legal move, under rules that allow passing |
| `N.` | Turn number; readers **SHOULD** ignore it when replaying |
| `{…}` | Comment, after the ply it describes |
| `; …` | Comment to the end of the line |
| `1-0` `0-1` `1/2-1/2` `*` | Result, at the end; **MUST** match the `Result` tag |

- Moves **MUST NOT** contain ranks, except after `=`, and only when `Visibility` shows that side. `=` **MUST NOT** follow `<` or `x`: the piece fought as it was.
- As in PGN, braces don't nest: a comment in braces **MUST NOT** contain `}`.
- **Annotations** go inside comments as `[%name value]`. GGN defines `[%clk m:ss]`, the mover's clock after the move (as on chess servers), and `[%think s]`, the seconds the move took. Readers **MUST** ignore comments and annotations they don't understand.

```
1. c2>c3 {[%clk 2:54] [%think 5.9]} d5>d4 {[%clk 2:52] [%think 7.8]}
2. c3>c4 {a bold Spy} d4>d3 ; North heads for the Flag
```

## 11. Replaying

From the two setups, apply each ply in order to the side whose turn it is:

- `>`: remove any enemy piece on the target square, then move the piece there.
- `<`: remove the attacker.
- `x`: remove the attacker and the defender.
- `=RANK`: the moved piece's rank becomes `RANK` (`?` for `=` alone).

No ranks or rules are needed. A ply from a square the mover doesn't occupy, or a `<` or `x` onto an empty square, makes the record invalid; readers **MUST** reject it.

## 12. Extensions

A rule set other than classic is an **extension**. Its records carry a `Variant` tag with its name, and a `Board` tag if the board isn't 9 × 8.

- An extension **MAY** add tags and `Termination` values. It **MUST NOT** change the meaning of anything defined here.
- Setups and moves are always written as in sections 8 and 10, so a reader that knows only this standard can still replay any record.
- An extension **SHOULD** be published in its own document. For example, the game 0GG publishes its extension, `Variant "0GG"`.

## 13. Writing a record

*From PGN's export format (section 3.2).* Readers **SHOULD** be lenient about spaces and line breaks. Writers **MUST** use one exact layout, so the same game always gives the same text:

1. One tag per line, with one space between the name and the value: the tags of section 8 in that order, then extension tags.
2. One blank line, then the moves.
3. One space between tokens, with a turn number, its move and the move's comment kept together. Each line holds as many tokens as fit in 80 characters; a new line starts only when the next one wouldn't fit.
4. The result as the last token, then a line break.
5. One blank line between records in a file.

## 14. Hidden information

Game of the Generals is a game of hidden ranks, and a record can give them away.

- During a game, a writer **MUST NOT** give anyone a record that shows ranks they may not see: a player gets at most their own record (`south` or `north`), and a spectator a `public` one.
- Ranks a side chose in secret, including those in extension tags, appear only when `Visibility` shows that side.
- After the game the armies are revealed, so a `full` record may be shared.

## 15. Versioning

The `GGN` tag holds the version of this standard, following [Semantic Versioning](https://semver.org). Drafts carry a pre-release tag (`1.0.0-draft.4`); the final version will be `1.0.0`. A reader **MUST** refuse a record whose major version is newer than its own, ignoring any pre-release tag in that comparison.

### Changelog

- **1.0.0-draft.4**: The generals are `5G` to `1G` (readers still accept `G5` to `G1`). Following PGN: tag syntax and escaping, `Termination` values, comment rules, and one exact layout for writers. Rules on hidden information. The `=` suffix is now part of the core. Slimmer: the 0GG extension is published by 0GG, and recording games on a physical board is a separate guide.
- **1.0.0-draft.3**: Wording only: US English; "pieces" for what was sometimes called tiles.
- **1.0.0-draft.2**: 0GG's All random games record how many pieces each player is shown.
- **1.0.0-draft.1**: First draft.

## 16. Open questions

- Move annotations such as `!` and `?`, or PGN's numeric `$1` codes (`?` already means an unknown rank in setups).
- Alternative lines in parentheses, as in PGN, for analysis.
- How to record penalties and corrections decided by an arbiter.
