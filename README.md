# Brother Speedio Posts

## Active production post
- Use: `brother speedio RGT 2026.cps`

## Required Fusion post properties (legacy BLUM-safe setup)
For jobs that must match legacy RGT BLUM probing behavior:
- `Probing type` = `Blum`
- `Blum macro style` = `Legacy (RGTSPD)`
- `Blum A1 parameter` = `OFF` (default)
- `Tool break control macro` = `8000` (or your control-specific macro number)

## Expected NC behavior with this setup
- BLUM probing uses legacy-style macro arguments (`K`, `I`, `J`, signed `Z` where applicable).
- If tool break control is used, operation end outputs:
  1. `M298 L0`
  2. `M98 P<toolBreakControlMacro>`

## Quick pre-run verification
- Repost and confirm first probe calls are in legacy style (no modern `Q`-style substitution).
- Confirm `M298 L0` is immediately followed by `M98 P...` at relevant operation ends.
- Run machine-side dry run before production cutting.

## Traceability
- Release tag: `v2026.02.27-probing-fix`
- Additional details: `DEV_NOTES.md` and `CHANGELOG.md`
- Operator quick card: `OPERATOR_SETUP_CARD.md`
- Printable checklist: `OPERATOR_CHECKLIST_PRINT.md`
