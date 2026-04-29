# GRACE-DOCX v3 Schema Notes

This document summarizes the v3 XML shape embedded into a GRACE-enabled `.docx`.

## Parts

| Part | Required | Purpose |
|---|---:|---|
| `word/grace-manifest.xml` | Yes | Discovery beacon and read order |
| `word/grace-instructions.xml` | Yes | Agent behavior and edit rules |
| `word/grace-graph.xml` | Yes | Module map and element inventory |
| `word/grace-contracts.xml` | Yes | Global rules, type contracts, module contracts |
| `word/grace-verification.xml` | Yes | Structural invariants and post-edit checks |

## Module Shape

```xml
<M-XXX>
  <n>[heading text]</n>
  <TYPE>[NARRATIVE|DATA|MIXED|NAVIGATION|META|REFERENCE]</TYPE>
  <BOOKMARK>GRACE_M-XXX</BOOKMARK>
  <PARA-START>[H1 paragraph index]</PARA-START>
  <PARA-END>[paragraph before next H1 or end]</PARA-END>
  <SubSections/>
  <ELEMENTS/>
</M-XXX>
```

## Element Types

| Type | Key attributes | Contract |
|---|---|---|
| `TABLE-DATA` | `para-index`, `columns`, `rows` | Add rows, edit data cells, preserve columns |
| `TABLE-STRUCT` | `para-index`, `columns`, `rows` | Edit text only, preserve structure |
| `CHART-NATIVE` | `subtype`, `source`, `embedded-data` | Edit chart XML or linked workbook |
| `CHART-IMAGE` | `source`, `readonly` | Do not edit source image |
| `CHART-SMARTART` | `data-source`, `layout-source` | Edit text data only |
| `VISUAL-IMAGE` | `source`, `readonly` | Do not edit source image |
| `EMBEDDED` | `readonly` | Host application required |

## Contract Resolution

Agents should resolve edit permissions in this order:

1. Read `GlobalRules`.
2. Identify element type from `grace-graph.xml`.
3. Read matching `TypeContracts` entry.
4. Read module-specific contract overrides.
5. Apply the strictest relevant rule.

## Verification Expectations

v3 verification should confirm:

- GRACE bookmarks remain balanced and correctly scoped.
- Every H1 has a graph module.
- All GRACE XML parts are well-formed.
- Table column counts remain stable.
- Readonly media files are unchanged unless explicitly replaced.
- Native chart and SmartArt source files referenced in the graph exist.
- SmartArt layout files remain unchanged.
- The element inventory is updated when tables, charts, or images are added or removed.

