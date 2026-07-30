# Changelog

All notable changes to this post set are documented in this file.

## 2026-07-30

### Release
- New post: `brother speedio U500XD1 2026 TWP FINAL.cps` — fork of `brother speedio U500XD1 2026.cps` (engine rev 44214 base). Parent post unchanged.
- Suggested git tag: `v2026.07.30-twp-final`
- Every posted file now begins with a revision tracer comment `(POST REV: U500XD1 TWP FINAL ...)` identifying the generating post copy.

### Added
- Tilted-work-plane (G68.2) WCS probing: probe cycles with WCS override under active G68.2 post geometry-only (errors bank in #151/#152/#153); section end emits `G65 P8744` (FCS-to-WCS conversion, in-frame) then retract/G49/G69 and `G65 P8732 S_ W1. Z1.` (offset write, out-of-frame). Required because the D-00 blocks work-offset writes while feature coordinate manufacturing mode is engaged (SM4107) and Renishaw's in-cycle S needs full XYZ error data under RTF (SM9123).
- `cancelWorkPlane()` now emits retract + G49 before any G69 when length compensation is active (D-00 alarms SM4106 otherwise). Fixes the between-probe-ops transition ordering bug in the parent post.
- 5-axis TCP link smoothing now works with section smoothing = Off: links swap to M284, M299 restored for cutting.
- M285 added to the 5-Axis TCP smoothing dropdown (deburr cutting level).
- Pre-TCP smoothing cancel (M298 L0 + M299) widened to ALL TCP entries including 3+2 optimized-for-machine sections (was multiaxis-only; G43.4 with M298 modal alarms SM4125).
- All edit sites carry `// TWP FORK:` code comments explaining what and why.

### Verified
- TWP probing machine-verified 2026-07-29: tilted fixture face + bore probed under G68.2, G54 updated (deltas matched #140/#141/#142), hole reamed. Reference outputs: O1601 (parent post, alarms) vs O1611 (fork, works).
- Smoothing system machine-verified 2026-07-30: O0159/O0201 posted output confirmed (M284/M285 brackets around all links, M299 guards before every G43.4); coupon parts cut smoothing-on vs off — equal edge quality, steadier motion. Pendant M284 = corner 999/999/999, accel 100, smooth 999; M285 = stock 250s.

## 2026-02-27

### Release
- Git tag: `v2026.02.27-probing-fix`

### Added
- BLUM A1 output toggle property (`blumUseA1`) with default set to OFF.
- Tool break macro property (`toolBreakControlMacro`) with default `8000`.
- BLUM macro style switch (`blumMacroStyle`) to support `modern` and `legacyRGT` probing argument formats.
- Developer documentation in `DEV_NOTES.md` capturing validation and safety notes.

### Changed
- BLUM probing macro output can now follow legacy RGT-style argument mapping where required for controller compatibility.
- End-of-operation behavior now emits `M98 P<toolBreakControlMacro>` immediately after `M298 L0` when break control was called.

### Verified
- Reposted output `O1401.NC` matches legacy probing argument style against `O1402.NC` for detected `G65 P8700` calls.
- `M298 L0` followed by `M98 P8000` confirmed at operation ends where break control applies.
