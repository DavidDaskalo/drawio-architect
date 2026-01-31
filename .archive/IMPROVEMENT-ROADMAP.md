# DrawIO Architect - Improvement Roadmap

**Last Updated:** 2026-01-31

---

## Current Issues to Address

### 1. Empty/Missing Icons 🔴 **High Priority**

**Problem:** Some GCP service icons render as empty boxes or don't display correctly.

**Root Causes:**
- Shape names in `gcp-icons.json` may not match DrawIO's actual `mxgraph.gcp2.*` library
- DrawIO library updates may have renamed/removed shapes
- Some services may not have dedicated icons in the GCP shape library

**Solutions:**

**A. Audit All 35 Service Icons** (2-3 hours)
- Open DrawIO Desktop → More Shapes → GCP (mxgraph.gcp2)
- Compare each service in `gcp-icons.json` against actual available shapes
- Document mismatches and create mapping table

**B. Fix Shape Name Mismatches**
```bash
# Test each shape by generating minimal .drawio file
python scripts/test-shape-rendering.py --service cloud_run
```

**C. Add Fallback Shapes**
For services without dedicated icons:
- Use generic icons (e.g., `hexagon` for compute, `database` for storage)
- Add `fallback_shape` field to `gcp-icons.json`:
```json
{
  "service_id": "secret_manager",
  "drawio_shape": {
    "shape_name": "secret_manager",  // May not exist
    "fallback_shape": "lock",         // Generic fallback
    "fallback_library": "general"
  }
}
```

**D. Document Known Issues**
Create `assets/ICON-COMPATIBILITY.md`:
- List of verified working icons
- List of problematic icons with workarounds
- DrawIO version compatibility notes

**Files to Update:**
- `assets/gcp-icons.json` - Fix shape names, add fallbacks
- `assets/ICON-COMPATIBILITY.md` - New file documenting status
- `scripts/test-shape-rendering.py` - New script to validate icons

---

### 2. Connector Overlapping 🟡 **Medium Priority**

**Problem:** Connections overlap each other, cross through shapes, or create visual clutter.

**Current Mitigation:**
- Connection consolidation (3+ connections → 1)
- Manual waypoint specification

**Limitations:**
- No automatic routing around obstacles
- No smart layer separation
- Parallel connectors still collide

**Solutions:**

**A. Smart Waypoint Generation**
Add auto-routing algorithm that:
- Detects shape positions
- Calculates collision-free paths
- Adds waypoints to avoid obstacles

```python
# New function in diagram generator
def calculate_smart_waypoints(source_pos, target_pos, obstacles):
    """
    Returns list of waypoints to route around obstacles.
    Uses A* pathfinding on grid overlay.
    """
    path = astar_pathfind(source_pos, target_pos, obstacles)
    return simplify_waypoints(path)  # Reduce to minimal orthogonal segments
```

**B. Connection Layering**
Implement vertical separation for crossing connectors:
```xml
<!-- Layer 0: Horizontal flows -->
<mxCell id="conn1" style="...;jettySize=auto;entryY=0.5;exitY=0.5;" />

<!-- Layer 1: Vertical flows (offset 20px) -->
<mxCell id="conn2" style="...;jettySize=auto;entryY=0;exitY=1;" />
```

**C. Connection Routing Strategies**

| Strategy | Use Case | Implementation |
|----------|----------|----------------|
| **Orthogonal** | Standard (current) | `edgeStyle=orthogonalEdgeStyle` |
| **Manhattan** | Dense diagrams | Add intermediate waypoints at grid intersections |
| **Bundled** | Parallel flows | Group parallel edges with shared segments |
| **Curved** | Avoid clutter | `edgeStyle=entityRelationEdgeStyle` for crossing paths |

**D. Layout Hints in SKILL.md**
Add guidance for AI to avoid overlap:
```markdown
## Connection Routing Best Practices

When generating diagrams:
1. **Horizontal flows:** Left-to-right data flow (x-axis)
2. **Vertical flows:** Top-to-bottom hierarchy (y-axis)
3. **Crossings:** Use curved edges for non-primary paths
4. **Spacing:** Minimum 100px between parallel connectors
5. **Entry points:** Stagger entry/exit points on large containers
```

