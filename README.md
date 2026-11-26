# Text Editor 2000

*Text Editor 2000* (TE2000) is a menu-driven word processor written by
Chris Nystrom in plain Sinclair BASIC for the Timex/Sinclair 2068. It also runs
on a ZX Spectrum because it uses no 2068-only commands.

It was published as a type-in in **T-S Horizons No. 13 (April 1985)**. It was
the cover story, the article starts on **p.18**, and the listing runs across
pp.19–23. Scans of the issue are on the Internet Archive:
<https://archive.org/details/TSHorizons>.

This repository holds a fresh transcription of that printed listing. It does
not include the magazine scans or the article text.

| File | What it is |
|------|------------|
| `TE2000.BAS` | The listing exactly as printed in T-S Horizons No. 13, in zmakebas text format (UDGs written as `\a`…`\u`). |
| `te2000.tap` | Tape image built from `TE2000.BAS`. It auto-starts at line 9830. |
| `TE2000-1.0.1.BAS` | v1.0.1: the published listing plus one added line, `8 GO TO 9830`. |
| `te2000-1.0.1.tap` | Tape image built from `TE2000-1.0.1.BAS`, with no auto-start. `LOAD ""` and then `RUN`. |
| `TRANSCRIPTION-NOTES.md` | How the listing was transcribed, the readings that were uncertain, and how it differs from the later 2.2 tape. |

## Features

The menu offers Input, Save, Load, Insert, Delete, Justify, Search and Replace,
Text Output (screen or printer, with double spacing and double width),
Clear, Quick Note, Center, Type Over and Change Colors. There are also two
hidden keys: `W` sets the column width and `Z` copies the screen to the
printer. It defines 21 user-defined graphics: a box, a spot and a star,
horizontal and vertical bars, four corners, three arrows, sub- and
superscript 2–4, and ½ ⅓ ¼.
Text lives in one string array, `t$`, and is saved to tape as `DATA t$()`.

## Loading and running

In an emulator (Fuse, ZEsarUX, etc.) choose a 48K Spectrum or a TC/TS2068 and
open `te2000.tap` (v1.0, which auto-starts) or `te2000-1.0.1.tap` (v1.0.1;
type `RUN` after it loads). On real hardware or with tape traps off, type:

```
LOAD ""
```

## How to start it

The program does **not** start at line 1. In the magazine it is saved with

```
9950 SAVE "TE2000" LINE 9830
```

and the line before it is `9820 REM ... START HERE`. Lines 9830–9920 set the
colours, print the title, define the UDGs (`RESTORE 9778: GO SUB 9700`), set
up the text array (`DIM t$(1)`) and width, and then jump to the menu.

* **Loading `te2000.tap` starts it automatically at line 9830.**
* If you break out of the program, **do not use `RUN`** in v1.0 (v1.0.1
  fixes this; see Versions below). `RUN` starts at the top. Lines 1–7 are REMs, and line 20 goes straight into `GO SUB 9600`,
  which stops with `2 Variable not found, 9610:1` because nothing has been set
  up yet.
* To get back to the menu **with your text intact**, type `GO TO 800`. The
  article says on p.18: *"If this should happen to you do not RUN the
  program. Type GOTO 800"*.
* To restart from scratch (this clears the text), type `GO TO 9830`.
* To save a fresh copy of the program, press `S` at the menu. The article
  says on p.20: *"To copy this program, type 'S' at the 'Selection?'
  prompt."* That runs lines 9940–9990: `CLEAR`, then
  `SAVE "TE2000" LINE 9830`, then `VERIFY`.

## Versions

### v1.0: as published (`TE2000.BAS`, `te2000.tap`)

This is the listing from T-S Horizons No. 13, unchanged. It must be started
at line 9830, as described in "How to start it" above. `te2000.tap` does this
for you.

### v1.0.1: Chris Nystrom's own fix (`TE2000-1.0.1.BAS`, `te2000-1.0.1.tap`)

