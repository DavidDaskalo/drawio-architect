# Evals Strategy for DrawIO Architect Skill

This document outlines a comprehensive evaluation strategy for testing the quality and accuracy of DrawIO diagram generation.

---

## Multi-Level Eval Strategy

### Level 1: XML Structural Validity (Fast, Deterministic)
**Purpose:** Ensure generated XML is well-formed and valid
**Runtime:** <100ms per diagram
**Frequency:** Run on every commit (CI)

#### Key Tests

```python
# tests/unit/test_xml_structure.py

def test_required_root_cells():
    """Every diagram must have id='0' and id='1' root cells"""
    xml = parse_drawio(generated_file)
    assert has_cell_with_id(xml, "0")
    assert has_cell_with_id(xml, "1")

def test_all_parents_exist():
    """All parent references must point to existing cells"""
    xml = parse_drawio(generated_file)
    for cell in get_all_cells(xml):
        if cell.has_parent():
            assert parent_exists(xml, cell.parent_id)

def test_connections_have_valid_targets():
    """All edges must reference valid source/target vertices"""
    xml = parse_drawio(generated_file)
    for edge in get_edges(xml):
        assert vertex_exists(xml, edge.source)
        assert vertex_exists(xml, edge.target)

def test_no_dangling_references():
    """All shape references in styles are valid"""
    xml = parse_drawio(generated_file)
    for cell in get_all_cells(xml):
        if has_gcp_shape(cell):
            shape_name = extract_shape_name(cell)
            assert shape_name in VALID_GCP_SHAPES

def test_geometry_validity():
    """All cells with geometry have valid coordinates"""
    xml = parse_drawio(generated_file)
    for cell in get_all_cells(xml):
        if has_geometry(cell):
            geom = get_geometry(cell)
            assert geom.width > 0
            assert geom.height > 0
            assert geom.x is not None
            assert geom.y is not None
```

**Metrics to Track:**
- Parse success rate (target: 100%)
- Average XML validation errors per diagram (target: 0)
- Cell ID uniqueness (target: 100%)
- Parent reference integrity (target: 100%)

---

### Level 2: Visual Best Practices (Rule-Based)
**Purpose:** Ensure generated diagrams follow documented standards
**Runtime:** <500ms per diagram
**Frequency:** Run on every commit (CI)

#### Key Tests

```python
# tests/unit/test_visual_standards.py

def test_service_icon_font_color():
    """Service labels should use #424242, not #999999"""
    xml = parse_drawio(generated_file)
    for service in get_service_icons(xml):
        font_color = extract_font_color(service)
        assert font_color == "#424242", \
            f"Service {service.id} has wrong font color: {font_color}"

def test_labeled_connections_have_background():
    """Labeled connections must include labelBackgroundColor"""
    xml = parse_drawio(generated_file)
    for edge in get_labeled_edges(xml):
        assert "labelBackgroundColor=#FFFFFF" in edge.style, \
            f"Edge {edge.id} missing labelBackgroundColor"
        assert "fontSize=10" in edge.style
        assert "fontColor=#333333" in edge.style

def test_gcp_project_uses_two_cell_pattern():
    """GCP Project containers should use two-cell pattern"""
    xml = parse_drawio(generated_file)
    for container in get_gcp_project_containers(xml):
        # Parent cell should NOT have shape attribute
        assert not has_shape_attribute(container), \
            f"Container {container.id} incorrectly has shape attribute"

        # Should have child with GCP logo
        logo_child = get_child_with_shape(container, "google_cloud_platform")
        assert logo_child is not None, \
            f"Container {container.id} missing GCP logo child"

        # Logo should be 23x20 with relative geometry
        assert logo_child.width == 23
        assert logo_child.height == 20
        assert has_relative_geometry(logo_child)

def test_connection_consolidation():
    """No more than 2 parallel connections between same groups"""
    xml = parse_drawio(generated_file)
    connection_map = build_connection_map(xml)

    for (source, target), connections in connection_map.items():
        assert len(connections) <= 2, \
            f"Too many connections ({len(connections)}) between {source} and {target}"

def test_vpc_container_styling():
    """VPC-SC containers use correct green styling"""
    xml = parse_drawio(generated_file)
    for vpc in get_vpc_containers(xml):
        assert "fillColor=#E8F5E9" in vpc.style
        assert "strokeColor=#4CAF50" in vpc.style
        assert "fontColor=#2E7D32" in vpc.style
```

