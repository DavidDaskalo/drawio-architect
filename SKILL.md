---
name: drawio-architect
description: "[DEPRECATED] This unified skill has been split into provider-specific skills. Use generating-gcp-diagrams for GCP or generating-aws-diagrams for AWS instead."
license: MIT
compatibility: Requires image analysis capability for Task 2 (image conversion). Scripts require Python 3 and Bash.
allowed-tools: Read Write
metadata:
  author: user
  version: "1.4"
  deprecated: true
  replaced_by: ["generating-gcp-diagrams", "generating-aws-diagrams"]
---

# DrawIO Architect Skill (DEPRECATED)

> **⚠️ DEPRECATED:** This unified skill has been replaced with provider-specific skills:
> - **[generating-gcp-diagrams](/.claude/skills/generating-gcp-diagrams/)** - For Google Cloud Platform
> - **[generating-aws-diagrams](/.claude/skills/generating-aws-diagrams/)** - For Amazon Web Services
>
> **Why?** The split provides:
> - Smaller, focused skills (~430-450 lines vs 491)
> - Provider-specific best practices
> - Independent versioning
> - Clearer invocation context
>
> See [README.md](/README.md#migration-guide) for migration details.

---

## Original Content (v1.4)

Converts architecture diagram images to DrawIO XML format, supporting **GCP** and **AWS** cloud services.

## Capabilities

1. **Extract** - Analyze existing DrawIO XML files to identify shapes, connections, and structure
2. **Identify** - Recognize GCP service icons from architecture diagram images
3. **Generate** - Create valid DrawIO XML from images or text descriptions
4. **Convert** - Transform architecture diagrams into editable DrawIO format

## Quick Reference

### Provider Detection

Determine the cloud provider from the user's request:
- **GCP**: mentions Google Cloud, GCP, Cloud Run, BigQuery, Pub/Sub, etc.
- **AWS**: mentions AWS, Amazon, Lambda, S3, EC2, DynamoDB, etc.
- **Multi-cloud**: mentions services from multiple providers

### GCP Shape Code Pattern
```
shape=mxgraph.gcp2.{service_name}
```

### AWS Shape Code Pattern
```
shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.{shape_name}
```
**Note:** AWS shape names use **spaces** not underscores or hyphens (e.g., `step functions`, `kinesis data streams`). Always look up exact names in `assets/aws-icons.json`.

### Common GCP Services
| Service | Shape Code |
|---------|-----------|
| Cloud Run | `mxgraph.gcp2.cloud_run` |
| BigQuery | `mxgraph.gcp2.bigquery` |
| Cloud Storage | `mxgraph.gcp2.cloud_storage` |
| Vertex AI | `mxgraph.gcp2.cloud_machine_learning` |
| Cloud Scheduler | `mxgraph.gcp2.cloud_scheduler` |
| Apigee | `mxgraph.gcp2.apigee_api_platform` |
| Pub/Sub | `mxgraph.gcp2.cloud_pubsub` |
| Cloud SQL | `mxgraph.gcp2.cloud_sql` |

### Common AWS Services
| Service | Shape Code |
|---------|-----------|
| Lambda | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.lambda` |
| Amazon S3 | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.s3` |
| Amazon EC2 | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.ec2` |
| DynamoDB | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.dynamodb` |
| API Gateway | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.api gateway` |
| Amazon SQS | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.sqs` |
| Amazon SNS | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.sns` |
| Step Functions | `mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.step functions` |

---

## Task 1: Analyze a DrawIO File

Use this workflow to extract and document all components from an existing DrawIO file.

### Steps

1. **Read the file** - Load the `.drawio` XML file
2. **Parse structure** - Extract all `<mxCell>` elements
3. **Identify shapes** - Find cells with `vertex="1"`
4. **Identify connections** - Find cells with `edge="1"`
5. **Extract styles** - Parse style strings for each element
6. **Map hierarchy** - Build container/child relationships using `parent` attribute
7. **Generate report** - Output findings in structured format

### Input
- Path to `.drawio` file

### Output
Generate a Markdown report with:

```markdown
# DrawIO Analysis Report

## Summary
- Total shapes: X
- Total connections: Y
- Containers: Z

## Shape Inventory

| ID | Label | Type | Position | Parent |
|----|-------|------|----------|--------|
| abc | Cloud Run | mxgraph.gcp2.cloud_run | (100,200) | vpc1 |

## Connection Matrix

| From | To | Label | Type |
|------|-----|-------|------|
| Cloud Run | BigQuery | API | solid |

## Container Hierarchy

- VPC-SC (vpc1)
  - Cloud Run (run1)
  - Cloud Run (run2)
  - BigQuery (bq1)

## Style Analysis

### Unique Shapes Found
- mxgraph.gcp2.cloud_run (4 instances)
- mxgraph.gcp2.bigquery (2 instances)
```

---

## Task 2: Convert Image to DrawIO

Use this workflow to recreate an architecture diagram from an image.

### Steps

1. **Analyze image** - Identify all visual elements:
   - Service icons (shape, color, label)
   - Containers/boundaries (color, border style)
   - Connections (solid, dashed, arrows)
   - Labels and text

2. **Map to library** - For each identified element:
   - Determine provider (GCP or AWS) from icon style/labels
   - Look up in `assets/gcp-icons.json` or `assets/aws-icons.json` by visual signature or label
   - Match containers to `assets/containers.json` (GCP) or `assets/aws-containers.json` (AWS)
   - Note any unrecognized elements

3. **Estimate layout** - Determine positions:
   - Identify container boundaries first
   - Place icons within containers
   - Estimate x,y coordinates and dimensions
   - Standard icon size: 50x50 pixels (GCP), 64x64 pixels (AWS)

4. **Generate XML** - Build the DrawIO structure:
   - Start with base template from `assets/templates/drawio-base.xml`
   - Add containers first (they become parents)
   - Add service icons with correct parent references
   - Add connections between shapes

5. **Create confidence report** - Document accuracy:
   - List all identified components
   - Note any uncertain matches
   - Flag potential issues

### Input
- Architecture diagram image (PNG/JPG)

### Output
1. Valid `.drawio` XML file
2. Confidence report (Markdown)

### Confidence Report Format

```markdown
# Conversion Confidence Report

## Overall Confidence: 85%

## Identified Components

### High Confidence (>90%)
- Cloud Run x4 - Clear icon match
- BigQuery x2 - Clear icon match
- VPC-SC container - Green border, correct label

### Medium Confidence (70-90%)
- Vertex AI Search - Icon similar, label confirms

### Low Confidence (<70%)
- Unknown icon at position (300, 400) - Mapped to generic service

## Connection Accuracy
- 12/14 connections clearly visible
- 2 connections inferred from layout

## Notes
- "Same Instance" dashed container identified
- Bidirectional arrows on 3 connections
```

---

## Task 3: Create DrawIO from Description

Use this workflow to generate a new diagram from text specifications.

### Steps

1. **Parse requirements** - Extract from description:
   - Required services
   - **Cloud provider** (GCP, AWS, or multi-cloud)
   - Container/grouping needs
   - Connection requirements
   - Layout preferences

2. **Select components** - From libraries:
   - **GCP**: Look up services in `assets/gcp-icons.json`, containers in `assets/containers.json`
   - **AWS**: Look up services in `assets/aws-icons.json`, containers in `assets/aws-containers.json`
   - Choose connection styles (provider-specific arrow styles)

3. **Plan layout** - Design the arrangement:
   - Determine canvas size
   - Position containers first
   - Arrange services logically (left-to-right data flow, top-to-bottom hierarchy)
   - Standard spacing: 100px between icons (GCP 50x50), 120px between icons (AWS 64x64)

4. **Generate XML** - Build the diagram:
   - Use `assets/templates/drawio-base.xml` as starting point
   - Add elements in order: containers, services, connections
   - Assign unique IDs to all elements

5. **Validate** - Check the output:
   - All requested components present
   - Connections reference valid IDs
   - Layout is logical and readable

### Input
- Text description of desired architecture

### Output
- Valid `.drawio` XML file

### Example Input

```
Create a GCP architecture with:
- VPC-SC container
- Cloud Scheduler triggering Cloud Run
- Cloud Run connecting to BigQuery and Cloud Storage
- Vertex AI Search connected to BigQuery
```

### Example Output Structure

```xml
<mxfile ...>
  <diagram name="Architecture">
    <mxGraphModel ...>
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- VPC-SC Container -->
        <mxCell id="vpc" value="VPC-SC" style="..." vertex="1" parent="1">
          <mxGeometry x="50" y="50" width="700" height="400" />
        </mxCell>
        <!-- Cloud Scheduler -->
        <mxCell id="sched" value="Cloud Scheduler" style="...mxgraph.gcp2.cloud_scheduler" vertex="1" parent="vpc">
          <mxGeometry x="50" y="100" width="50" height="50" />
        </mxCell>
        <!-- More shapes... -->
        <!-- Connections -->
        <mxCell id="conn1" edge="1" source="sched" target="run" style="..." />
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## Shape Library Reference

### Looking Up a GCP Service

1. Open `assets/gcp-icons.json`
2. Search by `service_name` or `recognition_keywords`
3. Use the `drawio_shape.full_style` for complete styling
4. Or construct style using `shape=mxgraph.gcp2.{shape_name}`

**Note:** All 35 GCP service icons have been validated against DrawIO's built-in gcp2 library. 33/35 icons have exact shape matches, while 2 services (Workflows, Eventarc) use fallback icons as they're newer services not yet in the library. See [assets/ICON-COMPATIBILITY.md](assets/ICON-COMPATIBILITY.md) for complete validation details.

### Looking Up an AWS Service

1. Open `assets/aws-icons.json`
2. Search by `service_name` or `recognition_keywords`
3. Use the `drawio_shape.full_style` for complete styling
4. Or construct style using the pattern: `shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.{shape_name}`

**Key AWS differences from GCP:**
- Icon size: **64x64** (not 50x50)
- Font color: **#232F3E** universally (GCP varies)
- Fill color varies by **category** (not per-service)
- Arrow style: open arrow, 2pt stroke, `#232F3E`

**Note:** AWS icons cover Part 1 (10 categories, ~112 services). Part 2 categories will be added in a future update.

### GCP Service Categories

| Category | Services |
|----------|----------|
| compute | Cloud Run, Compute Engine, GKE, Cloud Functions, App Engine |
| database | BigQuery, Cloud SQL, Firestore, Spanner, Bigtable, Memorystore |
| storage | Cloud Storage, Filestore, Persistent Disk |
| networking | VPC, Load Balancing, CDN, DNS, Armor |
| ai_ml | Vertex AI, AI Platform, Vision, NLP, Speech-to-Text |
| integration | Pub/Sub, Cloud Tasks, Workflows, Eventarc, Scheduler |
| operations | Logging, Monitoring, Trace, Error Reporting |
| api_management | Apigee, API Gateway |

### AWS Service Categories (Part 1)

| Category | Color | Example Services |
|----------|-------|------------------|
| Analytics | #8C4FFF | Athena, Kinesis, QuickSight, Redshift, Glue |
| Application Integration | #E7157B | API Gateway, EventBridge, SNS, SQS, Step Functions |
| Artificial Intelligence | #01A88D | Bedrock, SageMaker, Lex, Polly, Rekognition |
| Blockchain | #ED7100 | Managed Blockchain |
| Business Applications | #E7157B | Connect, SES, WorkSpaces, Chime |
| Cloud Financial Management | #C7131F | Cost Explorer, Budgets |
| Compute | #ED7100 | EC2, Lambda, ECS, Fargate, Batch |
| Containers | #ED7100 | ECS, EKS, Fargate, ECR |
| Customer Enablement | #C7131F | Support, Managed Services, IQ |
| Customer Experience | #E7157B | Connect |

### GCP Container Types

| Container | Use Case |
|-----------|----------|
| gcp_project | Main project boundary (two-cell pattern: rectangle + GCP logo child) |
| gcp_vpc_sc | VPC Service Controls perimeter (green border, `fontColor=#2E7D32`) |
| gcp_region | Regional grouping |
| logical_group_dashed | Logical grouping with dashed border (`fontColor=#424242`) |
| logical_group_solid | Solid border grouping |

### AWS Container Types

| Container | Use Case |
|-----------|----------|
| aws_cloud | Top-level AWS Cloud boundary (gray border) |
| aws_region | Region grouping (teal border) |
| aws_availability_zone | AZ grouping (teal dashed border) |
| aws_vpc | VPC boundary (purple border) |
| aws_public_subnet | Public subnet (green filled) |
| aws_private_subnet | Private subnet (blue filled) |
| aws_security_group | Security group boundary (red filled) |
| aws_auto_scaling_group | Auto scaling group (orange filled) |
| aws_account | AWS Account boundary (pink border) |

---

## Visual Best Practices

Rules learned from iterating on diagram output in DrawIO Desktop:

### GCP Project Zone (Two-Cell Pattern)

The GCP Project container uses **two cells**, not one. Do NOT set `shape=` on the container cell.

1. **Container cell**: Plain rectangle with `fillColor=#F6F6F6;strokeColor=none;` and HTML value `<b>Google </b>Cloud Platform`
2. **Logo child cell**: `shape=mxgraph.gcp2.google_cloud_platform` at 23x20px with `relative=1` geometry

See `assets/templates/node-template.xml` for the exact template.

### Font Colors

**GCP:**
- Service icon labels: `fontColor=#424242` (dark gray — never use `#999999`, too light on white)
- GCP Project zone text: `fontColor=#717171` (matches native DrawIO GCP zone)
- VPC container title: `fontColor=#2E7D32` (dark green)
- Dashed group labels: `fontColor=#424242`

**AWS:**
- All labels: `fontColor=#232F3E` (universal dark gray)
- Group labels: match stroke color of the container

### Connection Labels

Always add these properties to labeled connections:
```
labelBackgroundColor=#FFFFFF;fontSize=10;fontColor=#333333;
```
This prevents label text from overlapping icons and ensures readability.

### Connection Consolidation

When 3+ connections cross the same corridor between two groups:
- **Consolidate** into a single connection targeting the container group (not individual nodes)
- Use combined labels like `write / load` or `read / write`
- Connections CAN target container cells directly — this is the preferred approach
- Maximum 2 parallel connectors between any pair of groups

---

## XML Structure Quick Reference

The key building blocks for DrawIO XML:

- **Shape**: `<mxCell id="..." value="Label" style="..." vertex="1" parent="1">` with `<mxGeometry>`
- **Connection**: `<mxCell id="..." edge="1" source="..." target="..." style="...">`
- **Container**: Shape with `container=1` in style; children set `parent` to container ID
- **Root cells**: Every diagram needs `<mxCell id="0"/>` and `<mxCell id="1" parent="0"/>`

For complete XML examples (shapes, connections, containers, full diagrams), see [references/xml-examples.md](references/xml-examples.md).

For XML parsing and extraction techniques, see [references/xml-parser-guide.md](references/xml-parser-guide.md).

---

## Troubleshooting

### Icon Not Displaying
- **GCP**: Verify shape name matches exactly: `mxgraph.gcp2.cloud_run` (underscore, not hyphen)
- **AWS**: Verify both `shape=mxgraph.aws4.resourceIcon` AND `resIcon=mxgraph.aws4.{name}` are present
- Check [assets/ICON-COMPATIBILITY.md](assets/ICON-COMPATIBILITY.md) for correct GCP shape names
- Ensure `vertex="1"` is present
- Check that `<mxGeometry>` has valid width/height (50x50 GCP, 64x64 AWS)
- For GCP Workflows/Eventarc, note these don't have exact icon matches in DrawIO's library

### Connection Not Rendering
- Verify source and target IDs exist
- Ensure `edge="1"` is set
- Check that source/target shapes are vertices

### Shapes Not Inside Container
- Set child's `parent` attribute to container's ID
- Ensure container has `container=1` in style
- Position child coordinates relative to container (not absolute)

### Label Not Showing
- Check `value` attribute is set
- Verify `fontSize` is reasonable (11-14)
- Ensure `fontColor` is visible against background

### File Won't Open in DrawIO
- Validate XML structure (proper closing tags)
- Ensure `id="0"` and `id="1"` root cells exist
- Check for special characters in labels (use `&#xa;` for newlines)

---

## Desktop Integration

After generating a `.drawio` file, you can validate and preview it:

1. **Validate**: `python scripts/validate-drawio.py output.drawio --verbose`
2. **Analyze**: `python scripts/analyze-existing.py output.drawio --markdown`
3. **Export PNG**: `./scripts/export-diagram.sh output.drawio png`
4. **Open in DrawIO**: `./scripts/open-diagram.sh output.drawio`

Requires DrawIO Desktop. Install on macOS: `brew install drawio`

---

## Files in This Skill

| File | Purpose |
|------|---------|
| `SKILL.md` | This file - main instructions |
| `README.md` | Project overview and quick start guide |
| **Assets** | |
| `assets/gcp-icons.json` | GCP service icon database (35 services, 94.3% validated) |
| `assets/aws-icons.json` | AWS service icon database (112 services, Part 1 of 2) |
| `assets/ICON-COMPATIBILITY.md` | Icon validation reference and compatibility guide |
| `assets/containers.json` | GCP container and connection styles |
| `assets/aws-containers.json` | AWS container/group and connection styles |
| `assets/templates/drawio-base.xml` | Base XML template |
| `assets/templates/node-template.xml` | Shape insertion template |
| `assets/templates/connection-template.xml` | Connection template |
| `assets/examples/` | Sample diagrams for reference |
| **References** | |
| `references/DIAGRAM-BEST-PRACTICES.md` | Visual design and layout guidelines |
| `references/xml-parser-guide.md` | Detailed XML parsing reference |
| `references/xml-examples.md` | Copy-paste XML examples |
| `references/coordinate-system.md` | Positioning and layout guide |
| `references/style-guide.md` | Style string reference |
| **Scripts** | |
| `scripts/validate-drawio.py` | Validate .drawio XML structure |
| `scripts/validate-gcp-icons.py` | Validate GCP icon compatibility with DrawIO gcp2 library |
| `scripts/validate-aws-icons.py` | Validate AWS icon compatibility with DrawIO aws4 library |
| `scripts/fix-gcp-icons.py` | Auto-fix icon shape names in gcp-icons.json |
| `scripts/fix-aws-icons.py` | Auto-fix icon shape names in aws-icons.json |
| `scripts/fix-drawio-icons.py` | Bulk fix icon references in .drawio files |
| `scripts/extract-shape-names.py` | Extract available shapes from DrawIO stencil |
| `scripts/analyze-existing.py` | Extract shapes/connections from .drawio files |
| `scripts/export-diagram.sh` | Export to PNG/PDF via DrawIO Desktop CLI |
| `scripts/open-diagram.sh` | Open .drawio file in DrawIO Desktop |
