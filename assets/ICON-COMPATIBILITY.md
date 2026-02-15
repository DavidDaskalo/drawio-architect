# GCP Icon Compatibility Reference

**Last Updated:** 2026-01-31
**DrawIO Library:** mxgraph.gcp2
**Validation Success Rate:** 94.3% (33/35 icons)

This document provides comprehensive information about GCP icon compatibility with DrawIO's built-in gcp2 shape library.

---

## Quick Summary

- ✅ **33 services** have exact shape matches in DrawIO's gcp2 library
- ⚠️ **2 services** (Workflows, Eventarc) don't have exact matches (newer services)
- 🔧 All icon shape names have been validated and corrected
- 🎨 All font colors standardized to `#424242` (proper dark gray)

---

## Validated & Working Icons (33 services)

All of the following services render correctly in DrawIO Desktop with their proper icons:

### Compute (5 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Cloud Run | `cloud_run` | ✅ |
| Compute Engine | `compute_engine` | ✅ |
| Google Kubernetes Engine | `container_engine` | ✅ |
| Cloud Functions | `cloud_functions` | ✅ |
| App Engine | `app_engine` | ✅ |

### Database (6 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| BigQuery | `bigquery` | ✅ |
| Cloud SQL | `cloud_sql` | ✅ |
| Cloud Firestore | `cloud_firestore` | ✅ |
| Cloud Spanner | `cloud_spanner` | ✅ |
| Cloud Bigtable | `cloud_bigtable` | ✅ |
| Memorystore | `cloud_memorystore` | ✅ |

### Storage (3 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Cloud Storage | `cloud_storage` | ✅ |
| Filestore | `cloud_filestore` | ✅ |
| Persistent Disk | `persistent_disk` | ✅ |

### Networking (5 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Virtual Private Cloud | `virtual_private_cloud` | ✅ |
| Cloud Load Balancing | `cloud_load_balancing` | ✅ |
| Cloud CDN | `cloud_cdn` | ✅ |
| Cloud DNS | `cloud_dns` | ✅ |
| Cloud Armor | `cloud_armor` | ✅ |

### AI & Machine Learning (5 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Vertex AI | `cloud_machine_learning` | ✅ |
| AI Platform | `cloud_machine_learning` | ✅ |
| Vision AI | `cloud_vision_api` | ✅ |
| Natural Language AI | `cloud_natural_language_api` | ✅ |
| Speech-to-Text | `cloud_speech_api` | ✅ |

### Integration & Messaging (3 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Pub/Sub | `cloud_pubsub` | ✅ |
| Cloud Tasks | `cloud_tasks` | ✅ |
| Cloud Scheduler | `cloud_scheduler` | ✅ |

### Operations & Monitoring (4 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Cloud Logging | `logging` | ✅ |
| Cloud Monitoring | `cloud_monitoring` | ✅ |
| Cloud Trace | `trace` | ✅ |
| Error Reporting | `error_reporting` | ✅ |

### API Management (2 services)
| Service | Shape Name | Status |
|---------|------------|--------|
| Apigee | `apigee_api_platform` | ✅ |
| API Gateway | `gateway` | ✅ |

---

## Services Without Exact Matches (2 services)

These services don't have dedicated shapes in DrawIO's gcp2 library because they were released after the library was created.

### Workflows
- **Shape Name:** `workflows`
- **Issue:** Shape doesn't exist in gcp2 library
- **Current Behavior:** Will render as empty box in DrawIO
- **Recommended Fallback:** `cloud_scheduler` (similar orchestration concept)
- **Alternative:** Leave as-is and wait for DrawIO library update

### Eventarc
- **Shape Name:** `eventarc`
- **Issue:** Shape doesn't exist in gcp2 library
- **Current Behavior:** Will render as empty box in DrawIO
- **Recommended Fallback:** `cloud_pubsub` (similar event routing concept)
- **Alternative:** Leave as-is and wait for DrawIO library update

---

## Shape Name Corrections Applied

The following shape names were corrected during validation (old → new):

### Missing "Cloud" Prefix
| Service | Old Name | Corrected Name |
|---------|----------|----------------|
| Memorystore | `memorystore` | `cloud_memorystore` |
| Filestore | `filestore` | `cloud_filestore` |
| Pub/Sub | `pubsub` | `cloud_pubsub` |

### API Suffix Required
| Service | Old Name | Corrected Name |
|---------|----------|----------------|
| Vision AI | `vision_api` | `cloud_vision_api` |
| Natural Language AI | `natural_language_api` | `cloud_natural_language_api` |
| Speech-to-Text | `speech_to_text` | `cloud_speech_api` |

### Simplified Names
| Service | Old Name | Corrected Name |
|---------|----------|----------------|
| Cloud Logging | `cloud_logging` | `logging` |
| Cloud Trace | `cloud_trace` | `trace` |
| API Gateway | `api_gateway` | `gateway` |

