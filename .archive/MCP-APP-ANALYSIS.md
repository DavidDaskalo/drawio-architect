# MCP App Analysis for DrawIO Architect

**Date:** 2026-01-31
**Status:** Not Recommended for Current Use Case

---

## Executive Summary

Implementing an MCP App for DrawIO Architect would be **significant over-engineering** that contradicts the skill's core value proposition. The current Agent Skills approach (file-based, AI-driven, zero runtime) is the optimal solution for text-to-diagram generation.

**Verdict:** MCP Apps are NOT practical for this use case, but could be valuable if the project pivots to a visual diagram editor.

---

## Current Architecture vs MCP App

### Current Approach (Agent Skills) ✅

**Architecture:**
- File-based: SKILL.md + JSON assets + templates
- AI-driven: Natural language → DrawIO XML
- Zero runtime: No server, no dependencies
- Portable: Works with any Agent Skills-compatible AI
- Offline-capable: Everything is local files

**User Experience:**
```
User: "Create a GCP architecture with Cloud Run, BigQuery, and Pub/Sub"
AI: [Generates .drawio XML directly]
```

**Dependencies:**
- None (runtime)
- Python 3 (optional, for validation scripts)
- DrawIO Desktop (optional, for preview)

### MCP App Alternative ❌

**Architecture:**
- Server-side (Node.js): Tools, resource serving, HTTP endpoint
- Client-side UI: Drag-drop canvas, service palette, form builder
- Build system: Vite, TypeScript, bundler, framework
- Runtime: npm install, build, serve, port management
- Testing: Tunneling (cloudflared) for Claude web access

**User Experience:**
```
User: "Show me a diagram builder"
AI: [Launches MCP App UI in iframe]
User: [Clicks "Cloud Run" button, drags to canvas, clicks "BigQuery", draws connection]
```

**Dependencies:**
- Node.js 18+
- @modelcontextprotocol/ext-apps
- @modelcontextprotocol/sdk
- Vite + vite-plugin-singlefile
- Express + cors
- TypeScript + tsx
- UI framework (React/Vue/Svelte/etc.)

---

## Why MCP App Is NOT Practical

### 1. Complexity Explosion

| Component | Current | MCP App |
|-----------|---------|---------|
| Server code | 0 lines | 500+ lines |
| UI code | 0 lines | 1000+ lines |
| Build config | 0 files | 4 files (package.json, tsconfig.json, vite.config.ts, server.ts) |
| Build process | None | npm install && npm run build && npm run serve |
| Port management | None | localhost:3001 (+ conflicts) |
| Deployment | Copy files | npm install, build, serve, tunnel |

### 2. Wrong Interaction Model

The skill's core value is **AI generates diagrams from natural language**, not **humans build diagrams with UI**.

**Natural language is MORE efficient than UI for this task:**

- NL: "Create a GCP architecture with Cloud Run connecting to BigQuery via Pub/Sub"
- UI: Click Cloud Run → Drag to canvas → Click Pub/Sub → Drag to canvas → Click BigQuery → Drag to canvas → Click connection tool → Draw from Cloud Run to Pub/Sub → Draw from Pub/Sub to BigQuery

**User intent analysis:**
- Users want: Quick diagram generation from description
- They don't want: Manual diagram construction via clicking/dragging

### 3. Reduced Portability

| Environment | Agent Skills | MCP App |
|-------------|--------------|---------|
| Claude Code | ✅ | ❌ (not MCP host) |
| Cursor | ✅ | ❌ |
| Cline | ✅ | ❌ |
| Windsurf | ✅ | ❌ |
| Claude Web | ✅ | ✅ (requires tunnel) |
| Claude Desktop | ✅ | ✅ |
| VS Code Insiders | ✅ | ✅ |
| Any skill-compatible AI | ✅ | ❌ |

**MCP Apps only work with MCP-compatible hosts:** Claude web/desktop, VS Code Insiders, Goose, Postman, MCPJam

### 4. Runtime Overhead

**Current setup (Agent Skills):**
```bash
# User does nothing - skill just works
```

**MCP App setup:**
```bash
# User must run:
npm install
npm run build
npm run serve

# Keep server running in background
# Manage port conflicts (3001)

# For Claude web, also run:
npx cloudflared tunnel --url http://localhost:3001
# Copy tunnel URL to Claude custom connector settings
```

### 5. Maintenance Burden

| Aspect | Agent Skills | MCP App |
|--------|--------------|---------|
| Security updates | None | npm audit, package updates |
| Breaking changes | Rare (JSON/markdown) | Frequent (npm ecosystem) |
| Testing | Read files, validate JSON | Server tests, UI tests, E2E tests |
| Documentation | SKILL.md | Server API docs, UI component docs, MCP protocol docs |
| Debugging | Read XML output | Server logs, browser console, network tab, postMessage debugging |

---

## When MCP App WOULD Be Practical

