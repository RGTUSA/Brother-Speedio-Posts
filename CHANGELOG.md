# Changelog

All notable changes to this post set are documented in this file.

## 2026-10-02

### S-machine test post now `speedio RGT 2026 EXP E1015.cps` (repo root), tag **E1015**: Fusion sim clean on O1401 (Green)
Production `speedio RGT 2026.cps` still unchanged. Each test version has the tag in its file name; older ones are in `reference/experimental/`. O1401 G-code from E1015 matches the RGTSPD-style moves (A always 0-359.999, short-way indexing).
- **E1003** Tool break: no second `M05` after `M98 P8000` (M05 before it stays). **A0 before tool change / tool break** (property, default ON, 4th-axis machines only): `G28 G91 Z0` then `G00 A0.` so the machine matches the sim. Machine home X/Y properties (G53, used by the "Home" end position; Center at Door Y also uses Machine home Y). Ported from Autodesk 44242: G87 back-bore Z fix.
- **E1004** File name carries the version.
- **E1005-E1008** Sim of the O8000 break check: Z top, G53 to the tool setter (#701 -26.59 / #702 0), rapid to 1" above, feed to 0.05" above the touch (#704 5.9406 + tool gauge length), back to Z top. Fusion Z/X/Y lined up by matching the machine-definition range maximum to machine Z top (18.8976) / X0 / Y0. Sim-only properties: Tool setter X/Y/Z, Machine Z top, touch gap. (A sim tool change there is not allowed - only one per connection.)
- **E1009-E1014** A-axis direction in the sim (several wrong turns, see E1015).
- **E1013** Turned OFF the 44242 `activateWorkCoordsForNextOperation` sim call again (same breakage as the U500 X0929 test).
- **E1015** The Speedio indexes A the short way (rollover). With A Range Unlimited in the machine definition and sim direction "Shortest" (property, default), the sim matches. Property **"L plate on"** (default ON): index (3+2) angles are folded into 0-359.999 (A360 -> A0, A-90 -> A270, A450 -> A90); 4/5-axis ops are not folded but must stay 0-359.999 or posting stops. Turn it OFF with the L plate off (round parts, wrapping).
- Machine definition settings that go with this post: see the Green machine folder README.
- Not done: 4th-axis safe tool change return path (Z up, Y to rear, then X). Machine test: MDI `G90 G00 A45.` then `A315.` should go -90 through 0; single-block the first tool change / break check / A270->A0 index.

### New test post: `reference/experimental/speedio RGT 2026 EXP.cps` (tag **E1002**)
Built from `speedio RGT 2026.cps`. **Production `speedio RGT 2026.cps` is unchanged** (restored to its April version). Everything below is in the EXP post only, for testing on the S-machines:
- **Machine profiles + pre-post checks** (`rgtMachines`, `rgtCheckMachine()`), new property **Target machine** (Auto / Orange / Black / Red / Yellow / Green). Auto reads the Fusion machine-definition vendor/model/description: full ID (`S700X2-6`) first, then the colour word, then S500X1 / S700X1 (unique models). Can't identify it → post stops and asks.
  - S500X1-4 Orange 10,000 RPM / 14 tools · S700X1-2 Black 16k / 21 (CTS out of service) · S700X2-1 Red, S700X2-3 Yellow 16k / 21 · S700X2-6 Green 16k / 21 + 4th axis
  - **Errors:** any op over the machine's max RPM (names the op and tool), more distinct tools than the magazine holds, a rotary/indexed op on a 3-axis machine, *Use A-axis* on for a non-Green machine. **Warning:** through-spindle coolant on Black.
  - Writes `(TARGET MACHINE …)` in the header; the rev tag is on the program-name line.
- **Simulation fix:** `machineSimulation()` skips the `X[#5021-#5041+…]` Center-at-Door expression instead of erroring (NC unchanged).
- **4th-axis safe tool change** (property, default OFF): `G100 T_ D_ S_ M03` only, then `G00 A_`, `G00 X_` (clear the L plate), `G00 Y_` (forward), `G43 Z_ H_`. Return path (Z up, Y to the rear, then X to setter/home) **not done yet**.
- Untested: post a Green job and a 3-axis job, check the header and the errors, then single-block on the machine.

## 2026-10-01

### Changed
- Post tag `R1001`, a **defaults-only** change on top of the tested R0928. `partAccessX` now defaults to **-11.75** (Steve's door position, in program units: inch programs), so new NC programs no longer start at 0. All other defaults were already matching the known-good Gimbal OP1 / O1522 settings (TCP smoothing M285, link M284, M298 Automatic, washdown Always, part access on M00 + end + level table). No NC output change other than the tag and the default X.

## 2026-09-28

### Changed
- Post tag `R0928`. Part access now levels the table **A0 C0 at Z home before the X/Y move**, at program end and (property `partAccessLevelTable`, now default ON) at M00 - same order as the O8000 break-check macro (G28 Z, G28 A, then XY).
- Verified O1522 (R0925D): program end posts `G49 / G69 / (PART ACCESS POSITION) G53 G00 X-11.75 Y0` (X-11.75 Y0 = Steve's door position). M00 path still to be tested.

## 2026-09-25

### Release
- `brother speedio U500XD1 2026 TWP FINAL.cps` revisions R0925 -> R0925D. Suggested git tag: `v2026.09.25-rewind-fix`.
- Revision tracer line removed; the tag now lives in the Fusion post name ("... TWP FINAL R0925D") and is appended to the program-name line: `(O1521 GIMBAL OP1 R0925D)`. Bump `postRevTag` + `description` together.

### Fixed
- **SM4039.004 on rewind** (retract-and-reconfigure): M280-M287 stay modal after G49 as high-accuracy mode B (NC Prog. 14.2.6.5 NOTE 1); a G01 moving A+C in mode B alarms (14.1.6). Rewind now writes `M289` after G49 and restores the exact prior smoothing (M284 link / section level) after the re-entry `G43.4`; link-smoothing swaps are frozen during the rewind. Stock `rewind.cpi` (44214 and 44222) has the same gap.
- Rewind rotary index posts as `G00 A_ C_` instead of `G01 ... F200` (~57 s at 200 deg/min). Gated by the rewind flag only - normal TCP links still post high-feed G01.
- **Spindle/coolant restart after rewind**: the TCP retract `G100 T__` stops the spindle and nothing restarted it before the re-entry plunge. `S__ M03` + `M08` now output right after the index at Z home.
- Fusion machine simulation "Tool-change instruction missing" / "Connection without a tool": bare `G100 T__` for tool-change TCP entries now signals `machineSimulation({mode:TOOLCHANGE})`. Simulation only.

### Added
- Part access position: new properties `partAccessOnStop`, `partAccessOnProgramEnd`, `partAccessX`, `partAccessY` (G53 machine coords, program units, default 0/0 = old behaviour), `partAccessLevelTable`. Manual NC Stop between ops runs the break-control safe sequence (M09, retract, G49, G69, smoothing off, M05) then `G90 G53 G00 X_ Y_` before `M00`; program end uses the same X/Y instead of `G53 G00 X0 Y0` (and now also gets a G69 before the final A0 C0). M00s written inside an operation are unaffected (`inSection` gate).

### Verified
- O1520 (tracer 2026-07-30D DEFAULTS) alarmed SM4039.004 at line 3614; O1521 (R0925C) ran the rewind clean on the machine 2026-09-25 - index, spindle restart and M284 restore confirmed. Fusion simulation clean with R0925B+.
- Part access (R0925D): **not yet machine-tested.**

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
