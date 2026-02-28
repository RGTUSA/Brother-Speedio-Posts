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