MCP Apps excel at **interactive data exploration**, **real-time dashboards**, and **complex configuration UIs**. The DrawIO Architect skill would benefit from MCP App if the use case were:

### Scenario 1: Interactive Diagram Editor (Not Generator)

**Use case:** Users manually build diagrams with visual feedback

**MCP App features:**
- Drag-drop canvas with GCP service palette
- Click to add services, drag to position
- Visual connection drawing
- Real-time position/alignment guides
- Undo/redo stack
- Live DrawIO XML preview pane

**Why it makes sense:**
- Users WANT manual control
- Visual feedback is critical
- Multiple back-and-forth adjustments
- Canvas state needs to persist

**Current approach doesn't support:** Visual editing (only text → XML generation)

### Scenario 2: Visual Refinement Workflow

**Use case:** AI generates initial diagram, user tweaks visually

**MCP App workflow:**
1. AI generates initial .drawio from text description
2. MCP App UI loads the generated diagram
3. User drags services to better positions
4. User clicks to add/remove connections
5. Real-time preview shows changes
6. Export refined XML

**Why it makes sense:**
- Combines AI generation (fast initial layout) + human refinement (aesthetic control)
- Visual tweaking is faster than re-describing in text
- Users see changes immediately

**Current approach doesn't support:** Post-generation visual editing

### Scenario 3: Complex Configuration Wizard

**Use case:** 50+ interdependent diagram options

**MCP App features:**
- Multi-page form wizard
- Conditional fields (e.g., "Enable VPC-SC?" → show VPC config)
- Live validation (red/green feedback)
- Preview pane showing diagram as user fills form
- Template selection (3-tier, microservices, data pipeline)

**Example fields:**
- Cloud provider (GCP/AWS/Azure)
- Region selection
- Service tier (compute, storage, database, ML)
- Network topology (hub-spoke, mesh, multi-region)
- Security zones (public, private, dmz)
- Compliance (HIPAA, PCI-DSS, SOC2)

**Why it makes sense:**
- Too many options to express in single text prompt
- Validation prevents invalid combinations
- Preview helps users understand choices

**Current approach works fine:** Text descriptions handle most cases

### Scenario 4: Real-Time Collaboration

**Use case:** Multiple users building diagram together

**MCP App features:**
- Shared canvas state
- Live cursors showing other users
- Real-time updates pushed via WebSocket
- Conflict resolution for simultaneous edits
- Chat sidebar for team discussion
- Version history / snapshots

**Why it makes sense:**
- Multiple users need synchronized state
- Push updates (not just pull)
- Collaborative design sessions

**Current approach doesn't support:** Multi-user collaboration

---

## Better Alternatives (If Interactivity Is Needed)

### Alternative 1: DrawIO Desktop Plugin

**Approach:** Extend DrawIO itself with GCP shape helpers

**Implementation:**
```javascript
// DrawIO plugin: Add GCP shape palette
DrawIOPlugin.addPalette('GCP Quick Add', {
  'Add VPC Container': () => insertShape('gcp_vpc_sc'),
  'Add Cloud Run': () => insertShape('cloud_run'),
  'Add BigQuery': () => insertShape('bigquery'),
});
```

**Advantages:**
- Users already have DrawIO installed
- No separate server/UI to maintain
- Leverages DrawIO's robust editor
- One-click shape insertion

**Disadvantages:**
- DrawIO plugin ecosystem learning curve
- Limited to DrawIO Desktop users

### Alternative 2: CLI Tool

**Approach:** Command-line interface for diagram generation

**Implementation:**
```bash
# Install globally
npm install -g drawio-architect-cli

# Generate diagram
drawio-architect create "Cloud Run pipeline with BigQuery" -o diagram.drawio

# Add service to existing diagram
drawio-architect add-service cloud_run --label "API" --parent vpc1 diagram.drawio

# List available services
drawio-architect list-services --filter compute

# Validate diagram
drawio-architect validate diagram.drawio
```

**Advantages:**
- Scriptable (CI/CD integration)
- No UI complexity
- Fast for power users
- Works in terminal environments

**Disadvantages:**
- Not visual
- Steeper learning curve for non-CLI users

### Alternative 3: Simple Web Form (No AI, No MCP)

**Approach:** Static HTML form with client-side XML generation

**Implementation:**
```html
<!DOCTYPE html>
<html>
<body>
  <h1>GCP Diagram Generator</h1>
  <form id="diagram-form">
    <label>Services:</label>
    <input type="checkbox" name="service" value="cloud_run"> Cloud Run
    <input type="checkbox" name="service" value="bigquery"> BigQuery

    <label>Container:</label>
    <select name="container">
      <option value="vpc">VPC Service Controls</option>
      <option value="project">GCP Project</option>
    </select>

    <button type="submit">Generate Diagram</button>
  </form>

  <script>
    // Pure client-side generation - no server needed
    document.getElementById('diagram-form').onsubmit = (e) => {
      e.preventDefault();
      const xml = generateDrawIOXML(new FormData(e.target));
      downloadFile('diagram.drawio', xml);
    };
  </script>
</body>
</html>
```

