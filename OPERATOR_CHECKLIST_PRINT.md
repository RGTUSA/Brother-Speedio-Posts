# OPERATOR CHECKLIST (PRINT)

Job: ____________________    Date: __________    Operator: ____________________
Machine: _________________    Program: ________

## Pre-Post Setup
- [ ] Selected post: `brother speedio RGT 2026.cps`
- [ ] `Probing type` = `Blum`
- [ ] `Blum macro style` = `Legacy (RGTSPD)`
- [ ] `Blum A1 parameter` = `OFF`
- [ ] `Tool break control macro` = `8000` (or machine-specific value)

## NC Validation (Before Run)
- [ ] First BLUM call is legacy style: `G65 P8700 Z... K... W54.`
- [ ] X/Y BLUM calls use `I` and `J` (not `Q` substitution)
- [ ] WCS/probing block reviewed for expected work offset
- [ ] If break control is used, operation-end sequence is:
      `M298 L0` then `M98 P8000` (or configured macro)

## Run Safety
- [ ] Dry run completed
- [ ] Single-block verification completed for probe section
- [ ] Tool-length/probe status verified on control
- [ ] Safe start and retract confirmed

## Sign-Off
- [ ] Approved to cut

Notes:
______________________________________________________________________________
______________________________________________________________________________
______________________________________________________________________________
