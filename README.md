# Component Field Exploration Fixtures

This repository is a fixture lab for reverse-engineering Reality Composer Pro (RCP) Inspector fields.

Its purpose is to make field behavior **observable and repeatable** by storing small, controlled `.usda` snapshots for each component/field change, then diffing those snapshots to identify:

- the actual serialized field key/path
- the concrete data type (bool, token, float, vector, asset reference, etc.)
- how RCP rewrites neighboring data when a single Inspector value changes

## Why this exists

When building Deconstructed's Inspector, guessing schema details is not reliable. RCP can normalize values, reorder sections, emit defaults, and mutate related fields. This fixture set gives us evidence-based mappings from UI controls to USDA structure.

In short: this repo is the ground truth used by the article and by implementation work.

## What is in here

- `Package.realitycomposerpro/`:
  - RCP document metadata (`ProjectData/main.json`, workspace state)
- `Sources/RCPComponentDiffFixtures/RCPComponentDiffFixtures.rkassets/`:
  - Fixture USDA files grouped by component and field variation
  - Typical pattern:
    - `BASE.usda` = baseline for a component
    - variant files (`Intensity.usda`, `MaskAll.usda`, etc.) = one controlled change
    - sometimes `ALL.usda` = combined state reference

Examples:

- `Directional Light/BASE.usda` vs `Directional Light/Intensity.usda`
- `Collision/Filter/Mask/Default.usda` vs `Collision/Filter/Mask/All.usda`
- `Physics Body/Mode/Static.usda`, `Dynamic.usda`, `Kinematic.usda`

## Fixture method

1. Start from a known baseline (`BASE.usda`).
2. Change one Inspector field in RCP.
3. Save and export the resulting USDA snapshot.
4. Diff baseline vs variant.
5. Record discovered mapping (field path, type, value encoding, side effects).

## How this helps Deconstructed

- Drives Inspector field type modeling from real data.
- Prevents regressions when parser/editor logic changes.
- Exposes RCP write-time transformations we need to preserve for compatibility.
- Provides reproducible examples for docs and article references.

## Notes

- Files intentionally include edge and inconsistent naming cases captured from real tool output.
- Treat filesystem fixtures as source of truth; document indexes (`main.json`) are useful but not authoritative for existence.
