# DrawIO Skills MVP - Project Specification

## Executive Summary
Create a foundational skill that enables Claude to accurately convert architecture diagram images into DrawIO XML format, with initial focus on AWS and GCP cloud services.

## MVP Scope (Phase 1)

### What We're Building
A single skill: `drawio-architect` that can:
1. Analyze existing DrawIO XML files to extract shape codes and patterns
2. Maintain a reference library of common GCP cloud service icons
3. Convert GCP architecture diagram images to DrawIO XML format

### What We're NOT Building (Future Phases)
- AWS support (Phase 2)
- Azure/Kubernetes support (Phase 3)
- Auto-layout algorithms (Phase 3)
- DrawIO file editing/updates (Phase 4)
- Integration with IaC tools (Phase 4)

## Success Criteria

### MVP is successful if:
- [ ] Can identify 30+ GCP service icons from images
- [ ] Can extract shape codes from existing DrawIO XML files
- [ ] Can generate valid DrawIO XML that opens in diagrams.net
- [ ] Can recreate your original GCP architecture diagram with 85%+ accuracy
- [ ] Can create new GCP diagrams from text descriptions

## Technical Requirements

### Inputs
1. **For Extraction**: `.drawio` XML file
2. **For Recreation**: Architecture diagram image (PNG/JPG)
3. **For Creation**: Text specification or description

### Outputs
1. Valid DrawIO XML file (`.drawio`)
2. Analysis report (Markdown) with shape inventory
3. Confidence report for image-to-XML conversions

### Core Data Structures

#### Shape Library Entry
```json
{
  "service_name": "GCP Cloud Run",
  "provider": "gcp",
  "category": "compute",
  "visual_signatures": {
    "container": "white rounded rectangle with drop shadow",
    "icon_description": "blue right-pointing arrow/triangle symbol",
    "icon_position": "left side of container",
    "text_position": "right of icon",
    "icon_color": "#4285F4",
    "background_color": "#FFFFFF",
    "border_color": "#E0E0E0",
    "has_shadow": true
  },
  "drawio_shape": {
    "library": "mxgraph.gcp2",
    "shape_name": "cloud_run",
    "full_style": "sketch=0;html=1;verticalAlign=top;labelPosition=center;verticalLabelPosition=bottom;align=center;spacingTop=-6;fontSize=11;fontStyle=0;fontColor=#999999;shape=mxgraph.gcp2.cloud_run"
  },
  "recognition_keywords": ["cloud run", "serverless", "container", "run"],
  "common_labels": ["Cloud Run", "data transfer", "snow agent", "root agent"]
}
```

#### Container Style Template
```json
{
  "container_type": "gcp_vpc_sc",
  "style": "rounded=1;whiteSpace=wrap;html=1;fillColor=#E8F5E9;strokeColor=#4CAF50;strokeWidth=2;verticalAlign=top;fontSize=14;fontStyle=1;align=center;arcSize=5;",
  "typical_label_prefix": "Google Cloud Platform\nVPC-SC",
  "background_color": "#E8F5E9",
  "border_color": "#4CAF50"
}
```

## Project Structure

```
/mnt/skills/user/drawio-architect/
├── SKILL.md                          # Main skill file with instructions
├── README.md                         # Usage guide and examples
├── libraries/
│   ├── gcp-icons.json               # GCP service icon database
│   └── containers.json              # Container/grouping styles (GCP focused)
├── templates/
│   ├── drawio-base.xml              # Base DrawIO XML structure
│   ├── node-template.xml            # Template for service nodes
│   └── connection-template.xml      # Template for connections
├── examples/
│   ├── sample-gcp-simple.drawio     # Simple GCP diagram reference
│   ├── sample-gcp-complex.drawio    # Your original diagram (goal)
│   └── analysis-output.md           # Example analysis report
└── utilities/
    ├── xml-parser-guide.md          # How to parse DrawIO XML
    ├── coordinate-system.md         # Positioning guide
    └── style-guide.md               # Styling best practices (GCP focused)
```

## MVP Deliverables

### 1. Shape Library Files (Priority: CRITICAL)

**File: `libraries/gcp-icons.json`**
- Minimum 30 GCP services covering:
  - **Compute**: Cloud Run (4 variations: data transfer, snow agent, root agent, sharepoint), Compute Engine, GKE, Cloud Functions, App Engine
  - **Database**: BigQuery (2 variations), Cloud SQL, Firestore, Spanner, Bigtable, Memorystore
  - **Storage**: Cloud Storage, Filestore, Persistent Disk, Cloud Storage Transfer
  - **Networking**: VPC, Cloud Load Balancing, Cloud CDN, Cloud DNS, Cloud Armor
  - **AI/ML**: Vertex AI (Search Vector DB), AI Platform, Vision API, Natural Language, Speech-to-Text
  - **Integration**: Pub/Sub, Cloud Tasks, Workflows, Eventarc, Cloud Scheduler
  - **Operations**: Cloud Logging, Cloud Monitoring, Cloud Trace, Error Reporting
  - **API Management**: Apigee, API Gateway, Cloud Endpoints

