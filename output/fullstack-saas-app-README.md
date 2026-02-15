# Fullstack SaaS Application Architecture

**Created:** 2026-01-31
**Type:** Modern serverless fullstack application
**File:** [fullstack-saas-app.drawio](fullstack-saas-app.drawio)

---

## Overview

This architecture demonstrates a **production-ready collaborative document editing platform** (CollabDocs) built entirely on serverless GCP services. It showcases a modern React SPA frontend with a fully serverless backend, real-time collaboration, AI features, and comprehensive CI/CD.

### Application Features

- **Real-time collaboration** - Multiple users editing documents simultaneously
- **Smart suggestions** - AI-powered content recommendations
- **Document intelligence** - Sentiment analysis and OCR for scanned documents
- **Secure authentication** - JWT-based auth with session management
- **File management** - Upload, store, and export documents
- **Webhook integrations** - Third-party app notifications
- **Automated backups** - Scheduled daily exports

---

## Architecture Components

### 🎨 Frontend Infrastructure (Orange Zone)
- **Cloud Storage** - Static hosting for React SPA (HTML, CSS, JS bundles)
- **Cloud CDN** - Global edge caching for low-latency worldwide
- **Cloud Load Balancing** - HTTPS termination, SSL, and routing

**Stack:** React 18, TypeScript, Vite, TailwindCSS

### ⚡ Serverless API Layer (VPC - Dashed)
- **API Gateway** - Unified entry point, rate limiting, API versioning
- **Cloud Run (Auth Service)** - JWT authentication, user management
- **Cloud Run (Docs API)** - Document CRUD, collaboration endpoints
- **Cloud Functions (Webhooks)** - Event-driven integrations
- **Cloud Functions (Export Jobs)** - Background PDF/Excel export generation

**Stack:** Node.js/TypeScript, Express on Cloud Run, Functions for event handlers

### 💾 Data & Storage Layer (VPC - Dashed)
- **Firestore** - NoSQL for documents with real-time sync
- **Cloud SQL (PostgreSQL)** - Relational database for users, auth, billing
- **Cloud Storage** - User-uploaded files (images, attachments)
- **Memorystore (Redis)** - Session cache, rate limiting, temporary locks

**Data Strategy:**
- **Firestore:** Document content, collaborative edits (real-time)
- **Cloud SQL:** User profiles, subscriptions, permissions
- **Redis:** Session tokens, API rate limits, document locks

### 🔄 Real-time & Events Layer (VPC - Dashed)
- **Pub/Sub** - Event streaming for document changes, notifications
- **Cloud Tasks** - Asynchronous job queue (exports, email sends)
- **Cloud Scheduler** - Cron jobs for daily backups and cleanup

**Event Flow:**
1. Document edited → Pub/Sub event
2. Pub/Sub → Webhook function → External apps notified
3. User requests export → Cloud Tasks → Export function → PDF generated

### 🤖 AI & Intelligence Layer (VPC - Dashed)
- **Vertex AI** - Custom ML model for smart content suggestions
- **Natural Language API** - Sentiment analysis, entity extraction
- **Vision API** - OCR for scanned document uploads

**AI Use Cases:**
- Auto-complete sentences while typing
- Detect document tone (professional, casual, urgent)
- Extract text from images/PDFs

### 🏗️ CI/CD Pipeline (Purple Zone)
- **Cloud Build** - Automated builds on git push
- **Artifact Registry** - Container image storage for Cloud Run services

**Pipeline:**
```
git push → Cloud Build →
  ├─ Build React app → Deploy to Cloud Storage
  ├─ Build Docker images → Push to Artifact Registry
  └─ Deploy to Cloud Run (zero-downtime)
```

### 📊 Observability & Monitoring (Green Zone)
- **Cloud Monitoring** - Custom metrics, SLO tracking, dashboards
- **Cloud Logging** - Centralized logs from all services
- **Error Reporting** - Exception tracking, stack traces, alerting

---

## Data Flow Diagrams

### Primary User Flow (Frontend → Backend)
1. **User** → Load Balancer (HTTPS)
2. Load Balancer → **CDN** (cache static assets)
3. CDN → **Cloud Storage** (serve React SPA)
4. React app → Load Balancer → **API Gateway** (API calls)
5. API Gateway → **Cloud Run (Auth)** (authenticate)
6. API Gateway → **Cloud Run (Docs)** (CRUD operations)

