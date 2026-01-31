# GCP Icon Validation Report

**Date:** 2026-01-31
**Status:** 15 of 35 icons need fixes (57.1% success rate)

---

## Summary

| Status | Count | Percentage |
|--------|-------|------------|
| ✅ Valid | 20 | 57.1% |
| ❌ Invalid/Missing | 15 | 42.9% |
| **Total** | **35** | **100%** |

---

## ✅ Valid Icons (20)

These icons render correctly:

| Service | Shape Name | Actual Shape in DrawIO |
|---------|------------|------------------------|
| Cloud Run | `cloud_run` | Cloud Run |
| Compute Engine | `compute_engine` | Compute Engine |
| Cloud Functions | `cloud_functions` | Cloud Functions |
| App Engine | `app_engine` | App Engine |
| BigQuery | `bigquery` | BigQuery |
| Cloud SQL | `cloud_sql` | Cloud SQL |
| Cloud Firestore | `cloud_firestore` | cloud firestore |
| Cloud Spanner | `cloud_spanner` | Cloud Spanner |
| Cloud Bigtable | `cloud_bigtable` | Cloud Bigtable |
| Cloud Storage | `cloud_storage` | Cloud Storage |
| Persistent Disk | `persistent_disk` | Persistent Disk |
| Virtual Private Cloud | `virtual_private_cloud` | Virtual Private Cloud |
| Cloud Load Balancing | `cloud_load_balancing` | Cloud Load Balancing |
| Cloud CDN | `cloud_cdn` | Cloud CDN |
| Cloud DNS | `cloud_dns` | Cloud DNS |
| Cloud Armor | `cloud_armor` | Cloud Armor |
| Cloud Tasks | `cloud_tasks` | Cloud Tasks |
| Cloud Scheduler | `cloud_scheduler` | Cloud Scheduler |
| Cloud Monitoring | `cloud_monitoring` | cloud monitoring |
| Error Reporting | `error_reporting` | Error Reporting |

---

## ❌ Icons That Need Fixing (15)

### 1. Google Kubernetes Engine
- **Current:** `google_kubernetes_engine`
- **Issue:** Shape doesn't exist
- **Available alternatives:**
  - `kubernetes_name` (Kubernetes Name)
  - `kubernetes_logo` (Kubernetes Logo)
- **Recommended fix:** Use `container_engine` (Container Engine) which is the GCP-branded GKE icon
- **Note:** Check if "GKE on Prem" shape exists

### 2. Memorystore
- **Current:** `memorystore`
- **Issue:** Missing "Cloud" prefix
- **Fix:** Change to `cloud_memorystore`
- **Actual shape:** Cloud Memorystore

### 3. Filestore
- **Current:** `filestore`
- **Issue:** Missing "Cloud" prefix
- **Fix:** Change to `cloud_filestore`
- **Actual shape:** Cloud Filestore

### 4. Vertex AI
- **Current:** `vertexai`
- **Issue:** Shape name doesn't exist
- **Available alternatives:** None found
- **Investigation needed:** Check if there's a different name or if this is a newer service not yet in gcp2.xml
- **Fallback:** Use `cloud_machine_learning` or `cloud_automl`

### 5. AI Platform
- **Current:** `ai_platform`
- **Issue:** Shape doesn't exist
- **Fix:** Change to `cloud_machine_learning`
- **Actual shape:** Cloud Machine Learning

### 6. Vision AI
- **Current:** `vision_api`
- **Issue:** Missing "Cloud" prefix
- **Fix:** Change to `cloud_vision_api`
- **Actual shape:** Cloud Vision API

### 7. Natural Language AI
- **Current:** `natural_language_api`
- **Issue:** Missing "Cloud" prefix
- **Fix:** Change to `cloud_natural_language_api`
- **Actual shape:** Cloud Natural Language API

### 8. Speech-to-Text
- **Current:** `speech_to_text`
- **Issue:** Shape doesn't exist
- **Available in gcp2.xml:** "Cloud Speech API", "Cloud Text to Speech"
- **Fix:** Change to `cloud_speech_api`
- **Actual shape:** Cloud Speech API

### 9. Pub/Sub
- **Current:** `pubsub`
- **Issue:** Shape doesn't exist as single word
- **Available in gcp2.xml:** "Cloud PubSub" (note: capital S)
- **Fix:** Change to `cloud_pubsub`
- **Actual shape:** Cloud PubSub

### 10. Workflows
- **Current:** `workflows`
- **Issue:** Shape doesn't exist
- **Investigation needed:** Check if this is a newer service or uses a different icon
- **Fallback:** Use generic `process` or `arrows_system` icon

