# v3 Transition Guide

GRACE-DOCX v3 changes the mental model from section-aware editing to element-aware editing.

## v1 Model

v1 gives the agent a deterministic map of the document:

```xml
<M-PROC TYPE="CONTENT">
  <heading>Approval Process</heading>
  <BOOKMARK>GRACE_M-PROC</BOOKMARK>
  <PARA-START>120</PARA-START>
  <PARA-END>248</PARA-END>
  <TABLE-COUNT>2</TABLE-COUNT>
</M-PROC>
```

This is enough to find the section and avoid broad context scanning.

## v3 Model

v3 keeps section navigation and adds typed objects inside the section:

```xml
<M-PROC>
  <n>Approval Process</n>
  <TYPE>MIXED</TYPE>
  <BOOKMARK>GRACE_M-PROC</BOOKMARK>
  <PARA-START>120</PARA-START>
  <PARA-END>248</PARA-END>
  <ELEMENTS>
    <element type="TABLE-DATA" para-index="144" columns="4" rows="12"/>
    <element type="CHART-NATIVE" subtype="BAR" para-index="190" source="word/charts/chart1.xml"/>
    <element type="VISUAL-IMAGE" para-index="220" source="word/media/image2.png" readonly="true"/>
  </ELEMENTS>
</M-PROC>
```

This lets the agent choose the correct editing protocol for each object.

## Migration Notes

- Replace v1 `TABLE-COUNT` with v3 `<ELEMENTS>`.
- Replace broad module types with `NARRATIVE`, `DATA`, `MIXED`, `NAVIGATION`, `META`, or `REFERENCE`.
- Move generic table rules into `TypeContracts`.
- Keep module contracts only for section-specific overrides and must-sync dependencies.
- Add verification for protected assets and source files.

## Operational Difference

In v1, a request like "update the chart in section 4" is ambiguous.

In v3, the graph should reveal whether the chart is:

- `CHART-NATIVE`, editable through `word/charts/chartN.xml`;
- `CHART-IMAGE`, readonly unless explicitly replaced as a raster asset;
- `CHART-SMARTART`, editable only through `word/diagrams/dataN.xml`.

