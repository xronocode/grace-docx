# GRACE-PPTX v1 Schema Notes

GRACE-PPTX applies the GRACE document pattern to PowerPoint decks. The core unit is not a paragraph section, but a slide module with shape-level inventory.

## Package Parts

| Part | Required | Purpose |
|---|---:|---|
| `ppt/grace-manifest.xml` | Yes | Discovery, read order, style fingerprint |
| `ppt/grace-instructions.xml` | Yes | Agent behavior and anti-patterns |
| `ppt/grace-graph.xml` | Yes | Slide order, modules, shape inventory, cross-links |
| `ppt/grace-contracts.xml` | Yes | Type and module edit contracts |
| `ppt/grace-verification.xml` | Yes | Structural invariants and post-edit checks |

## Authoritative Slide Order

Slide order comes from:

```text
ppt/presentation.xml
  p:sldIdLst / p:sldId[@r:id]
    -> ppt/_rels/presentation.xml.rels
       Relationship[@Id = r:id] / @Target
```

Do not infer order from `slide1.xml`, `slide2.xml`, or filename sorting.

## Module Shape

```xml
<M-XXX>
  <n>[section or detected module name]</n>
  <TYPE>[NARRATIVE|DATA|NAVIGATION|META|MIXED|EMPTY]</TYPE>
  <POSITIONS>[1-3]</POSITIONS>
  <SLIDE-FILES>
    <file position="1">ppt/slides/slide3.xml</file>
  </SLIDE-FILES>
  <HAS-NOTES>[true|false]</HAS-NOTES>
  <ELEMENTS/>
</M-XXX>
```

## Element Identity

Every slide element should be addressable by stable technical identity, not only by visible text:

```xml
<element type="TEXT-PLACEHOLDER"
         slide="4"
         shape-id="7"
         shape-name="Title 1"
         placeholder="title"
         tree-path="/p:sld/p:cSld/p:spTree/p:sp[2]"/>
```

## Element Types

| Type | Main identity | Edit policy |
|---|---|---|
| `TEXT-PLACEHOLDER` | slide, shape-id, placeholder type | Edit text, preserve placeholder |
| `TEXT-BOX` | slide, shape-id, tree path | Edit text, preserve geometry |
| `TABLE-DATA` | slide, shape-id, rows, columns | Edit values, preserve columns |
| `TABLE-STRUCT` | slide, shape-id, rows, columns | Edit cell text only |
| `CHART-NATIVE` | chart source relationship | Edit chart XML or workbook |
| `CHART-IMAGE` | media source relationship | Readonly unless replacement requested |
| `CHART-SMARTART` | diagram data/layout sources | Edit data text, preserve layout |
| `VISUAL-IMAGE` | media source relationship | Readonly unless replacement requested |
| `EMBEDDED` | OLE relationship | Host application required |
| `NOTES` | notes slide source | Edit note text only |
| `ANIMATION` | p:timing/p:transition | Preserve by default |

## Contract Resolution

Agents should resolve permissions in this order:

1. `GlobalRules`
2. element `TypeContracts`
3. module-specific overrides
4. explicit user instruction

The strictest relevant rule wins unless the user explicitly requests a broader structural operation.

## Verification Priorities

- Slide order still matches `p:sldIdLst`.
- Every slide position appears in exactly one module.
- Shape IDs in graph still resolve.
- Protected theme, layout, and master files did not change during content edits.
- Readonly media did not change unless explicitly replaced.
- SmartArt layout files did not change.
- Animations and transitions were preserved.
- Agenda and internal hyperlinks still resolve after structural edits.