**E. Auto-Spacing Algorithm**
```python
def calculate_connector_spacing(connections_between_groups):
    """
    For N connections between two groups:
    - Offset entry points by 20px * index
    - Use different routing styles (orthogonal/curved)
    - Add labels only to primary connection
    """
    if len(connections) > 2:
        # Consolidate to max 2 connectors
        return consolidate_connections(connections)

    # Otherwise, stagger entry points
    for i, conn in enumerate(connections):
        conn.entry_offset_y = i * 20
```

**Files to Create:**
- `scripts/auto-router.py` - Pathfinding and waypoint generation
- `assets/routing-strategies.json` - Configuration for different routing patterns
- `references/connection-routing.md` - Documentation on routing algorithms

**Files to Update:**
- `SKILL.md` - Add connection routing best practices section
- `containers.json` - Add entry point configurations

---

### 3. Font Color Issue (Minor) 🟢 **Already Partially Fixed**

**Status:** Templates updated to `#424242`, but `gcp-icons.json` still has `#999999`.

**Fix:**
```bash
# Update all fontColor in gcp-icons.json
sed -i '' 's/fontColor=#999999/fontColor=#424242/g' assets/gcp-icons.json
```

---

## Enhancement Backlog (Future)

### 4. Auto-Layout Algorithm 🔵 **Future**

**Goal:** Generate optimal positions automatically from service list.

**Approaches:**
- **Force-directed:** Physics-based simulation (D3.js-style)
- **Hierarchical:** Layered top-to-bottom (Sugiyama algorithm)
- **Orthogonal:** Grid-based with minimal crossings

**Example:**
```python
def auto_layout_services(services, connections):
    """
    Input: List of services + connections
    Output: {service_id: {x, y}} position mapping
    """
    # Use graphviz dot layout, convert to DrawIO coordinates
    graph = create_directed_graph(services, connections)
    positions = graphviz_layout(graph, prog='dot')
    return scale_to_drawio_coords(positions)
```

**Complexity:** High (requires graph layout library integration)

---

### 5. Missing GCP Services 🔵 **Future**

**Current:** 35 services
**Target:** 50+ services

**High-value additions:**
- Cloud Build (CI/CD)
- Secret Manager (security)
- Artifact Registry (containers)
- Cloud Deploy (deployment)
- Certificate Manager (SSL/TLS)
- Cloud Armor (security)
- Cloud CDN (networking)
- Cloud Interconnect (hybrid cloud)
- Identity Platform (auth)
- Cloud Composer (Airflow)

**Process:**
1. Open DrawIO → GCP shapes palette
2. Add each service to `gcp-icons.json`
3. Test rendering with validation script
4. Update category counts in metadata

---

### 6. Connection Annotations 🔵 **Future**

**Goal:** Add metadata to connections beyond labels.

**Use Cases:**
- Protocol (gRPC, HTTP, WebSocket)
- Bandwidth (10GB/s, 1MB/s)
- Latency (5ms, 100ms)
- Security (encrypted, mutual TLS)
- Data format (JSON, Protobuf, Avro)

**Implementation:**
```xml
<mxCell id="conn1" value="API" style="...;labelBackgroundColor=#FFFFFF;">
  <!-- Primary label -->
  <mxGeometry relative="1" as="geometry" />
</mxCell>
<!-- Secondary annotation label -->
<mxCell id="conn1_annotation" value="gRPC • 10MB/s"
  style="text;fontSize=9;fontColor=#666;fillColor=#FFF;strokeColor=none;"
  vertex="1" parent="1">
  <mxGeometry x="250" y="195" width="80" height="20" as="geometry" />
</mxCell>
```

---

### 7. Multi-Cloud Support 🔵 **Future**

**Status:** Roadmap item

**Providers to Add:**
1. **AWS** (mxgraph.aws4.*)
2. **Azure** (mxgraph.azure.*)
3. **Kubernetes** (mxgraph.kubernetes.*)
4. **Generic cloud** (mxgraph.cloud.*)

**File Structure:**
```
assets/
├── gcp-icons.json      (existing)
├── aws-icons.json      (new)
├── azure-icons.json    (new)
├── k8s-icons.json      (new)
└── containers.json     (update with AWS VPC, Azure VNet, etc.)
```

**SKILL.md Updates:**
- Add AWS/Azure/K8s sections
- Multi-cloud diagram examples
- Hybrid cloud patterns

---

### 8. Container Auto-Sizing 🔵 **Future**

**Goal:** Automatically size containers based on children.

**Current:** Manual width/height specification
**Target:** Auto-calculate based on:
- Number of child services
- Layout algorithm (grid, flow, hierarchical)
- Padding requirements

