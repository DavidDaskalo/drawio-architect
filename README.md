# DrawIO Architect

Agent Skills for generating cloud architecture diagrams as DrawIO XML. Supports **AWS** and **GCP** via provider-specific skills.

Built for AI coding agents. Compatible with the [Agent Skills](https://agentskills.io) open specification.

## Available Skills

| Skill | Provider | Services | Icon Validation |
|-------|----------|----------|-----------------|
| [`generating-aws-diagrams`](skills/generating-aws-diagrams/) | Amazon Web Services | 264 across 24 categories | 96.6% (255/264) |
| [`generating-gcp-diagrams`](skills/generating-gcp-diagrams/) | Google Cloud Platform | 46 across 11 categories | 87% (40/46) |

Each skill is fully self-contained with its own SKILL.md, icon database, templates, and reference docs.

---

## Install: generating-aws-diagrams

**GitHub:** `https://github.com/DavidDaskalo/drawio-architect`

### 1. Choose Installation Scope

- **Project-level** — skill is available only in the current project
- **Global** — skill is available across all projects for your agent

### 2. Determine Your Install Path

**Mac / Linux:**

| Agent | Project Path | Global Path |
|-------|-------------|-------------|
| Claude | `.claude/skills/` | `~/.claude/skills/` |
| Cursor | `.cursor/skills/` | `~/.cursor/skills/` |
| Codex | `.codex/skills/` | `~/.codex/skills/` |
| Antigravity | `.agent/skills/` | `~/.gemini/antigravity/global_skills/` |
| Windsurf | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |
| Cline | `.cline/skills/` | `~/.cline/skills/` |

**Windows:** Replace `~/` with `%USERPROFILE%\`

### 3. Download

Download the `skills/generating-aws-diagrams/` folder from this repo into your chosen install path.

**Important:**
- Must download to a subfolder named `generating-aws-diagrams`
- Correct structure: `{install_path}/generating-aws-diagrams/SKILL.md`
- Auto-create the directory if it doesn't exist

### 4. Verify

Confirm that `{install_path}/generating-aws-diagrams/SKILL.md` exists.

---

## Install: generating-gcp-diagrams

**GitHub:** `https://github.com/DavidDaskalo/drawio-architect`

### 1. Choose Installation Scope

- **Project-level** — skill is available only in the current project
- **Global** — skill is available across all projects for your agent

### 2. Determine Your Install Path

**Mac / Linux:**

| Agent | Project Path | Global Path |
|-------|-------------|-------------|
| Claude | `.claude/skills/` | `~/.claude/skills/` |
| Cursor | `.cursor/skills/` | `~/.cursor/skills/` |
| Codex | `.codex/skills/` | `~/.codex/skills/` |
| Antigravity | `.agent/skills/` | `~/.gemini/antigravity/global_skills/` |
| Windsurf | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |
| Cline | `.cline/skills/` | `~/.cline/skills/` |

**Windows:** Replace `~/` with `%USERPROFILE%\`

### 3. Download

Download the `skills/generating-gcp-diagrams/` folder from this repo into your chosen install path.

**Important:**
- Must download to a subfolder named `generating-gcp-diagrams`
- Correct structure: `{install_path}/generating-gcp-diagrams/SKILL.md`
- Auto-create the directory if it doesn't exist

### 4. Verify

Confirm that `{install_path}/generating-gcp-diagrams/SKILL.md` exists.

---

## Usage

Once installed, invoke by asking your agent:

- **AWS**: "Generate an AWS architecture diagram for a serverless API with Lambda, API Gateway, and DynamoDB"
- **GCP**: "Create a GCP architecture diagram with Cloud Run, Pub/Sub, and BigQuery"

The agent will generate a `.drawio` XML file you can open in [DrawIO Desktop](https://www.drawio.com/) or the web editor.

## Requirements

- Python 3 (for validation scripts)
- DrawIO Desktop (optional, for export/preview) — `brew install drawio`

## License

MIT
