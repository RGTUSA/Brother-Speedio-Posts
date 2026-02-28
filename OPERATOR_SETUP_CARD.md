# Operator Setup Card (Fusion + Brother)

Use this card before posting/running jobs that require legacy BLUM compatibility.

## 1) Select post
- `brother speedio RGT 2026.cps`

## 2) Fusion post properties
- `Probing type` = `Blum`
- `Blum macro style` = `Legacy (RGTSPD)`
- `Blum A1 parameter` = `OFF`
- `Tool break control macro` = `8000` (or your machine-specific value)

## 3) NC spot-check (first minute)
- First BLUM call should look like legacy style, e.g. `G65 P8700 Z... K... W54.`
- X/Y BLUM calls should use `I` / `J` center values (not modern `Q` substitution)

## 4) Tool-break spot-check
- Where break control is used at operation end, sequence should be:
  1. `M298 L0`
  2. `M98 P8000` (or your configured macro)

## 5) Run safety
- Perform machine dry run/single-block verification before production cutting.