**Metrics to Track:**
- Font color compliance rate (target: >95%)
- Connection label background rate (target: >95%)
- Two-cell pattern adherence (target: 100%)
- Connection consolidation rate (target: >90%)
- Container styling accuracy (target: 100%)

---

### Level 3: Semantic Accuracy (LLM-as-Judge)
**Purpose:** Does the diagram match the text description?
**Runtime:** ~2-5s per diagram
**Frequency:** Run on PR, nightly full suite

#### Test Structure

```python
# tests/integration/test_semantic_accuracy.py

@pytest.mark.parametrize("prompt,expected_services,expected_connections", [
    (
        "Cloud Run → BigQuery",
        ["cloud_run", "bigquery"],
        [("cloud_run", "bigquery")]
    ),
    (
        "VPC-SC with Cloud Scheduler triggering Pub/Sub",
        ["cloud_scheduler", "pubsub"],
        [("cloud_scheduler", "pubsub")]
    ),
    (
        "Cloud Run reads from BigQuery and writes to Cloud Storage",
        ["cloud_run", "bigquery", "cloud_storage"],
        [("cloud_run", "bigquery"), ("cloud_run", "cloud_storage")]
    ),
])
def test_required_services_present(prompt, expected_services, expected_connections):
    """Generated diagram contains all requested services"""
    diagram = generate_diagram(prompt)
    xml = parse_drawio(diagram)

    # Check all services present
    actual_services = get_all_service_shapes(xml)
    for expected in expected_services:
        assert f"mxgraph.gcp2.{expected}" in actual_services, \
            f"Missing service: {expected}"

    # Check connections
    actual_connections = get_all_connections(xml)
    for (source, target) in expected_connections:
        assert has_connection(xml, source_contains=source, target_contains=target), \
            f"Missing connection: {source} → {target}"

def test_connection_directions():
    """Connections flow in the correct direction"""
    prompt = "Cloud Scheduler triggers Cloud Run which queries BigQuery"
    diagram = generate_diagram(prompt)
    xml = parse_drawio(diagram)

    # Verify flow: Scheduler → Cloud Run → BigQuery
    assert has_connection(xml, source="scheduler", target="cloud_run")
    assert has_connection(xml, source="cloud_run", target="bigquery")

    # Verify NO reverse connections
    assert not has_connection(xml, source="cloud_run", target="scheduler")
    assert not has_connection(xml, source="bigquery", target="cloud_run")

def test_container_nesting():
    """Services are placed in correct containers"""
    prompt = "VPC-SC containing Cloud Run and BigQuery"
    diagram = generate_diagram(prompt)
    xml = parse_drawio(diagram)

    vpc = get_container_by_type(xml, "gcp_vpc_sc")
    assert vpc is not None

    run = get_service_by_name(xml, "cloud_run")
    bq = get_service_by_name(xml, "bigquery")

    assert get_parent(run) == vpc.id
    assert get_parent(bq) == vpc.id
```

#### LLM Judge Evaluation

