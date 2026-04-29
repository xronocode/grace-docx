# GRACE-DOCX v3.0.0: Element-Aware Bootstrap

GRACE-DOCX v3 upgrades the bootstrap from section-level document navigation to element-level document intelligence.

The bootstrap now inventories non-trivial `.docx` elements inside each H1 module: data tables, structural tables, native charts, raster chart images, SmartArt diagrams, visual images, and embedded OLE objects. Each element type gets explicit edit contracts and verification rules, allowing agents to make safer surgical edits without treating the document as plain text or generic XML.

## Release Summary

v1 made a document section-aware.

v3 makes it element-aware.

That means an agent can distinguish a live chart from a pasted chart image, a data table from a structural matrix, and editable SmartArt text from forbidden SmartArt layout topology.

## Highlights

- Element inventory per module through `<ELEMENTS>`.
- Type-specific editing contracts.
- Readonly handling for raster images and embedded objects.
- Native chart editing through chart XML instead of `document.xml` drawing references.
- SmartArt text editing through diagram data XML while preserving layout XML.
- Stronger verification around image hashes, chart sources, and inventory freshness.

## Compatibility

This is a schema-level upgrade from `1.0.0` to `3.0.0`.

Existing v1-bootstrapped documents remain readable as section-aware GRACE-DOCX files, but they do not contain the v3 element inventory. To use v3 behavior, run the v3 bootstrap on the source `.docx` or migrate the embedded GRACE parts.

## Suggested GitHub Release Text

```md
GRACE-DOCX v3.0.0 introduces the element-aware bootstrap.

The project now maps not only document sections, but also the editable and readonly objects inside them: tables, native charts, chart images, SmartArt, visual images, and embedded OLE objects. Agents can now select the correct editing protocol for each object type and verify that protected document internals remain intact.

Use `grace-docx-bootstrap.md` for the latest stable v3 prompt. The previous v1 prompt is preserved under `archive/`.
```

## Suggested Tag

```text
v3.0.0
```

