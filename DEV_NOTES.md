# Developer Notes

Date: 2026-07-30

Release tag: `v2026.07.30-twp-final`

## Post: brother speedio U500XD1 2026 TWP FINAL.cps
Fork of `brother speedio U500XD1 2026.cps` (rev 44214). All changes marked `// TWP FORK:` in code. See CHANGELOG 2026-07-30 for the feature list. Full engineering writeups with manual citations live in `Claude Workspace\Projects\Brother U500XD1\` (Tilted-Plane WCS Probing.md, vault notes, TWP Probing Post - Tech Breakdown.pdf).

## Machine-side settings this post assumes (U500Xd1, D-00)
- Pendant 5-axes-machining (By M code): M284 = corner decel 999/999/999, accel 100/100/100, smooth 999 (link level). M285 = 250/250/250, 100s, 250 (deburr cutting level, Yamazen stock).
- Renishaw Inspection Plus (O87xx/O88xx) loaded; O8744 configured #30=13 (A/C).
- Recommended Fusion setup for deburr/link-heavy TCP ops: 5-Axis TCP smoothing = M285, link move smoothing = M284, op Linking = high feedrate mode ON + allow rapid retract OFF (rapids under TCP stop at every block).

## Validation
- O1601 vs O1611 (probing), O0200 vs O0201 (smoothing/SM4125), coupon parts 2026-07-30. Machine dry-run + first-article checks performed by Steve.

## Notes
- The DRY RUN key disables TCP look-ahead accel/decel — never evaluate motion quality with it on.
- Fusion post-selection gotcha: machine definitions and Downloads copies snapshot the post. The output tracer line is the source of truth for which copy posted a file.

---

# Developer Notes

Date: 2026-02-27

Release tag: `v2026.02.27-probing-fix`

## Post updates included
- Added BLUM A1 output toggle property (`blumUseA1`) and set default to OFF.
- Added tool break macro property (`toolBreakControlMacro`, default `8000`).
- Added queued end-of-operation tool-break call so `M98 P<toolBreakControlMacro>` is output immediately after `M298 L0` when break control is used.
- Added BLUM macro style switch (`blumMacroStyle`) to support legacy RGT probing argument format.

## Validation against legacy output
Compared reposted NC output:
- New file: `O1401.NC`
- Legacy file: `O1402.NC`

Result: all `G65 P8700` calls found in both files match in argument style and values for this repost, including legacy-format tokens (`K`, `I`, `J`) and signed `Z` usage where expected.

Examples that match:
- `G65 P8700 Z-0.1575 K2.9385 W54.`
- `G65 P8700 S6.03 X1 I2.3125 Z-0.2787 R0.1575 W54.`
- `G65 P8700 S6.03 Y1 J1.5475 Z-0.2787 R0.1575 W54.`

## Tool break sequence validation
Verified in `O1401.NC` that each relevant operation end emits:
1. `M298 L0`
2. `M98 P8000`

Observed at multiple locations (e.g. around N6885/N6890 and final lines N31085/N31090).

## Notes
- This validation confirms repost settings now align with legacy BLUM behavior for the tested operations.
- Final machine-side dry-run is still recommended before production cutting.