```python
# tests/integration/test_llm_judge.py

def eval_diagram_accuracy_llm(prompt: str, diagram_path: str) -> dict:
    """Use LLM to score how well diagram matches prompt"""

    # Extract structured representation
    xml = parse_drawio(diagram_path)
    structure = {
        "services": [s.name for s in get_all_services(xml)],
        "containers": [c.label for c in get_all_containers(xml)],
        "connections": [
            {"from": c.source_label, "to": c.target_label, "label": c.label}
            for c in get_all_connections(xml)
        ]
    }

    judge_prompt = f"""
You are evaluating the accuracy of a generated GCP architecture diagram.

USER REQUEST: "{prompt}"

GENERATED DIAGRAM:
- Services: {', '.join(structure['services'])}
- Containers: {', '.join(structure['containers'])}
- Connections: {json.dumps(structure['connections'], indent=2)}

Score 0-10 on these criteria:
1. Completeness: Are all requested services present?
2. Connections: Are connections in the correct direction with appropriate labels?
3. Containers: Are services grouped appropriately?
4. Layout: Is the logical flow correct (e.g., left-to-right for data flow)?

Return JSON:
{{
  "overall_score": <0-10>,
  "completeness_score": <0-10>,
  "connections_score": <0-10>,
  "containers_score": <0-10>,
  "layout_score": <0-10>,
  "reasoning": "<brief explanation>",
  "missing_elements": ["<element1>", ...],
  "incorrect_elements": ["<element1>", ...]
}}
"""

    response = call_llm_judge(judge_prompt, model="claude-sonnet-4.5")
    result = json.loads(response)

    return result

# Example test using LLM judge
def test_complex_architecture_llm():
    """Test complex multi-service architecture with LLM judge"""
    prompt = """
    Create a data pipeline architecture:
    - Cloud Scheduler triggers Cloud Run
    - Cloud Run processes data from Pub/Sub
    - Processed data goes to BigQuery and Cloud Storage
    - Vertex AI reads from BigQuery for training
    - Everything in VPC Service Controls
    """

    diagram = generate_diagram(prompt)
    result = eval_diagram_accuracy_llm(prompt, diagram)

    assert result['overall_score'] >= 7.0, \
        f"LLM judge score too low: {result['reasoning']}"
    assert result['completeness_score'] >= 8.0, \
        f"Missing elements: {result['missing_elements']}"
```

**Metrics to Track:**
- Average LLM judge score (target: >8.0/10)
- Service precision/recall (target: >95%)
- Connection accuracy (target: >90%)
- Container usage accuracy (target: >85%)

---

### Level 4: Visual Rendering (Screenshot Comparison)
**Purpose:** Does it actually render correctly in DrawIO?
**Runtime:** ~1-3s per diagram
**Frequency:** Run on PR, release candidates

#### Key Tests

```python
# tests/visual/test_rendering.py

def test_diagram_renders_without_errors():
    """Diagram opens in DrawIO without errors"""
    diagram = generate_diagram("Cloud Run → BigQuery")

    # Use DrawIO CLI to export to PNG
    result = subprocess.run([
        "drawio", "-x", "-f", "png",
        "-o", "/tmp/test.png",
        diagram
    ], capture_output=True)

    assert result.returncode == 0, \
        f"DrawIO export failed: {result.stderr}"
    assert os.path.exists("/tmp/test.png")
    assert os.path.getsize("/tmp/test.png") > 1000  # Non-trivial size

def test_icons_visible_in_render():
    """Service icons actually appear (not blank)"""
    diagram = generate_diagram("Cloud Run, BigQuery, and Cloud Storage")
    render_to_png(diagram, "/tmp/test.png")

    # Simple image analysis - check it's not all white/blank
    img = Image.open("/tmp/test.png")
    pixels = list(img.getdata())
    non_white = [p for p in pixels if p != (255, 255, 255, 255)]

    # At least 5% of pixels should be non-white (icons, text, borders)
    coverage = len(non_white) / len(pixels)
    assert coverage > 0.05, \
        f"Diagram appears blank, only {coverage*100:.1f}% non-white pixels"

def test_visual_regression():
    """Compare against golden reference images"""
    test_cases = load_golden_test_cases()

    for case in test_cases:
        diagram = generate_diagram(case['prompt'])
        render_to_png(diagram, "/tmp/current.png")

        # Pixel-wise comparison
        golden = Image.open(case['golden_image'])
        current = Image.open("/tmp/current.png")

        diff_percentage = compute_image_diff(golden, current)

        assert diff_percentage < case['max_diff'], \
            f"Visual regression detected: {diff_percentage}% diff"

def test_text_readability():
    """Verify labels are readable (not white-on-white, etc.)"""
    diagram = generate_diagram("Cloud Run labeled 'API Service'")
    render_to_png(diagram, "/tmp/test.png")

    # Use OCR to verify text is actually visible
    text = extract_text_from_image("/tmp/test.png")
    assert "API Service" in text, \
        "Label text not visible in rendered diagram"
```

