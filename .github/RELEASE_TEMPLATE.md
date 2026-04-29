# GRACE-DOCX v3.0.0: Element-Aware Bootstrap

GRACE-DOCX v3 upgrades the bootstrap from section-level navigation to element-level document intelligence.

The bootstrap now inventories non-trivial `.docx` elements inside each H1 module: data tables, structural tables, native charts, raster chart images, SmartArt diagrams, visual images, and embedded OLE objects. Each element type gets explicit edit contracts and verification rules, allowing agents to make safer surgical edits without treating the document as plain text or generic XML.

## Highlights

- Element inventory per module through `<ELEMENTS>`.
- Type-specific editing contracts for tables, charts, SmartArt, images, and embedded objects.
- Native chart editing through `word/charts/chartN.xml`.
- SmartArt text editing through `word/diagrams/dataN.xml` while preserving layout XML.
- Readonly protection for raster media and OLE objects.
- Stronger verification checks for protected assets and source files.

## Upgrade Notes

- `grace-docx-bootstrap.md` is now the latest stable v3 prompt.
- The previous v1 prompt is preserved at `archive/grace-docx-bootstrap-v1.md`.
- Existing v1-bootstrapped documents remain section-aware, but need v3 bootstrap or migration to gain element inventory.

## Files

- `grace-docx-bootstrap.md`
- `grace-docx-bootstrap-v3.md`
- `docs/release-v3.md`
- `docs/v3-transition.md`
- `docs/schema-v3.md`
- `assets/*.svg`