**Algorithm:**
```python
def auto_size_container(container_id, children, layout_mode='grid'):
    """
    Calculate optimal container dimensions.
    """
    if layout_mode == 'grid':
        cols = math.ceil(math.sqrt(len(children)))
        rows = math.ceil(len(children) / cols)
        width = cols * 100 + padding.left + padding.right
        height = rows * 100 + padding.top + padding.bottom

    return {"width": width, "height": height}
```

---

### 9. Validation Enhancements 🔵 **Future**

**New Checks:**
- **Shape existence:** Warn if shape name not in DrawIO library
- **Overlap detection:** Identify overlapping shapes/connectors
- **Layout quality:** Score based on crossing minimization, alignment
- **Density warning:** Flag over-crowded diagrams (>20 shapes per 1000x1000px)

**Enhanced `validate-drawio.py`:**
```python
def validate_layout_quality(diagram):
    checks = {
        'overlapping_shapes': detect_overlaps(diagram.shapes),
        'crossing_connectors': count_crossings(diagram.connectors),
        'alignment_score': check_grid_alignment(diagram.shapes),
        'density': calculate_density(diagram),
    }
    return LayoutQualityReport(checks)
```

---

### 10. Interactive Preview Mode 🔵 **Future** (Low Priority)

**Goal:** Generate PNG preview without DrawIO Desktop.

**Approaches:**
1. **Headless DrawIO:** Use DrawIO CLI in Docker
2. **Renderer library:** Custom SVG renderer for DrawIO XML
3. **Screenshot service:** Puppeteer + DrawIO web viewer

**Complexity:** High (rendering engine required)

---

## Implementation Priority

### Phase 1: Fix Critical Issues (Week 1)
- ✅ Font colors fixed (templates updated)
- 🔴 Icon audit and fixes
- 🟡 Basic waypoint generation for overlaps

### Phase 2: Smart Routing (Week 2-3)
- 🟡 Connection layering
- 🟡 Auto-spacing for parallel connectors
- 🟡 Routing strategy documentation

### Phase 3: Missing Features (Week 4-6)
- 🔵 Add 15+ missing GCP services
- 🔵 Auto-layout algorithm (hierarchical)
- 🔵 Enhanced validation

### Phase 4: Multi-Cloud (Month 2)
- 🔵 AWS icon library
- 🔵 Azure icon library
- 🔵 Hybrid cloud examples

---

## Quick Wins (Do First)

1. **Fix `gcp-icons.json` font colors** (5 min)
   ```bash
   sed -i '' 's/fontColor=#999999/fontColor=#424242/g' assets/gcp-icons.json
   ```

2. **Create icon compatibility doc** (30 min)
   - Test top 10 most-used services in DrawIO Desktop
   - Document which render correctly

3. **Add waypoint template** (15 min)
   ```xml
   <!-- Already exists in connection-template.xml, just promote usage -->
   ```

4. **Update SKILL.md with spacing guidance** (20 min)
   - Add "Connection Spacing" subsection
   - Recommend minimum 100px between services

---

## Measurement & Success Criteria

### Icon Rendering
- **Target:** 95%+ of icons render correctly
- **Measurement:** `python scripts/test-all-icons.py` → success rate

### Connection Quality
- **Target:** <10% connector overlap in typical diagrams
- **Measurement:** Visual inspection + overlap detection script

### User Satisfaction
- **Metric:** Diagram generation success rate (AI generates valid XML on first try)
- **Target:** >90% success rate

---

## Resources Needed

### Tools
- DrawIO Desktop (for icon verification)
- Python 3.8+ with lxml, networkx (for algorithms)
- Graphviz (for auto-layout)

### Skills
- XML parsing and generation
- Graph layout algorithms (optional for auto-layout)
- DrawIO shape library knowledge

### Time Estimate
- **Phase 1:** 8-10 hours
- **Phase 2:** 15-20 hours
- **Phase 3:** 20-30 hours
- **Phase 4:** 40-60 hours

**Total for Phases 1-3:** ~50 hours (~1.5 weeks full-time)

---

## Notes

- Prioritize icon fixes first (user-facing, high impact)
- Connection routing is complex; start with simple waypoint generation
- Auto-layout is nice-to-have but not essential (users can refine manually in DrawIO)
- Multi-cloud support should wait until GCP is rock-solid
