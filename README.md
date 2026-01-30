# DrawIO Architect

> **Work in Progress** - This project is under active development and subject to changes.

An Agent Skill for creating GCP architecture diagrams as DrawIO XML. Designed for AI agents (compliant with the [Agent Skills](https://agentskills.io) specification).

## Features

- Generate DrawIO XML from text descriptions
- Analyze existing .drawio files to extract shapes and connections
- 35+ GCP service icons supported
- Container patterns (VPC-SC, Project zones, logical groups)
- Connection styles and best practices built-in
- Validation scripts for generated XML

## Roadmap

Currently supports **GCP services only**. Future releases will expand to include:

- [ ] AWS architecture diagrams (mxgraph.aws4.*)
- [ ] Azure architecture diagrams (mxgraph.azure.*)
- [ ] Multi-cloud and hybrid diagrams
- [ ] Kubernetes/container diagrams

## Project Structure

```
drawio-architect/
├── SKILL.md              # Agent skill instructions
├── assets/
│   ├── gcp-icons.json    # GCP service icon database (35 services)
│   ├── containers.json   # Container and connection styles
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
| `analyze-existing.py` | Extract shapes/connections from existing files |
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

## License

MIT
