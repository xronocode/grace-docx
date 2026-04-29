# PowerPoint Roadmap

The same GRACE pattern can be applied to `.pptx` files, but the object model should be slide-first instead of section-first.

## Working Name

```text
GRACE-PPTX
```

## Expected Bootstrap Units

| DOCX concept | PPTX equivalent |
|---|---|
| H1 module | Slide or slide group |
| Paragraph range | Shape tree path |
| Table element | Slide table shape |
| Native chart | `ppt/charts/chartN.xml` |
| SmartArt | `ppt/diagrams/dataN.xml` and layout files |
| Visual image | `ppt/media/imageN.*` |
| Bookmark | Stable shape ID or custom tag |

## Candidate Embedded Parts

```text
ppt/grace-manifest.xml
ppt/grace-instructions.xml
ppt/grace-graph.xml
ppt/grace-contracts.xml
ppt/grace-verification.xml
```

## Main Design Question

Word has section headings and paragraph ranges. PowerPoint has slides, layouts, placeholders, and shape trees.

The PPTX version should probably identify modules as:

- each slide by default;
- logical slide groups when titles follow a repeated section pattern;
- specific named shapes for high-risk edits.

## First Bootstrap Goal

The first useful GRACE-PPTX version should inventory:

- slide titles;
- text boxes and placeholders;
- tables;
- native charts;
- SmartArt;
- images;
- speaker notes;
- layout and theme dependencies.

It should be conservative: edit text and chart data first, preserve slide layouts, masters, themes, and shape geometry unless explicitly requested.

