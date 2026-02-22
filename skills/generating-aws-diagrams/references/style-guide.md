# DrawIO Style Guide (AWS)

## Style String Format

Styles are semicolon-separated `key=value` pairs:
```
rounded=1;fillColor=#E9F3E6;strokeColor=#248814;strokeWidth=2;
```

## AWS Color Palette

### AWS Category Colors
| Category | Hex | Color Name |
|----------|-----|-----------|
| Analytics | #8C4FFF | Purple |
| Application Integration | #E7157B | Pink |
| Artificial Intelligence | #01A88D | Teal |
| Blockchain | #ED7100 | Orange |
| Business Applications | #DD344C | Red/Pink |
| Cloud Financial Management | #7AA116 | Green/Olive |
| Compute | #ED7100 | Orange |
| Containers | #ED7100 | Orange |
| Customer Enablement | #C7131F | Dark Red |
| Databases | #C925D1 | Purple |
| Developer Tools | #C925D1 | Purple |
| End User Computing | #ED7100 | Orange |
| Front-End Web & Mobile | #DD344C | Red/Pink |
| Game Tech | #ED7100 | Orange |
| Internet of Things | #3F8624 | Green |
| Management & Governance | #E7157B | Pink |
| Media Services | #ED7100 | Orange |
| Migration & Transfer | #ED7100 | Orange |
| Networking | #8C4FFF | Purple |
| Quantum Technologies | #8C4FFF | Purple |
| Robotics | #ED7100 | Orange |
| Satellite | #ED7100 | Orange |
| Security & Identity | #DD344C | Red |
| Storage | #3F8624 | Green |

### AWS Text & Line Colors
| Element | Hex | Notes |
|---------|-----|-------|
| Font Color | #232F3E | Universal dark gray for all labels |
| Arrow Stroke | #232F3E | 2pt stroke width |
| Group Border (neutral) | #879196 | Generic groups |

## Common Style Properties

### Shape Properties
| Property | Values | Description |
|----------|--------|-------------|
| `shape` | `mxgraph.aws4.*` | Shape library reference |
| `fillColor` | `#RRGGBB`, `none` | Background color |
| `strokeColor` | `#RRGGBB`, `none` | Border color |
| `strokeWidth` | `1`, `2`, `3` | Border thickness |
| `rounded` | `0`, `1` | Rounded corners |
| `dashed` | `0`, `1` | Dashed border |
| `opacity` | `0-100` | Transparency |

### Text Properties
| Property | Values | Description |
|----------|--------|-------------|
| `fontSize` | `10`, `11`, `12`, `14` | Font size |
| `fontStyle` | `0`=normal, `1`=bold, `2`=italic, `3`=bold+italic | |
| `fontColor` | `#RRGGBB` | Text color |
| `align` | `left`, `center`, `right` | Horizontal alignment |
| `verticalAlign` | `top`, `middle`, `bottom` | Vertical alignment |

### Label Position (for icons)
```
labelPosition=center;verticalLabelPosition=bottom;
```
Places label below the icon.

## Standard Style Strings

### AWS Service Icon (with colored square background)
Use when labeling the AWS service itself (e.g., "AWS Lambda", "Amazon S3").
```
outlineConnect=0;fontColor=#232F3E;gradientColor=none;fillColor={CATEGORY_COLOR};strokeColor=none;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;pointerEvents=1;shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.{SHAPE_NAME}
```

### AWS Instance Icon (bare icon, no background)
Use when representing a specific resource or instance (e.g., "Order Processor", "Users Table", "Web Server").
```
outlineConnect=0;fontColor=#232F3E;gradientColor=none;fillColor={CATEGORY_COLOR};strokeColor=none;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;pointerEvents=1;shape=mxgraph.aws4.{SHAPE_NAME}
```
The only difference: no `resourceIcon` wrapper — the shape name is used directly.

### When to Use Each
| Scenario | Icon Type | Label Example |
|----------|-----------|---------------|
| The AWS service itself | **Service** (with background) | "AWS Lambda", "Amazon RDS" |
| A specific resource/instance | **Instance** (no background) | "Order Processor", "Users Table" |
| Multiple instances of same service | **Instance** (no background) | "ECS Task 1", "ECS Task 2" |

**IMPORTANT:** `{SHAPE_NAME}` uses **underscores** for multi-word names (e.g., `kinesis_data_streams`, `step_functions`, `elastic_beanstalk`). Single-word names are unaffected (e.g., `lambda`, `ec2`, `sqs`). Look up exact names in `assets/aws-icons.json`.

**CRITICAL:** For service icons, always use `resourceIcon`/`resIcon`, NEVER `productIcon`/`prIcon`.

### AWS Arrow Style
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=open;endSize=4;
```
Open arrow, size 4, orthogonal routing, 2pt stroke.

### Dashed Arrow
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=open;endSize=4;dashed=1;
```

### Bidirectional Arrow
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=open;endSize=4;startArrow=open;startSize=4;
```

### AWS Label Rules
- Font: 12pt, color `#232F3E`
- Max 2 lines for service labels
- "AWS" or "Amazon" stays on same line as service name
- Break after 2nd word if needed
- Icon size: 64x64 fixed

## AWS Container Colors

| Container Type | Fill | Stroke |
|----------------|------|--------|
| AWS Cloud | none | #AAB7B8 |
| Region | none | #00A4A6 |
| Availability Zone | none | #00A4A6 (dashed) |
| VPC | none | #8C4FFF |
| Public Subnet | #E9F3E6 | #248814 |
| Private Subnet | #E6F2F8 | #147EBA |
| Security Group | #F2DEDE | #DD3522 |
| Auto Scaling | #FFF4E8 | #ED7100 |
| AWS Account | none | #CD2264 |
| Corp Datacenter | none | #147EBA |
| Generic Group | none | #879196 |

## Special Characters

Use XML entities in `value` attribute:
- Newline: `&#xa;`
- Ampersand: `&amp;`
- Less than: `&lt;`
- Greater than: `&gt;`
- Quote: `&quot;`

Example: `value="Amazon&#xa;API Gateway"`