**Advantages:**
- Zero server (static hosting)
- Works offline
- Instant generation
- No dependencies

**Disadvantages:**
- Limited to predefined templates
- No AI assistance
- Basic functionality only

### Alternative 4: VSCode Extension

**Approach:** Editor extension with inline preview

**Implementation:**
```typescript
// VSCode extension
vscode.commands.registerCommand('drawio-architect.generateFromSelection', async () => {
  const editor = vscode.window.activeTextEditor;
  const selection = editor.document.getText(editor.selection);

  // Generate diagram from selected code/text
  const xml = await generateDiagram(selection);

  // Show inline preview
  const panel = vscode.window.createWebviewPanel(
    'drawioPreview',
    'DrawIO Preview',
    vscode.ViewColumn.Beside,
    {}
  );
  panel.webview.html = renderDrawIO(xml);
});
```

**Advantages:**
- Integrated into developer workflow
- Right-click code → generate diagram
- Inline preview in editor
- Version control integration

**Disadvantages:**
- VSCode-specific
- Extension marketplace submission required

---

## MCP App Use Cases (When They Excel)

MCP Apps are designed for scenarios where **interactive UI is essential**:

### 1. Data Exploration

**Examples from MCP repo:**
- cohort-heatmap-server: Click cells to drill down
- customer-segmentation-server: Adjust filters, see segments update
- wiki-explorer-server: Navigate linked articles visually

**Why it works:** Users need to interact with data, not just view it

### 2. Real-Time Dashboards

**Examples:**
- system-monitor-server: Live CPU/memory graphs
- Video/audio playback with controls
- Live metrics that update without prompting

**Why it works:** Push updates (server → UI) without user re-prompting

### 3. Complex Configuration

**Examples:**
- scenario-modeler-server: Multi-variable financial modeling
- budget-allocator-server: Drag sliders to allocate budget

**Why it works:** Too many interdependent options for text prompts

### 4. Media Viewers

**Examples:**
- pdf-server: Pan/zoom PDF viewer
- video-resource-server: Video player with timeline
- sheet-music-server: Musical notation with playback

**Why it works:** Rich media needs specialized renderers

### 5. Specialized Input

**Examples:**
- transcript-server: Record audio → speech-to-text
- qr-server: Generate/scan QR codes with camera
- shadertoy-server: Live shader code editor

**Why it works:** Requires browser APIs (microphone, camera, WebGL)

---

## Decision Matrix

| Criteria | Agent Skills | MCP App |
|----------|--------------|---------|
| **Use Case Fit** | ✅ Perfect (text → diagram) | ❌ Wrong model (UI → diagram) |
| **Complexity** | ✅ Minimal | ❌ High |
| **Portability** | ✅ Universal | ❌ MCP hosts only |
| **Setup** | ✅ Zero | ❌ npm install + build + serve |
| **Maintenance** | ✅ Low | ❌ High |
| **User Experience** | ✅ Natural language (fast) | ❌ Click/drag (slow) |
| **Dependencies** | ✅ None | ❌ Node.js + packages |
| **Offline Support** | ✅ Yes | ❌ No (needs server) |
| **AI Integration** | ✅ Core value | ❌ Peripheral |

**Score:** Agent Skills wins 8/8 criteria

---

## Recommendation

**Stick with Agent Skills.** The file-based approach is:

✅ **Simpler** - No build, no server, no runtime
✅ **More portable** - Works everywhere
✅ **Aligned with AI-first workflow** - Natural language beats clicking
✅ **Easier to maintain** - JSON files, not npm packages
✅ **Lower barrier to entry** - Users just talk to AI

MCP Apps shine for **interactive data exploration**, **real-time dashboards**, and **complex configuration UIs**. DrawIO Architect is about **AI-powered text-to-diagram generation**, which is fundamentally different.

**The only scenario** where MCP App would make sense: If the project pivots to building a **visual diagram editor** where the AI acts as an assistant WHILE the user manually constructs diagrams. But that's a completely different product.

---

## Future Considerations

If user feedback indicates desire for visual editing features:

1. **Phase 1:** Keep current Agent Skills approach
2. **Phase 2:** Add DrawIO Desktop plugin for manual refinement
3. **Phase 3:** Evaluate MCP App for collaborative editing
4. **Phase 4:** Only then consider full MCP App implementation

**Don't build features users haven't asked for.**

---

## References

- [MCP Apps Documentation](https://modelcontextprotocol.io/docs/extensions/apps.md)
- [MCP Apps API Reference](https://modelcontextprotocol.github.io/ext-apps/api/)
- [MCP Apps Examples](https://github.com/modelcontextprotocol/ext-apps/tree/main/examples)
- [MCP Apps Specification](https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/draft/apps.mdx)
- [Agent Skills Specification](https://agentskills.io/specification)
