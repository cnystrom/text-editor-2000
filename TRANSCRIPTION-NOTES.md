# Transcription notes

`TE2000.BAS` is a hand transcription of the *Text Editor 2000* type-in listing
printed in *T-S Horizons* No. 13 (April 1985). The article starts on p.18; the
listing is printed on these pages:

| Magazine page | Lines            |
|---------------|------------------|
| p.19          | 1 – 950          |
| p.20          | 955 – 5130       |
| p.22          | 5140 – 9797      |
| p.23          | 9798 – 9990      |

Total: **553 program lines.** The scans are on archive.org
(<https://archive.org/details/TSHorizons>). This repository does not include them.

## Method

1. Each listing column was cut from the 150 dpi page scans and enlarged, then
   read by eye, line by line.
2. The listing was printed by a 32-column printer, so every printed row holds
   32 characters. Where a count mattered (runs of dashes, runs of spaces inside
   strings, the REM boxes and banners), the count was taken from the wrap
   position against a 32-column ruler laid over the scan.
3. Two sources were used only as cross-checks, never as the text:
   * archive.org's OCR of the issue (`T-S Horizons n13_djvu.txt`), and
   * the later "Text Editor 2000 2.2" tape (named `Textype`; it is Mannie
     Quintero's 1985 meld of TE2000 v2.2 with Ron Ruegg's SETYPE). The tape
     was detokenized and compared line by line, ignoring whitespace. Each
     difference was then checked against the scan, and **the magazine wins
     every time** (see the list below).
4. The `.BAS` file uses zmakebas syntax. UDGs are written as `\a` … `\u`
   (CHR$ 144 … 164).

## Uncertain or inferred readings

| # | Page | Line(s) | Note |
|---|------|---------|------|
| 1 | p.19–p.23 | 38 section-banner REMs (10, 120, 165, 215, 270, 315, 345, 375, 710, 895, 995, 1490, 1990, 2990, 3190, 3490, 3990, 4490, 4990, 5490, 5990, 6490, 6990, 7320, 7495, 7590, 7990, 8490, 8990, 9090, 9190, 9295, 9390, 9490, 9590, 9690, 9820, 9930) | The banners print as solid bars of UDG D (horizontal bar) followed by a label. The bars run together on the scan and cannot be counted bar by bar. The count used is the one that fills out the first 32-column printed row: 23 − 1 − len(label) bars. |
| 2 | p.19 | 1–6 | The box drawn from UDGs D–I (corners `\f \g \h \i`, sides `\e`, top and bottom `\d`). The border lengths and the inner spacing were read from column positions on the scan. |
| 3 | p.20 | 3130 | A stray mark that looks like a backslash appears before `t$` (`LET \t$(...`). It is taken to be print noise and transcribed `LET t$(d1 TO d1+LEN a$-1)=a$`, the same as line 950. |
| 4 | several | indented lines (55 lines, e.g. 7045 has 3 spaces) | The leading indentation inside FOR loops was read by eye. It is cosmetic: zmakebas drops it when tokenizing. |
| 5 | p.22 | 6000 | Printed `INPUT AT  21,0` with two spaces. Kept as printed, but zmakebas removes it in the `.tap`, so it is cosmetic. |
| 6 | p.19/p.20/p.22 | 920, 1000, 2000, 3080, 3200, 4500, 6000, 6020, 8000, 9000, 9100, 9110 | Dash and space runs inside prompt strings were counted from the wrap positions: 32 dashes, then 11/2/2/9/2/2/4/5/–/10 spaces before the label (9100 is 32 dashes, 9110 is 32 spaces). The dashes fill exactly one screen row, so the label lands on the next row. |
| 7 | p.22 | 9787 | The REM labels UDG F "Top Right Corner", but the DATA on 9788 draws a top-left corner (┌). Transcribed as printed. |
| 8 | p.19 | 5 | The REM says "15,473 Bytes". The tokenized program built here is about 14.4 KB. The REM is kept as printed. |

## Printed text that looks wrong but is kept as printed

These match the scan. They are recorded here so nobody "fixes" them by mistake.

* **1040** `"SAVEING AS: "`: spelling as printed.
* **8590** `IF b$<CHR$ 48 OR a$>CHR$ 56 THEN GO TO 8550`: the second test reads
  `a$`, not `b$` (the later 2.2 tape has `b$`). See the bug list in README.
* **3200** `Name of text:` has a lowercase "t". 1000 and 2000 use "Text".

## Differences from the later 2.2 / Textype tape

The magazine reading is kept in every case.

* 1–7: the magazine has the UDG title box. The tape has two REMs crediting
  the Quintero meld.
* 50, 3040, 4110, 7005, 7065, 7110, 7170, 7220, 8010, 8560: the magazine has
  `FLASH 1` on the cursor or "?" prompt.
* 7410, 7450: the magazine has `FLASH 1`, and `" STOP THE TAPE"` has a
  leading space.
* 920, 2000, 3080, 3200, 4500: the dash and space counts differ.
* 1080 `(Y/N)` (the tape drops the "(").
* 1130 `" VERIFYING: "`.
* 1520 `BEEP .1,7` and 1530 `GO TO 800` (the tape has only `1520 GO TO 800`).
* 7505 is on the tape only (`OUT 127,…` printer codes from the meld). It is
  not in the magazine.
* 7620 `>=` (the tape has `=`).
* 8510–8550 are in the magazine only (the BORDER colour prompt).
* 8590 `a$>CHR$ 56` (the tape has `b$`).
* 9400 `FOR i=1 TO` (the tape has `FOR i=i TO`).
* 9690–9818 and 9860–9870 (the UDG definitions and their loader) are in the
  magazine only.
* 9850, 9950, 9970: "Text Editor 2000" / "TE2000" (the tape has "Textype").

## Building the tape image

```
zmakebas -a 9830 -n TE2000 -o te2000.tap TE2000.BAS
```

zmakebas (https://github.com/z00m128/zmakebas) removes extra spaces outside
strings and REMs. Indentation and the double space in 6000 are therefore in
the `.BAS` file but not in the tokenized `.tap`. Nothing else changes.