**File: `libraries/containers.json`**
- GCP Project container
- GCP VPC-SC container (like your diagram)
- Generic grouping boxes (dashed - "Same Instance" style)
- Solid grouping boxes

### 2. Core Skill File (Priority: CRITICAL)

**File: `SKILL.md`**

Structure:
```markdown
# DrawIO Architect Skill

## Purpose
Enable accurate creation and analysis of architecture diagrams in DrawIO format.

## Capabilities
1. Extract and analyze existing DrawIO XML files
2. Identify cloud service icons from images
3. Generate valid DrawIO XML from images or descriptions

## Usage Instructions

### Task 1: Analyze a DrawIO File
[Step-by-step instructions]

### Task 2: Convert Image to DrawIO
[Step-by-step instructions]

### Task 3: Create DrawIO from Description
[Step-by-step instructions]

## Shape Library Reference
[How to use the JSON libraries]

## XML Structure Guide
[DrawIO XML basics]

## Troubleshooting
[Common issues and solutions]
```

### 3. XML Templates (Priority: HIGH)

**File: `templates/drawio-base.xml`**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<mxfile host="app.diagrams.net" modified="..." agent="Claude" version="22.0.0">
  <diagram name="Architecture" id="architecture">
    <mxGraphModel dx="1434" dy="844" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1600" pageHeight="900" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Components will be inserted here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

### 4. Utility Guides (Priority: MEDIUM)

**File: `utilities/xml-parser-guide.md`**
- How DrawIO XML is structured
- How to extract shape information
- How to identify connections
- How to parse styles

**File: `utilities/coordinate-system.md`**
- DrawIO coordinate system explanation
- How to calculate positions
- Alignment and spacing guidelines
- Layout patterns

**File: `utilities/style-guide.md`**
- Common style attributes
- Color schemes for cloud providers
- Shadow and border effects
- Font and text styling

### 5. Example Files (Priority: MEDIUM)

**File: `examples/sample-gcp-simple.drawio`**
- Simple GCP architecture (Cloud Run + BigQuery + Cloud Storage)
- Should demonstrate VPC-SC container
- Include connections with labels
- Should be straightforward to recreate

**File: `examples/sample-gcp-complex.drawio`**
- Recreation of YOUR original diagram from the image
- VPC-SC with multiple Cloud Run instances
- BigQuery, Cloud Storage, Vertex AI
- "Same Instance" dashed container
- Cloud Scheduler, Apigee
- All proper connections and styling

**File: `examples/analysis-output.md`**
- Example of what the analysis report should look like
- Shape inventory table with GCP services
- Connection matrix
- Style analysis
- Container hierarchy

## Implementation Tasks for Claude Code

### Task 1: Research and Document GCP Shape Library
**Objective**: Build comprehensive GCP icon database

**Steps**:
1. Create `libraries/gcp-icons.json` structure
2. For each GCP service (30+ services):
   - Search for official GCP architecture icons
   - Document visual appearance (icon, container, colors)
   - Find corresponding DrawIO shape code (mxgraph.gcp2.*)
   - Test in DrawIO to confirm it works
   - Document the full style string
   - Note variations (e.g., Cloud Run appears 4 times in your diagram)
