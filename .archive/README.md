# DrawIO Architect Skill

Convert architecture diagram images to DrawIO XML format, with focus on GCP cloud services.

## Quick Start

### Analyze an Existing DrawIO File
```
Read the file and use SKILL.md Task 1 workflow to extract:
- All shapes and their types
- Connections between components
- Container hierarchy
- Style information
```

### Convert Image to DrawIO
```
1. Analyze the image to identify GCP services
2. Look up shapes in libraries/gcp-icons.json
3. Generate XML using templates/drawio-base.xml
4. Add shapes, containers, and connections
```

### Create from Description
```
"Create a GCP diagram with Cloud Scheduler triggering Cloud Run,
which queries BigQuery and stores results in Cloud Storage"

→ Generates valid .drawio file
```

## Files

| File | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Main skill instructions |
| [libraries/gcp-icons.json](libraries/gcp-icons.json) | 35 GCP service definitions |
| [libraries/containers.json](libraries/containers.json) | Container and connection styles |
| [templates/](templates/) | XML templates for generation |
| [utilities/](utilities/) | Parser, coordinate, and style guides |
| [examples/](examples/) | Sample DrawIO files |

## GCP Services Included

**Compute:** Cloud Run, Compute Engine, GKE, Cloud Functions, App Engine

**Database:** BigQuery, Cloud SQL, Firestore, Spanner, Bigtable, Memorystore

**Storage:** Cloud Storage, Filestore, Persistent Disk

**Networking:** VPC, Load Balancing, CDN, DNS, Armor

**AI/ML:** Vertex AI, AI Platform, Vision, NLP, Speech-to-Text

**Integration:** Pub/Sub, Cloud Tasks, Workflows, Eventarc, Scheduler

**Operations:** Logging, Monitoring, Trace, Error Reporting

**API Management:** Apigee, API Gateway

## Shape Code Pattern

All GCP shapes use: `shape=mxgraph.gcp2.{service_name}`

Examples:
- `mxgraph.gcp2.cloud_run`
- `mxgraph.gcp2.bigquery`
- `mxgraph.gcp2.cloud_storage`

## Usage Examples

### Add a Cloud Run Service
```xml
<mxCell id="run1" value="My Service"
  style="...shape=mxgraph.gcp2.cloud_run"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="50" height="50" as="geometry" />
</mxCell>
```

### Add a VPC-SC Container
```xml
<mxCell id="vpc" value="Google Cloud Platform&#xa;VPC-SC"
  style="rounded=1;fillColor=#E8F5E9;strokeColor=#4CAF50;strokeWidth=2;container=1;..."
  vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="600" height="400" as="geometry" />
</mxCell>
```

### Connect Two Services
```xml
<mxCell id="conn1"
  style="edgeStyle=orthogonalEdgeStyle;endArrow=classic;..."
  edge="1" parent="1" source="run1" target="bq1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

## Testing

Open example files in DrawIO (app.diagrams.net) to verify:
- [examples/sample-gcp-simple.drawio](examples/sample-gcp-simple.drawio) - Basic 4-service diagram
- [examples/sample-gcp-complex.drawio](examples/sample-gcp-complex.drawio) - Multi-service with containers
