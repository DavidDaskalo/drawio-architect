# DrawIO Analysis Report

**File:** sample-gcp-complex.drawio
**Analyzed:** 2024-01-24

## Summary

| Metric | Count |
|--------|-------|
| Total Shapes | 10 |
| Total Connections | 10 |
| Containers | 2 |
| GCP Services | 8 |

## Shape Inventory

| ID | Label | Shape Type | Position | Parent |
|----|-------|------------|----------|--------|
| scheduler | Cloud Scheduler | mxgraph.gcp2.cloud_scheduler | (60, 200) | root |
| apigee | Apigee | mxgraph.gcp2.apigee | (60, 400) | root |
| vpc | VPC-SC | container | (180, 80) | root |
| sameinstance | Same Instance | container (dashed) | (50, 80) | vpc |
| run_data | data transfer | mxgraph.gcp2.cloud_run | (30, 60) | sameinstance |
| run_snow | snow agent | mxgraph.gcp2.cloud_run | (130, 60) | sameinstance |
| run_root | root agent | mxgraph.gcp2.cloud_run | (30, 180) | sameinstance |
| run_sharepoint | sharepoint | mxgraph.gcp2.cloud_run | (130, 180) | sameinstance |
| bq1 | BigQuery | mxgraph.gcp2.bigquery | (500, 150) | vpc |
| bq2 | BigQuery | mxgraph.gcp2.bigquery | (500, 300) | vpc |
| storage | Visual Data | mxgraph.gcp2.cloud_storage | (700, 150) | vpc |
| vertex | Vertex AI Search Vector DB | mxgraph.gcp2.vertexai | (900, 220) | vpc |

## Connection Matrix

| From | To | Label | Type |
|------|-----|-------|------|
| Cloud Scheduler | data transfer | - | solid |
| Apigee | root agent | - | solid |
| data transfer | BigQuery | - | bidirectional |
| snow agent | BigQuery | - | solid |
| root agent | BigQuery | - | solid |
| sharepoint | BigQuery | - | solid |
| BigQuery | Visual Data | - | solid |
| BigQuery | Vertex AI | - | dashed |
| root agent | Vertex AI | - | bidirectional |

## Container Hierarchy

```
root (id="1")
├── Cloud Scheduler
├── Apigee
└── VPC-SC (vpc)
    ├── Same Instance (sameinstance)
    │   ├── data transfer (Cloud Run)
    │   ├── snow agent (Cloud Run)
    │   ├── root agent (Cloud Run)
    │   └── sharepoint (Cloud Run)
    ├── BigQuery x2
    ├── Visual Data (Cloud Storage)
    └── Vertex AI Search Vector DB
```

## Style Analysis

### Unique Shape Types
| Shape | Count | Color |
|-------|-------|-------|
| mxgraph.gcp2.cloud_run | 4 | #4285F4 |
| mxgraph.gcp2.bigquery | 2 | #4285F4 |
| mxgraph.gcp2.cloud_storage | 1 | #4285F4 |
| mxgraph.gcp2.vertexai | 1 | #4285F4 |
| mxgraph.gcp2.cloud_scheduler | 1 | #4285F4 |
| mxgraph.gcp2.apigee | 1 | #FF6D00 |

### Container Types
| Type | Style | Color |
|------|-------|-------|
| VPC-SC | solid, rounded | #E8F5E9 / #4CAF50 |
| Same Instance | dashed, rounded | transparent / #757575 |

### Connection Types
| Type | Count |
|------|-------|
| Solid arrow | 6 |
| Bidirectional | 2 |
| Dashed arrow | 1 |