### Real-time Collaboration Flow
7. Cloud Run (Docs) ↔ **Firestore** (real-time sync)
8. Firestore → Multiple browsers (via Firebase SDK)
9. Cloud Run (Docs) → **Pub/Sub** (publish change events)
10. Pub/Sub → **Webhooks Function** (notify integrations)

### Data Persistence Flow
11. Cloud Run (Auth) → **Cloud SQL** (store user data)
12. Cloud Run (Auth) ↔ **Memorystore** (cache sessions)
13. Cloud Run (Docs) → **Cloud Storage** (upload files)

### Background Jobs Flow
14. Cloud Run (Docs) → **Cloud Tasks** (enqueue export job)
15. Cloud Tasks → **Export Function** (trigger)
16. **Cloud Scheduler** → Export Function (daily backups)

### AI Enhancement Flow
17. Cloud Run (Docs) → **Vertex AI** (get smart suggestions)
18. Cloud Run (Docs) → **Natural Language API** (analyze sentiment)
19. Export Function → **Vision API** (OCR scanned docs)

### CI/CD Flow
20. Git push → **Cloud Build** (trigger)
21. Cloud Build → **Artifact Registry** (push images)
22. Cloud Build → **Cloud Storage** (deploy React app)
23. Artifact Registry → **Cloud Run** (auto-deploy)

### Observability Flow
24. All services → **Cloud Monitoring** (metrics)
25. All services → **Cloud Logging** (logs)
26. All services → **Error Reporting** (errors)

---

## Technical Highlights

### Serverless-First Architecture
✅ **Zero server management** - All compute is Cloud Run or Cloud Functions
✅ **Auto-scaling** - Scales to zero, handles millions of requests
✅ **Pay-per-use** - Only charged for actual usage

### Real-time Capabilities
✅ **Firestore real-time sync** - See collaborators' changes instantly
✅ **Pub/Sub event streaming** - Decouple services, enable integrations
✅ **WebSocket alternative** - Firestore SDK handles real-time without custom WebSockets

### AI-Powered Features
✅ **Vertex AI** - Custom models for domain-specific suggestions
✅ **Pre-trained APIs** - Vision, Natural Language without training
✅ **Progressive enhancement** - AI features don't block core functionality

### Security & Performance
✅ **API Gateway** - Rate limiting, authentication, API versioning
✅ **Memorystore (Redis)** - Session caching, reduces DB load
✅ **Cloud Armor** (can be added) - DDoS protection, WAF rules
✅ **Cloud CDN** - Global edge caching, 99.9% uptime SLA

### Developer Experience
✅ **Cloud Build** - Automated CI/CD from git
✅ **Artifact Registry** - Private container registry
✅ **Observability** - Logs, metrics, errors in one place
✅ **Infrastructure as Code** (can add Terraform) - Reproducible deployments

---

## Container Organization

**9 logical containers** demonstrating hierarchy:

1. **GCP Project** (gray) - Outermost boundary
2. **Frontend Infrastructure** (orange) - CDN, storage, load balancer
3. **VPC - Application Network** (blue dashed) - Backend services
4. **Serverless API Layer** (nested in VPC)
5. **Data & Storage Layer** (nested in VPC)
6. **Real-time & Events** (nested in VPC)
7. **AI & Intelligence** (nested in VPC)
8. **CI/CD Pipeline** (purple) - Build and deployment
9. **Observability** (green) - Monitoring stack

---

## Connection Styles Demonstrated

### Thick Blue Lines (strokeWidth=2)
- **User → Load Balancer:** Primary traffic flow
- **Load Balancer → API Gateway:** API request path

### Standard Solid Lines (strokeWidth=1)
- Service-to-service communication
- Database queries
- File operations

### Dashed Lines
- **Gray dashed:** Async operations (cache, background jobs)
- **Green dashed:** Observability (metrics, logs)
- **Purple dashed:** Deployment flows (CI/CD)
- **Orange dashed:** Scheduled jobs

### Color-Coded Flows
- **Blue:** User and API traffic
- **Green:** Observability signals
- **Purple:** CI/CD deployments
- **Orange:** Cron/scheduled operations
- **Red:** Error reporting