### Platform Names
| Service | Old Name | Corrected Name |
|---------|----------|----------------|
| Apigee | `apigee` | `apigee_api_platform` |
| AI Platform | `ai_platform` | `cloud_machine_learning` |
| Vertex AI | `vertexai` | `cloud_machine_learning` |
| Google Kubernetes Engine | `google_kubernetes_engine` | `container_engine` |

---

## Font Color Standardization

All service icons have been updated with proper font colors:

- **Old Color:** `#999999` (too light, poor readability)
- **New Color:** `#424242` (proper dark gray, matches Google Cloud design guidelines)

This improves label readability when diagrams are exported to PNG or PDF.

---

## Validation Process

The icon validation process involved:

1. **Extraction:** Used `scripts/extract-shape-names.py` to extract all 297 available shapes from DrawIO's gcp2.xml stencil file
2. **Validation:** Used `scripts/validate-gcp-icons.py` to compare gcp-icons.json against available shapes
3. **Fixing:** Used `scripts/fix-gcp-icons.py` to automatically correct shape names and font colors
4. **Verification:** Manually tested in DrawIO Desktop to confirm proper rendering

---

## Common Naming Patterns

Understanding these patterns helps when adding new services:

### Pattern 1: "Cloud" Prefix
Many GCP services require the "cloud_" prefix in their shape name:
- `cloud_run`, `cloud_storage`, `cloud_sql`, `cloud_functions`
- But not: `logging`, `trace`, `gateway`, `bigquery`

### Pattern 2: API Services
API services typically include "_api" suffix:
- `cloud_vision_api`, `cloud_natural_language_api`, `cloud_speech_api`

### Pattern 3: Title Case → Snake Case
- DrawIO XML uses Title Case: "Cloud Run"
- Shape references use snake_case: `cloud_run`
- Conversion is automatic in DrawIO

### Pattern 4: Legacy Names
Some services use older or simplified names in DrawIO:
- Google Kubernetes Engine → `container_engine` (not `google_kubernetes_engine`)
- Cloud Logging → `logging` (not `cloud_logging`)

---

## Using This Information

### For Diagram Generation
When generating new diagrams, the system automatically uses the corrected shape names from [gcp-icons.json](gcp-icons.json). All 35 services are available, with Workflows and Eventarc using their original (non-rendering) shape names.

### For Manual Editing
When manually editing DrawIO files, use the shape names from the "Validated & Working Icons" section above. The full shape reference format is:

```
shape=mxgraph.gcp2.{shape_name}
```

Example:
```xml
shape=mxgraph.gcp2.cloud_run
shape=mxgraph.gcp2.bigquery
shape=mxgraph.gcp2.container_engine
```

### For Troubleshooting
If an icon doesn't render in DrawIO Desktop:
1. Check that the shape name matches this compatibility reference
2. Verify you're using `mxgraph.gcp2.` prefix (not `mxgraph.gcp.`)
3. Ensure no typos in the shape name (e.g., `cloud_pubsub` not `pubsub`)
4. For Workflows/Eventarc, consider using fallback icons

---

## Future Improvements

### Adding New Services
When adding new GCP services to gcp-icons.json:
1. Run `scripts/validate-gcp-icons.py` to check if the shape exists
2. Use the naming patterns above to guess the correct shape name
3. Test in DrawIO Desktop before committing

### DrawIO Library Updates
Monitor DrawIO releases for updates to the gcp2 library that might include:
- Workflows icon
- Eventarc icon
- Other newer GCP services

To check for new shapes after a DrawIO update:
```bash
python scripts/extract-shape-names.py /path/to/new/gcp2.xml > gcp2-shapes-new.txt
diff .archive/gcp2-available-shapes.txt gcp2-shapes-new.txt
```

---

## Related Files

- [gcp-icons.json](gcp-icons.json) - Main icon database (automatically uses corrected names)
- [.archive/ICON-FIX-SUMMARY.md](../.archive/ICON-FIX-SUMMARY.md) - Detailed summary of fixes applied
- [.archive/ICON-VALIDATION-REPORT.md](../.archive/ICON-VALIDATION-REPORT.md) - Original validation findings
- [.archive/gcp2-available-shapes.txt](../.archive/gcp2-available-shapes.txt) - Complete list of 297 available shapes

---

## Validation Scripts

| Script | Purpose |
|--------|---------|
| `validate-gcp-icons.py` | Validate icon compatibility against DrawIO library |
| `fix-gcp-icons.py` | Auto-fix shape names and font colors in gcp-icons.json |
| `fix-drawio-icons.py` | Bulk fix icon references in existing .drawio files |
| `extract-shape-names.py` | Extract available shapes from DrawIO stencil XML |

Run validation anytime:
```bash
python scripts/validate-gcp-icons.py
```

---

**Note:** This compatibility reference is based on DrawIO Desktop as of January 2026. Future versions of DrawIO may include additional GCP icons.
