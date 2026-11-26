# Changes

## v1.0.2 (2026): three bug fixes

File: `TE2000-1.0.2.BAS` / `te2000-1.0.2.tap` (no auto-start: `LOAD ""`, then `RUN`).
This is v1.0.1 plus the three fixes below and two version-note REMs at the
end. The REMs come after `9990 STOP` and are never executed.

1. **Line 860**: the menu key `:` crashed the program (`C Nonsense in BASIC, 890:1`).
   ```
   before: 860 IF s$<CHR$ 48 OR s$>CHR$ 58 THEN GO TO 720
   after:  860 IF s$<CHR$ 48 OR s$>CHR$ 57 THEN GO TO 720
   ```
2. **Lines 8540 and 8590**: colour 8 was accepted and crashed with
   `K Invalid colour, 8600:1`. The upper bound is now `7` (CHR$ 55).
3. **Line 8590**: the paper-colour check tested `a$` (the border key)
   instead of `b$`. A letter for the paper colour crashed with
   `2 Variable not found, 8600:2`.
   ```
   before: 8540 IF a$<CHR$ 48 OR a$>CHR$ 56 THEN GO TO 8500
   after:  8540 IF a$<CHR$ 48 OR a$>CHR$ 55 THEN GO TO 8500

   before: 8590 IF b$<CHR$ 48 OR a$>CHR$ 56 THEN GO TO 8550
   after:  8590 IF b$<CHR$ 48 OR b$>CHR$ 55 THEN GO TO 8550
   ```

Added version notes:
```
9991 REM v1.0.1 (2026): added line 8 GO TO 9830 so RUN works
9992 REM v1.0.2 (2026): fixed lines 860, 8540, 8590 (: key, colour 8, b$ check)
```

## v1.0.1 (2026): Chris Nystrom's fix, so RUN works

File: `TE2000-1.0.1.BAS` / `te2000-1.0.1.tap` (no auto-start: `LOAD ""`, then `RUN`).
This is the published listing plus one line:
```
8 GO TO 9830
```
Lines 1–7 are the REM title box, so line 8 is the first line `RUN` executes.
It jumps to the real start at 9830. Nothing else changes.

## v1.0 (1985): as published

File: `TE2000.BAS` / `te2000.tap`. This is the type-in listing from
*T-S Horizons* No. 13 (April 1985), transcribed unchanged. It must be started
at line 9830: the magazine saves it with `SAVE "TE2000" LINE 9830`, and
`te2000.tap` auto-starts there. If you break out of the program, type
`GO TO 9830` to restart, or `GO TO 800` to return to the menu with your text
kept. Do not type `RUN`, which stops with `2 Variable not found, 9610:1`.
