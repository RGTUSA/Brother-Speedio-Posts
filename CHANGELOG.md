# Changelog

All notable changes to this post set are documented in this file.

## 2026-02-27

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
