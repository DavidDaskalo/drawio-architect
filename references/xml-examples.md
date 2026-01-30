# DrawIO XML Examples

Quick-reference examples for constructing DrawIO XML elements.

## Minimum Valid Structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mxfile host="app.diagrams.net">
  <diagram name="Architecture" id="arch">
    <mxGraphModel dx="1434" dy="844" grid="1" gridSize="10">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Add shapes here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

## Adding a GCP Service

```xml
<mxCell id="run1" value="Cloud Run"
  style="sketch=0;html=1;fillColor=#4285F4;strokeColor=none;verticalAlign=top;labelPosition=center;verticalLabelPosition=bottom;align=center;spacingTop=-6;fontSize=11;fontStyle=0;fontColor=#999999;shape=mxgraph.gcp2.cloud_run"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="50" height="50" as="geometry" />
</mxCell>
```

## Adding a Connection

```xml
<mxCell id="conn1" value=""
  style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;endArrow=classic;endFill=1;"
  edge="1" parent="1" source="run1" target="bq1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

## Adding a Container

```xml
<mxCell id="vpc1" value="Google Cloud Platform&#xa;VPC-SC"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#E8F5E9;strokeColor=#4CAF50;strokeWidth=2;verticalAlign=top;fontSize=14;fontStyle=1;align=center;container=1;collapsible=0;recursiveResize=0;"
  vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="600" height="400" as="geometry" />
</mxCell>

<!-- Child shapes use parent="vpc1" -->
<mxCell id="run1" ... parent="vpc1">
```

## Connection Labeled Example

```xml
<mxCell id="conn1" value="API Call" edge="1" source="a" target="b"
  style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;endArrow=classic;endFill=1;">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

## Bidirectional Connection

```xml
<mxCell id="conn2" value="Sync" edge="1" source="a" target="b"
  style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;startArrow=classic;startFill=1;endArrow=classic;endFill=1;">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

## Dashed Connection

```xml
<mxCell id="conn3" value="Optional" edge="1" source="a" target="b"
  style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#666666;strokeWidth=1;dashed=1;endArrow=classic;endFill=1;">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

## Complete Small Diagram Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mxfile host="app.diagrams.net">
  <diagram name="Architecture" id="arch">
    <mxGraphModel dx="1434" dy="844" grid="1" gridSize="10">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- VPC-SC Container -->
        <mxCell id="vpc" value="Google Cloud Platform&#xa;VPC-SC"
          style="rounded=1;whiteSpace=wrap;html=1;fillColor=#E8F5E9;strokeColor=#4CAF50;strokeWidth=2;verticalAlign=top;fontSize=14;fontStyle=1;align=center;container=1;collapsible=0;recursiveResize=0;"
          vertex="1" parent="1">
          <mxGeometry x="50" y="50" width="700" height="400" as="geometry" />
        </mxCell>
        <!-- Cloud Scheduler (outside VPC) -->
        <mxCell id="sched" value="Cloud Scheduler"
          style="sketch=0;html=1;fillColor=#4285F4;strokeColor=none;verticalAlign=top;labelPosition=center;verticalLabelPosition=bottom;align=center;spacingTop=-6;fontSize=11;fontStyle=0;fontColor=#999999;shape=mxgraph.gcp2.cloud_scheduler"
          vertex="1" parent="1">
          <mxGeometry x="50" y="500" width="50" height="50" as="geometry" />
        </mxCell>
        <!-- Cloud Run (inside VPC) -->
        <mxCell id="run1" value="Cloud Run"
          style="sketch=0;html=1;fillColor=#4285F4;strokeColor=none;verticalAlign=top;labelPosition=center;verticalLabelPosition=bottom;align=center;spacingTop=-6;fontSize=11;fontStyle=0;fontColor=#999999;shape=mxgraph.gcp2.cloud_run"
          vertex="1" parent="vpc">
          <mxGeometry x="100" y="100" width="50" height="50" as="geometry" />
        </mxCell>
        <!-- BigQuery (inside VPC) -->
        <mxCell id="bq1" value="BigQuery"
          style="sketch=0;html=1;fillColor=#4285F4;strokeColor=none;verticalAlign=top;labelPosition=center;verticalLabelPosition=bottom;align=center;spacingTop=-6;fontSize=11;fontStyle=0;fontColor=#999999;shape=mxgraph.gcp2.bigquery"
          vertex="1" parent="vpc">
          <mxGeometry x="300" y="100" width="50" height="50" as="geometry" />
        </mxCell>
        <!-- Connections -->
        <mxCell id="c1" edge="1" source="sched" target="run1"
          style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;endArrow=classic;endFill=1;">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="c2" edge="1" source="run1" target="bq1"
          style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;endArrow=classic;endFill=1;">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```