**Metrics to Track:**
- Render success rate (target: >99%)
- Average pixel diff from golden images (target: <5%)
- Text visibility rate (target: 100%)
- Icon rendering success (target: 100%)

---

## Eval Dataset Structure

```
evals/
├── unit/                          # Level 1 & 2 - fast structural/style tests
│   ├── valid_xml/
│   │   ├── simple_services.drawio
│   │   ├── nested_containers.drawio
│   │   ├── complex_connections.drawio
│   │   └── edge_cases.drawio
│   └── best_practices/
│       ├── font_colors.drawio
│       ├── connection_consolidation.drawio
│       ├── two_cell_pattern.drawio
│       └── container_styling.drawio
│
├── integration/                   # Level 3 - semantic accuracy
│   ├── prompts.jsonl             # {"prompt": "...", "expected": {...}}
│   │
│   ├── simple/                   # 2-3 services, 1-2 connections
│   │   ├── two_services_connection.json
│   │   ├── single_container.json
│   │   └── basic_flow.json
│   │
│   ├── medium/                   # 4-8 services, multiple containers
│   │   ├── multi_container_flow.json
│   │   ├── branching_connections.json
│   │   └── nested_groups.json
│   │
│   └── complex/                  # 10+ services, complex topology
│       ├── full_data_pipeline.json
│       ├── microservices_arch.json
│       └── ml_training_pipeline.json
│
└── visual/                        # Level 4 - rendering tests
    ├── golden_images/             # Reference PNGs
    │   ├── simple_flow.png
    │   ├── vpc_container.png
    │   ├── complex_arch.png
    │   └── connection_labels.png
    └── test_cases.json
```

### Example Test Case Format

```json
{
  "id": "simple_flow_001",
  "category": "simple",
  "prompt": "Cloud Scheduler triggers Cloud Run which writes to BigQuery",
  "expected": {
    "services": ["cloud_scheduler", "cloud_run", "bigquery"],
    "connections": [
      {
        "from": "cloud_scheduler",
        "to": "cloud_run",
        "label": "trigger"
      },
      {
        "from": "cloud_run",
        "to": "bigquery",
        "label": "write"
      }
    ],
    "containers": ["gcp_project"],
    "layout": "left_to_right"
  },
  "constraints": {
    "max_services": 5,
    "max_connections": 3,
    "required_containers": []
  },
  "golden_image": "evals/visual/golden_images/simple_flow_001.png",
  "max_visual_diff": 0.05
}
```

---

## Metrics Dashboard

Track these metrics over time to monitor skill quality:

```python
metrics = {
    "structural_validity": {
        "parse_success_rate": 0.98,      # target: 1.00
        "avg_xml_errors": 0.2,           # target: 0
        "cell_id_uniqueness": 1.00,      # target: 1.00
        "parent_integrity": 1.00         # target: 1.00
    },

    "best_practices": {
        "font_color_compliance": 0.95,   # target: >0.95
        "connection_label_bg_rate": 0.92,# target: >0.95
        "consolidation_adherence": 0.88, # target: >0.90
        "two_cell_pattern_rate": 1.00,   # target: 1.00
        "container_styling": 0.96        # target: >0.95
    },

    "semantic_accuracy": {
        "avg_llm_judge_score": 8.3,      # target: >8.0
        "service_precision": 0.97,       # target: >0.95
        "service_recall": 0.94,          # target: >0.95
        "connection_accuracy": 0.91,     # target: >0.90
        "container_accuracy": 0.88       # target: >0.85
    },

    "visual_rendering": {
        "render_success_rate": 0.99,     # target: >0.99
        "avg_pixel_diff": 0.03,          # target: <0.05
        "text_visibility": 1.00,         # target: 1.00
        "icon_rendering": 1.00           # target: 1.00
    },

    "performance": {
        "avg_generation_time_ms": 450,   # track over time
        "avg_validation_time_ms": 80,    # track over time
        "xml_size_kb": 15                # track over time
    }
}
```