---

## Services Used (25 total)

### Compute & Hosting
- Cloud Storage (3 instances) - React SPA, user files, container registry
- Cloud Run (2 services) - Auth API, Docs API
- Cloud Functions (2 functions) - Webhooks, export jobs
- Cloud CDN - Edge caching
- Cloud Load Balancing - HTTPS routing

### Data & Databases
- Firestore - Real-time document storage
- Cloud SQL - Relational user data
- Memorystore - Redis cache

### Integration & Events
- API Gateway - API management
- Pub/Sub - Event streaming
- Cloud Tasks - Task queue
- Cloud Scheduler - Cron jobs

### AI & ML
- Vertex AI - Smart suggestions
- Natural Language API - Sentiment analysis
- Vision API - OCR

### CI/CD & Tools
- Cloud Build - CI/CD automation
- Artifact Registry - Container images

### Observability
- Cloud Monitoring - Metrics
- Cloud Logging - Logs
- Error Reporting - Errors

---

## Real-World Production Additions

**To make this production-ready, add:**

### Security
- **Cloud Armor** - WAF, DDoS protection on Load Balancer
- **Secret Manager** - API keys, DB passwords
- **VPC Service Controls** - Data exfiltration protection
- **Cloud KMS** - Encryption key management

### Reliability
- **Multi-region deployment** - Disaster recovery
- **Cloud Spanner** - Replace Cloud SQL for global scale
- **Backup strategy** - Automated Firestore/SQL backups

### Compliance
- **Data Loss Prevention API** - Scan for PII
- **Audit logging** - Compliance trail
- **Cloud IAM** - Fine-grained permissions

### Cost Optimization
- **Cloud Billing budgets** - Spending alerts
- **Committed use discounts** - For predictable load
- **Resource labels** - Track costs by team/feature

---

## Skill Validation

This diagram tests:

✅ **25 GCP services** with validated icons
✅ **9 nested containers** showing clear hierarchy
✅ **24 connections** with varied styles and colors
✅ **Multiple line widths** (1pt standard, 2pt primary flows)
✅ **Color-coded zones** (orange frontend, purple CI/CD, green observability)
✅ **Dashed vs solid** connections (async vs sync)
✅ **Real-world patterns** (serverless, event-driven, AI integration)
✅ **Bidirectional flows** (Firestore real-time sync)
✅ **External actors** (web users)
✅ **CI/CD integration** (build → deploy flow)

---

## Cost Estimate (Approximate Monthly)

**Assuming:**
- 50,000 monthly active users
- 500K API requests/day
- 100GB Firestore storage
- 50GB Cloud SQL

| Service | Estimated Cost |
|---------|----------------|
| Cloud Run (2 services) | $50-100 |
| Cloud Functions | $20-40 |
| Firestore | $100-200 |
| Cloud SQL | $100-150 |
| Memorystore (1GB) | $50 |
| Cloud Storage + CDN | $50-100 |
| Load Balancing | $20 |
| AI APIs (usage-based) | $50-200 |
| Monitoring & Logs | $50 |
| **Total** | **~$490-$910/month** |

**Scaling:** Serverless = costs scale with usage. Low traffic = near $0.

---

## Tech Stack Summary

**Frontend:**
- React 18, TypeScript, Vite
- Firestore SDK (real-time)
- TailwindCSS, shadcn/ui

**Backend:**
- Node.js 20, TypeScript
- Express on Cloud Run
- Firestore Admin SDK
- PostgreSQL (Cloud SQL)
- Redis (Memorystore)

**DevOps:**
- Docker, Cloud Build
- Terraform (infrastructure as code)
- Git-based deployments

---

## Open in DrawIO

```bash
# View the diagram
./scripts/open-diagram.sh output/fullstack-saas-app.drawio

# Export to PNG
./scripts/export-diagram.sh output/fullstack-saas-app.drawio png

# Analyze structure
python scripts/analyze-existing.py output/fullstack-saas-app.drawio --markdown
```

---

**Generated by:** DrawIO Architect Skill
**Validation:** ✅ All icons validated, professional layout
**Ready for:** Investor pitches, architecture reviews, team onboarding
