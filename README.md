# DrawIO Architect

An Agent Skill for creating cloud architecture diagrams as DrawIO XML. Supports **GCP** and **AWS** via provider-specific skills. Designed for AI agents (compliant with the [Agent Skills](https://agentskills.io) specification).

## Skills

| Skill | Provider | Services | Validation |
|-------|----------|----------|------------|
| `generating-gcp-diagrams` | Google Cloud Platform | 35 (8 categories) | 94.3% |
| `generating-aws-diagrams` | Amazon Web Services | 112 (10 categories) | 100% |

Each skill is fully self-contained with its own SKILL.md, icon database, templates, scripts, and references.

## Project Structure

```
drawio-architect/
├── .claude/skills/
│   ├── generating-gcp-diagrams/   # GCP skill (self-contained)
│   │   ├── SKILL.md               # Skill instructions
│   │   ├── README.md              # Skill documentation
│   │   ├── assets/                # gcp-icons.json, containers.json, templates/
│   │   ├── references/            # Style guide, coordinate system, XML docs
│   │   └── scripts/               # validate, fix, export, analyze
│   └── generating-aws-diagrams/   # AWS skill (self-contained)
│       ├── SKILL.md               # Skill instructions
│       ├── README.md              # Skill documentation
│       ├── assets/                # aws-icons.json, aws-containers.json, templates/
│       ├── references/            # Style guide, coordinate system, XML docs
│       └── scripts/               # validate, fix, export, analyze
├── .archive/                      # Deprecated files (gitignored)
├── .gitignore
└── README.md
```

## Usage

Both skills are automatically discovered when this repo is cloned and used with a Claude Code-compatible agent. Invoke with:

- **GCP**: Ask the agent to generate a GCP architecture diagram
- **AWS**: Ask the agent to generate an AWS architecture diagram

## Requirements

- Python 3 (for validation/fix scripts)
- DrawIO Desktop (optional, for export/preview) — `brew install drawio`

## Roadmap

- [x] GCP architecture diagrams (mxgraph.gcp2)
- [x] AWS Part 1: 112 services across 10 categories (mxgraph.aws4)
- [ ] AWS Part 2: ~16 remaining categories (Databases, Networking, Security, Storage, etc.)
- [ ] Azure architecture diagrams
- [ ] Multi-cloud and hybrid diagrams

## License

MIT