### 11. Eventarc
- **Current:** `eventarc`
- **Issue:** Shape doesn't exist
- **Investigation needed:** This is a newer GCP service, may not be in gcp2 library yet
- **Fallback:** Use generic event/routing icon

### 12. Cloud Logging
- **Current:** `cloud_logging`
- **Issue:** Shape is just "Logging" without "Cloud" prefix
- **Fix:** Change to `logging`
- **Actual shape:** Logging

### 13. Cloud Trace
- **Current:** `cloud_trace`
- **Issue:** Shape is just "Trace" without "Cloud" prefix
- **Fix:** Change to `trace`
- **Actual shape:** Trace

### 14. Apigee
- **Current:** `apigee`
- **Issue:** Generic name doesn't exist
- **Available alternatives:**
  - `apigee_api_platform` (Apigee API Platform)
  - `apigee_sense` (Apigee Sense)
- **Fix:** Change to `apigee_api_platform`
- **Actual shape:** Apigee API Platform

### 15. API Gateway
- **Current:** `api_gateway`
- **Issue:** Shape doesn't exist with this exact name
- **Available alternatives:**
  - `gateway` (Gateway)
  - `gateway_icon` (Gateway icon)
- **Fix:** Change to `gateway`
- **Actual shape:** Gateway
- **Note:** May need to verify this is the correct icon visually

---

## Recommended Fixes

### High Confidence Fixes (11)

These have exact matches in gcp2.xml:

```json
{
  "memorystore": "cloud_memorystore",
  "filestore": "cloud_filestore",
  "vision_api": "cloud_vision_api",
  "natural_language_api": "cloud_natural_language_api",
  "speech_to_text": "cloud_speech_api",
  "pubsub": "cloud_pubsub",
  "cloud_logging": "logging",
  "cloud_trace": "trace",
  "ai_platform": "cloud_machine_learning",
  "apigee": "apigee_api_platform",
  "api_gateway": "gateway"
}
```

### Medium Confidence (2)

These need visual verification:

- **Google Kubernetes Engine:** Change `google_kubernetes_engine` → `container_engine` (verify icon)
- **Vertex AI:** No exact match found. Options:
  - Use `cloud_machine_learning` (same as AI Platform)
  - Use `cloud_automl`
  - Leave as generic or remove until confirmed

### Low Confidence (2)

These services may not have dedicated icons in gcp2 library:

- **Workflows:** No match found. Use generic `process` icon or investigate further
- **Eventarc:** No match found. Use generic event routing icon or investigate further

---

## Action Items

### Immediate (Phase 1)
1. Update gcp-icons.json with the 11 high-confidence fixes
2. Test each updated icon in DrawIO Desktop to verify rendering
3. Update the metadata count if services are removed

### Verification (Phase 2)
4. Visual verification for GKE, Vertex AI
5. Research Workflows and Eventarc icon availability
6. Consider adding fallback icons for services without dedicated shapes

### Documentation (Phase 3)
7. Create ICON-COMPATIBILITY.md documenting known issues
8. Update SKILL.md with any service name changes
9. Add notes about which services use generic/fallback icons

---

## Pattern Analysis

### Common Issues Found:

1. **Missing "Cloud" prefix:** Several services (Memorystore, Filestore, Vision API, etc.)
2. **Different capitalization:** "PubSub" vs "Pub/Sub", "API" vs "Api"
3. **Service evolution:** Newer services (Workflows, Eventarc, Vertex AI) may not be in older gcp2 library
4. **Abbreviated names:** "Logging" vs "Cloud Logging", "Trace" vs "Cloud Trace"

### Naming Convention Observed:

- Core infrastructure: Includes "Cloud" prefix (Cloud Storage, Cloud SQL)
- APIs: Uses "Cloud X API" format (Cloud Vision API, Cloud Natural Language API)
- Legacy services: May omit "Cloud" (Logging, Trace)
- Third-party: Uses brand name (Apigee API Platform)

---

## Next Steps

1. **Run fix script:**
   ```bash
   python scripts/fix-gcp-icons.py --apply
   ```

2. **Validate changes:**
   ```bash
   python scripts/validate-gcp-icons.py
   ```

3. **Test in DrawIO:**
   - Generate a test diagram with all 35 services
   - Open in DrawIO Desktop
   - Verify all icons render correctly
   - Document any remaining issues

4. **Update documentation:**
   - Update SKILL.md service list
   - Create ICON-COMPATIBILITY.md
   - Note any fallback icons used
