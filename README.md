# DrawIO Architect

> **Migrated to Provider-Specific Skills (v2.0)**
>
> This project has been split into two focused skills for better clarity and usability:
> - **[generating-gcp-diagrams](/.claude/skills/generating-gcp-diagrams/)** - For Google Cloud Platform
> - **[generating-aws-diagrams](/.claude/skills/generating-aws-diagrams/)** - For Amazon Web Services
>
> The unified `drawio-architect` skill (v1.4) is now deprecated. See [Migration Guide](#migration-guide) below.

An Agent Skill for creating cloud architecture diagrams as DrawIO XML. Supports **GCP** and **AWS**. Designed for AI agents (compliant with the [Agent Skills](https://agentskills.io) specification).

## Features

- Generate DrawIO XML from text descriptions
- Analyze existing .drawio files to extract shapes and connections
- **GCP**: 35 service icons (94.3% validated against DrawIO's gcp2 library)
- **AWS**: 112 service icons across 10 categories (Part 1 of 2, mxgraph.aws4 library)
- Container/group patterns for both providers (VPC-SC, AWS VPC, subnets, regions, etc.)
- Provider-specific connection styles and best practices built-in
- Validation scripts for generated XML and icon compatibility

## Migration Guide

### Why the Split?

The unified `drawio-architect` skill has been split into provider-specific skills to:
- **Improve focus**: Each skill contains only relevant provider content
- **Reduce size**: SKILL.md files are now ~430-450 lines (vs. 491 unified)
- **Enable independent versioning**: GCP and AWS skills can evolve separately
- **Clearer invocation**: "Generate GCP diagram" vs. "Generate AWS diagram"
- **Better maintenance**: Provider-specific updates don't affect the other skill

### Changes

**Before (v1.4):**
- Single unified skill: `drawio-architect`
- Both GCP and AWS in one SKILL.md (491 lines)
- Provider detection required at runtime

**After (v2.0):**
- Two independent skills:
  - `generating-gcp-diagrams` (431 lines) - GCP only
  - `generating-aws-diagrams` (448 lines) - AWS only
- No provider detection needed
- Each skill is self-contained with all necessary files

### What's Included in Each Skill

Both skills include:
- Complete SKILL.md with provider-specific instructions
- Provider-specific icon database (gcp-icons.json or aws-icons.json)
- Provider-specific containers (containers.json or aws-containers.json)
- All templates (drawio-base.xml, node-template.xml, connection-template.xml)
- All scripts (validation, analysis, export)
- Reference documentation customized per provider

### Key Differences

| Aspect | GCP Skill | AWS Skill |
|--------|-----------|-----------|
| **Name** | generating-gcp-diagrams | generating-aws-diagrams |
| **Stencil** | mxgraph.gcp2 | mxgraph.aws4 |
| **Naming** | snake_case (`cloud_run`) | Space-separated (`step functions`) |
| **Icon Size** | 50x50 | 64x64 |
| **Font Color** | #424242 | #232F3E |
| **Services** | 35 (8 categories) | 112 (10 categories, Part 1 of 2) |
| **Validation** | 94.3% (33 exact, 2 fallback) | 100% (112 exact) |

### Installation

The new skills are located in:
- `.claude/skills/generating-gcp-diagrams/`
- `.claude/skills/generating-aws-diagrams/`

Both skills are automatically discovered if the `.claude/skills/` directory is in your project.

### For Multi-Cloud Users

If you work with both GCP and AWS:
- You can install both skills simultaneously
- Each skill operates independently
- No conflicts between the two skills

---

## Roadmap

Currently supports **GCP** and **AWS** (Part 1). Future releases will expand to include:

- [x] GCP architecture diagrams (mxgraph.gcp2.*)
- [ ] AWS Part 2 (~16 remaining categories: Databases, Developer Tools, Networking, Security, Storage, etc.)
- [ ] Azure architecture diagrams (mxgraph.azure.*)
- [ ] Multi-cloud and hybrid diagrams
- [ ] Kubernetes/container diagrams

## Project Structure

```
drawio-architect/
├── SKILL.md              # Agent skill instructions
├── assets/
│   ├── gcp-icons.json    # GCP service icon database (35 services)
│   ├── aws-icons.json    # AWS service icon database (112 services, Part 1)
│   ├── containers.json   # GCP container and connection styles
│   ├── aws-containers.json # AWS group/container styles
│   ├── templates/        # XML templates for shapes & connections
│   └── examples/         # Sample diagrams
├── references/           # Detailed documentation
├── scripts/              # Validation & export tools
└── output/               # Generated diagrams
```

## Scripts

| Script | Purpose |
|--------|---------|
| `validate-drawio.py` | Validate .drawio XML structure |
| `validate-gcp-icons.py` | Validate GCP icon compatibility with DrawIO gcp2 library |
| `validate-aws-icons.py` | Validate AWS icon compatibility with DrawIO aws4 library |
| `fix-gcp-icons.py` | Automatically fix icon shape names in gcp-icons.json |
| `fix-aws-icons.py` | Automatically fix icon shape names in aws-icons.json |
| `fix-drawio-icons.py` | Bulk fix icon references in existing .drawio files |
| `analyze-existing.py` | Extract shapes/connections from existing files |
| `extract-shape-names.py` | Extract available shape names from DrawIO stencil XML |
| `export-diagram.sh` | Export to PNG/PDF (requires DrawIO Desktop) |
| `open-diagram.sh` | Open .drawio file in DrawIO Desktop |

## Requirements

- Python 3 (for scripts)
- DrawIO Desktop (optional, for export/preview) - `brew install drawio`

## Usage

This is an Agent Skill designed for use with AI agents. See [SKILL.md](SKILL.md) for full instructions on:

- Analyzing existing DrawIO files
- Creating diagrams from text descriptions
- XML structure and styling best practices

## Icon Compatibility

### GCP
35 services with **94.3% validation success** against DrawIO's built-in gcp2 library:
- ✅ 33/35 icons render correctly with exact shape matches
- ⚠️ 2/35 services (Workflows, Eventarc) use fallback icons (newer services not in gcp2 library)

### AWS (Part 1)
112 services across 10 categories from the official AWS Architecture Icons (Release 23-2026.01.30):
- Analytics, Application Integration, AI/ML, Blockchain, Business Applications
- Cloud Financial Management, Compute, Containers, Customer Enablement, Customer Experience
- Shape names derived from DrawIO's aws4 library conventions — run `validate-aws-icons.py` to check

All icon shape names and font colors have been validated and corrected to ensure proper rendering in DrawIO Desktop.

## License

MIT