3. Create `libraries/containers.json` with GCP-specific container styles
4. Focus on services from your original diagram first:
   - Cloud Run (priority #1)
   - BigQuery (priority #2)
   - Cloud Storage (priority #3)
   - Vertex AI Search (priority #4)
   - Cloud Scheduler (priority #5)
   - Apigee (priority #6)

**Acceptance Criteria**:
- JSON file is valid and well-structured
- Each entry has complete visual_signatures
- Each entry has tested drawio_shape code
- All services from your diagram are documented
- Minimum 30 total GCP services documented

### Task 2: Create XML Parser and Extractor
**Objective**: Build tools to analyze existing DrawIO files

**Steps**:
1. Create `utilities/xml-parser-guide.md`
2. Document how to:
   - Parse mxCell elements
   - Extract style attributes
   - Identify shape types
   - Map connections
   - Handle nested containers
3. Create example Python/JS code snippets for common operations
4. Test with sample DrawIO files

**Acceptance Criteria**:
- Guide is clear and actionable
- Code snippets are tested and working
- Covers all major DrawIO XML elements

### Task 3: Create the Main Skill File
**Objective**: Write comprehensive SKILL.md

**Steps**:
1. Write clear purpose and capabilities section
2. Create step-by-step workflows for each use case:
   - Analyzing existing files
   - Converting images to XML
   - Creating from descriptions
3. Include examples and best practices
4. Add troubleshooting section
5. Reference all library files

**Acceptance Criteria**:
- Claude can follow the instructions to complete tasks
- All workflows are clear and complete
- Examples are practical and tested

### Task 4: Create Templates
**Objective**: Build reusable XML templates

**Steps**:
1. Create base DrawIO XML structure
2. Create node template with placeholder values
3. Create connection template
4. Document how to use templates
5. Test that generated XML opens in diagrams.net

**Acceptance Criteria**:
- Templates are valid XML
- Templates include all necessary attributes
- Documentation explains customization

### Task 5: Build Example Files
**Objective**: Provide reference implementations

**Steps**:
1. Create simple GCP architecture diagram in DrawIO:
   - 3-5 services (e.g., Cloud Run → BigQuery → Cloud Storage)
   - VPC-SC container
   - Clean layout
2. Recreate YOUR original diagram in DrawIO:
   - Use the documented shape codes
   - Match the layout as closely as possible
   - Get all connections right (solid, dashed, bidirectional)
   - Match container styling
3. Run extraction on both to create analysis report
4. Compare generated diagram to your original image
5. Document accuracy percentage and any discrepancies
6. Add both to examples/ directory

**Acceptance Criteria**:
- Example files open correctly in diagrams.net
- Simple example is clean and well-structured
- Complex example matches your original ≥85% accuracy
- Analysis reports are accurate
- Discrepancies are documented

### Task 6: Testing and Validation
**Objective**: Ensure MVP works end-to-end with your diagram

**Steps**:
1. Test extracting shapes from example files
2. Test recreating your original diagram from the image you provided
3. Measure accuracy:
   - Icon matching accuracy
   - Layout similarity
   - Connection accuracy
   - Style matching
4. Generate final analysis report
5. Document any limitations specific to GCP diagrams
6. Create issue list for Phase 2 (AWS support)

**Acceptance Criteria**:
- Can extract all shapes from examples correctly
- Can recreate your diagram with 85%+ accuracy
- All GCP services in your diagram are correctly identified
- All connections match (including bidirectional arrows)
- Container styling matches
- All limitations are documented

## Development Workflow

### For Claude Code:
1. Read this specification completely
2. Create the directory structure
3. Execute tasks in order (1 → 6)
4. After each task, verify acceptance criteria
5. Document any blockers or questions
6. Produce final deliverable package

### Quality Checklist:
- [ ] All JSON files are valid JSON
- [ ] All XML files are valid XML
- [ ] All Markdown files are well-formatted
- [ ] File paths in documentation are correct
- [ ] Examples have been tested in actual DrawIO
- [ ] Shape codes have been verified to work
- [ ] Documentation is clear and actionable

## MVP Timeline Estimate

- Task 1 (GCP Shape Library): 2-3 hours
- Task 2 (Parser Guide): 1-2 hours
- Task 3 (Skill File): 2 hours
- Task 4 (Templates): 1 hour
- Task 5 (Examples): 2-3 hours
- Task 6 (Testing): 1-2 hours

**Total: 9-13 hours of focused work**

*Reduced from 10-15 hours by focusing only on GCP*

## Success Metrics

After MVP completion, Claude should be able to:
1. Look up any of the 30+ documented GCP services and get exact DrawIO shape code
2. Parse an existing DrawIO file and extract all GCP components
3. Take your architecture diagram and recreate it in DrawIO with proper icons (85%+ accuracy)
4. Generate a detailed analysis report for any GCP-focused DrawIO file
5. Create new GCP architecture diagrams from text descriptions

## Phase 2 Preview (Future)

After GCP MVP proves successful:
- **Phase 2A**: Add AWS icon library (30+ services)
- **Phase 2B**: Add Azure icon library (30+ services)
- **Phase 3**: Add Kubernetes icon library (20+ resources)
- **Phase 4**: Add auto-layout algorithms
- **Phase 5**: Add interactive editing capabilities
- **Phase 6**: Add style consistency checker
- **Phase 7**: Add diagram comparison tool
- **Phase 8**: Multi-cloud hybrid diagram support

## Questions to Answer During Development

1. What's the best way to store style strings? (Full vs. partial)
2. Should we include color variations for dark mode?
3. How to handle deprecated services or icon changes?
4. Should we support custom/user-created shapes?
5. What level of connection intelligence is needed?

## References

- DrawIO documentation: https://www.drawio.com/
- GCP Architecture Icons: https://cloud.google.com/icons
- GCP Architecture Center: https://cloud.google.com/architecture
- DrawIO shape libraries: https://github.com/jgraph/drawio
- GCP shape library in DrawIO: Search "GCP" in shape libraries panel

## Your Original Diagram

The primary test case is your uploaded GCP architecture diagram showing:
- VPC-SC container (light green background)
- Cloud Scheduler
- Cloud Run (4 instances: data transfer, snow agent, root agent, sharepoint)
- BigQuery (2 instances)
- Cloud Storage (Visual Data)
- Vertex AI Search Vector DB
- Apigee
- "Same Instance" dashed container grouping
- Various connection types (solid, dashed, bidirectional)

---

## Ready to Start?

This specification is complete and ready for Claude Code to execute. The output will be a fully functional skill that solves the original problem: accurate recreation of architecture diagrams in DrawIO format.