---

## Regression Test Suite

```bash
#!/bin/bash
# run_evals.sh

# Fast unit tests - run on every commit
pytest tests/unit/test_xml_structure.py -v --fast
pytest tests/unit/test_visual_standards.py -v --fast

# Integration tests - run on PR
pytest tests/integration/test_semantic_accuracy.py -v --medium

# Visual tests - run before release
pytest tests/visual/test_rendering.py -v --slow

# Full suite with report generation
pytest tests/ --full --generate-report --html=evals/report.html
```

### CI Pipeline Integration

```yaml
# .github/workflows/evals.yml
name: Evals

on: [push, pull_request]

jobs:
  unit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run unit tests (Level 1 & 2)
        run: pytest tests/unit -v --fast
        timeout-minutes: 2

  integration:
    runs-on: ubuntu-latest
    if: github.event_name == 'pull_request'
    steps:
      - uses: actions/checkout@v3
      - name: Run integration tests (Level 3)
        run: pytest tests/integration -v --medium
        timeout-minutes: 10

  visual:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - name: Install DrawIO
        run: |
          wget https://github.com/jgraph/drawio-desktop/releases/download/v22.1.15/drawio-amd64-22.1.15.deb
          sudo dpkg -i drawio-amd64-22.1.15.deb
      - name: Run visual tests (Level 4)
        run: pytest tests/visual -v --slow
        timeout-minutes: 15
```

---

## Key Eval Principles

1. **Fast feedback loop** - Level 1 & 2 run in CI on every commit (<5s total)
2. **Hierarchical** - Pass structural validation before checking semantics
3. **Golden test set** - Curated examples covering edge cases
4. **Version tracking** - Tag evals with skill version (e.g., v1.3)
5. **Failure analysis** - Log failures with full context for debugging
6. **Progressive difficulty** - Simple → Medium → Complex test cases
7. **Deterministic where possible** - Use rule-based tests before LLM judge
8. **Real-world validation** - Visual rendering tests ensure DrawIO compatibility

---

## Implementation Priority

### Phase 1: Foundation (Week 1)
- [ ] Implement Level 1 XML structural tests
- [ ] Set up test infrastructure and pytest configuration
- [ ] Create initial golden test set (10 diagrams)

### Phase 2: Best Practices (Week 2)
- [ ] Implement Level 2 visual standards tests
- [ ] Add rule-based validators for documented best practices
- [ ] Expand test coverage to 30 diagrams

### Phase 3: Semantic Tests (Week 3)
- [ ] Implement Level 3 semantic accuracy tests
- [ ] Build LLM-as-judge evaluation pipeline
- [ ] Create comprehensive test case library (50+ prompts)

### Phase 4: Visual Validation (Week 4)
- [ ] Implement Level 4 rendering tests
- [ ] Set up DrawIO CLI integration
- [ ] Create visual regression framework

### Phase 5: CI/CD Integration (Week 5)
- [ ] Configure GitHub Actions workflows
- [ ] Set up metrics dashboard
- [ ] Establish quality gates and thresholds

---

## Future Enhancements

- **Human eval**: Collect user ratings on generated diagrams
- **A/B testing**: Compare different generation strategies
- **Benchmark suite**: Track performance against other diagramming tools
- **Automated golden image generation**: Use best diagrams as new references
- **Multi-cloud evals**: Prepare for AWS/Azure expansion
