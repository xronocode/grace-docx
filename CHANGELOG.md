# Changelog

## Unreleased

### Added

- `grace-pptx-bootstrap.md` as a preview bootstrap for PowerPoint decks.
- GRACE-PPTX v1 schema notes under `docs/pptx-schema-v1.md`.
- PowerPoint support references in README and publishing docs.

## v3.0.0 - Element-Aware Bootstrap

GRACE-DOCX v3 upgrades the bootstrap from section-level navigation to element-level document intelligence.

### Added

- Element inventory per H1 module through `<ELEMENTS>`.
- Typed support for `TABLE-DATA`, `TABLE-STRUCT`, `CHART-NATIVE`, `CHART-IMAGE`, `CHART-SMARTART`, `VISUAL-IMAGE`, and `EMBEDDED`.
- `TypeContracts` for reusable element-level editing rules.
- Readonly rules for raster images and embedded OLE objects.
- Native chart routing through `word/charts/chartN.xml`.
- SmartArt text-only editing through `word/diagrams/dataN.xml`.
- Verification checks for chart sources, image hashes, SmartArt layout integrity, and inventory freshness.
- GitHub README diagrams under `assets/`.
- Release and transition documentation under `docs/`.

### Changed

- `grace-docx-bootstrap.md` is now the v3 bootstrap.
- Schema version moved from `1.0.0` to `3.0.0`.
- Module types now use `NARRATIVE`, `DATA`, `MIXED`, `NAVIGATION`, `META`, and `REFERENCE`.
- `grace-graph.xml` now describes editable document objects, not just section ranges and table counts.
- `grace-contracts.xml` now separates global rules, element type contracts, and module-specific overrides.
- Bootstrap report now includes element inventory counts and classification flags.

### Archived

- The previous v1 prompt is preserved at `archive/grace-docx-bootstrap-v1.md`.

## v1.0.0 - Section-Aware Bootstrap

- Initial bootstrap prompt for injecting GRACE semantic markup into `.docx` files.
- H1/H2 section map.
- Paragraph ranges.
- GRACE bookmarks.
- Five embedded XML parts.
- Module contracts and basic verification rules.