This is Chris Nystrom's own fix: a GO TO at the top so RUN works.

In Chris's words, the problem was that "if you started at the top it did not
work". The program "was saved to start in the middle of the program", and
"the bug fix was to put a go to at the top of the program". v1.0.1 is the
published listing with exactly one line added:

```
8 GO TO 9830
```

Nothing else changes. Lines 1–7 are the REM title box, so line 8 is the first
line `RUN` actually executes, and it sends the program to the real start at
9830 (the `LINE 9830` in the magazine's `SAVE`). A line number below 1 isn't
possible without renumbering the title box, which this fix deliberately does
not do.

```diff
  7 REM
+ 8 GO TO 9830
  10 REM ... EDITOR
```

With v1.0.1, `RUN` starts the program cleanly. `RUN` (like `GO TO 9830`)
still clears any text in memory, so after a break use `GO TO 800` to get back
to the menu with your text.

Building:

```
zmakebas -a 9830 -n TE2000 -o te2000.tap       TE2000.BAS        # v1.0
zmakebas         -n TE2000 -o te2000-1.0.1.tap TE2000-1.0.1.BAS  # v1.0.1
```

## Known bugs in the published listing (not fixed)

These were found while testing this transcription in Fuse. They are present
in both v1.0 and v1.0.1, which keep the published code unchanged apart from
line 8. Suggested one-character fixes are listed but not applied.

| Line(s) | What happens | Suggested fix |
|---------|--------------|---------------|
| 860 | `s$>CHR$ 58` lets `:` (CHR$ 58) through to `890 GO TO VAL s$*1000`. Pressing `:` at the menu stops with `C Nonsense in BASIC, 890:1`. | `CHR$ 57` |
| 8540, 8590 | The colour range check accepts `8` (CHR$ 56). Choosing border 8 stops with `K Invalid colour, 8600:1`. | `CHR$ 55` |
| 8590 | The upper-bound test checks `a$` (the border key) instead of `b$` (the paper key). A letter for the paper colour gets through and stops with `2 Variable not found, 8600:2`. The later 2.2 tape has `b$`. | `b$>CHR$ …` |
| 410 | `GO TO 630` points at a line that does not exist, so BASIC falls through to 710/720 (the menu) from inside the `GO SUB 20` editor routine. Each time, a GO SUB return address is left on the stack. This was found by reading only and is harmless in normal use. | none needed |

After any of these errors, `GO TO 800` returns to the menu with the text kept.

## Notes

* The article says on p.20: *"I have not used any of the TS 2068 specific
  commands, so owners of emulators and ROMswitches should not have to switch
  back and forth."* The listing uses only Spectrum BASIC. Text Output
  assumes a TS 2040 / ZX printer (`LPRINT`, `COPY`). `INK 9` and
  `POKE 23609,5` (key click) are standard Spectrum features.
* The article also suggests optional key-repeat POKEs: `9900 POKE 23561,10`
  and `9910 POKE 23562,3`. They are not in the listing and are not added here.
* *T-S Horizons* No. 14 reviewed the program on p.22 in Bill Ferrebee's "2068
  Word-Processor Software Evaluation (Part III)". The review sold it for $6.00
  from People's Software Supply and pointed readers to the free listing in
  Issue #13. No errata or corrections for TE2000 were found in issues 14–21.

## Related versions (not included)

* **Text Editor 2000 v2.2** and **TEXTYPE**. TEXTYPE is a 1985 public-domain
  "meld" by M. (Mannie) Quintero that combines TE2000 v2.2 with Ron Ruegg's
  SETYPE. It circulated on CATS Library Tape 6. These are not purely Chris
  Nystrom's work, so they are not included. See
  <https://www.timexsinclair.com/computer_media/textype/index.html> and
  <https://www.timexsinclair.com/indiv/christopher-nystrom/index.html>.

## License

Released into the public domain by Chris Nystrom in 2026 (see LICENSE).
