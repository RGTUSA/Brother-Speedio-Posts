# Reference posts: not for production

Kept for diffs, history and borrowing code. **Production posts live in the repo root only.** Moved here 2026-10-01/02 to stop mix-ups.

## stock/ (Autodesk originals, unedited)
| File | Autodesk rev | Use |
|---|---|---|
| `brother speedio.cps` | 44214 (2026-02-17) | Base of `speedio RGT 2026.cps` and the U500 fork; diff against this to see every RGT change |
| `brother speedio 4-30-2026.cps` | 44222 | Newer stock, for comparing Autodesk fixes |
| `brother speedio 442.42.cps` | 44242 (2026-09-07) | Newest stock; base of the experimental port |

## old/ (superseded or third-party: **reference value, don't delete**)
| File | What it is | Why keep it |
|---|---|---|
| `RGTSPD.cps` | Custom post by **Wisconsin CAM Solutions (Alex Rosner), 2022**, S300/500/700X1 with A axis | What the S-machines ran up to 2026. `speedio RGT 2026` emulates its probing ("Legacy (RGTSPD)" Blum style). Reference output: O1401 (A270 on the G100 line, `G28 Z` → `G00 A_ X_ Y_` index, `G53 X-27 Y0` + `G00 A0.` at end, legacy Blum `P8700 S_ X1 I_ E_`). |
| `brother speedio Wildgoose.cps` | Autodesk 44093 (2023) with **extensive edits by another user** | Source of pulled-in features. **Only post with G54.2 rotary fixture offset WCS** (`G54–G59 + G54.2 P1–P8`, cancel `G54.2 P0`). Needs the Brother G54.2 option (not licensed on Green); see the 4th-axis note and the O9811 macro alternative. |
| `brother speedio U500XD1 2026.cps` | Original U500 parent (44214) | Superseded by `… TWP FINAL.cps` |
| `brother speedio U500XD1 2026 TWP FINAL 2.cps` | Stray R0925 copy of the U500 fork | Superseded, safe to delete |
| `brother speedio inspection.cps` | Autodesk inspection variant (44214) | Reference for inspection/probe-results output |
| `FULL.NC` | Sample output from March 2026 | Reference NC |

## experimental/
| File | What it is |
|---|---|
| `brother speedio U500XD1 TWP 44242 EXP.cps` | U500 fork ported onto 44242 (X0929C). Not for production until side-by-side NC compares pass. |
