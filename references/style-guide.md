# DrawIO Style Guide

## Style String Format

Styles are semicolon-separated `key=value` pairs:
```
rounded=1;fillColor=#E8F5E9;strokeColor=#4CAF50;strokeWidth=2;
```

## GCP Color Palette

| Color | Hex | Use |
|-------|-----|-----|
| Google Blue | #4285F4 | Primary icons |
| Google Red | #EA4335 | Errors, alerts |
| Google Yellow | #FBBC04 | Warnings |
| Google Green | #34A853 | Success, VPC |
| Apigee Orange | #FF6D00 | Apigee |
| Firebase Yellow | #FFCA28 | Firestore |

## Common Style Properties

### Shape Properties
| Property | Values | Description |
|----------|--------|-------------|
| `shape` | `mxgraph.gcp2.*` | Shape library reference |
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

### GCP Service Icon
```
sketch=0;html=1;fillColor=#4285F4;strokeColor=none;verticalAlign=top;labelPosition=center;verticalLabelPosition=bottom;align=center;spacingTop=-6;fontSize=11;fontStyle=0;fontColor=#424242;shape=mxgraph.gcp2.{service}
```

### VPC-SC Container
```
rounded=1;whiteSpace=wrap;html=1;fillColor=#E8F5E9;strokeColor=#4CAF50;strokeWidth=2;verticalAlign=top;fontSize=14;fontStyle=1;align=center;container=1;collapsible=0;recursiveResize=0;
```

### Dashed Logical Group
```
rounded=1;whiteSpace=wrap;html=1;fillColor=none;strokeColor=#757575;strokeWidth=1;dashed=1;dashPattern=5 5;verticalAlign=top;fontSize=11;fontStyle=2;align=center;container=1;collapsible=0;recursiveResize=0;
```

### Solid Arrow Connection
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;endArrow=classic;endFill=1;
```

### Bidirectional Arrow
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#333333;strokeWidth=1;startArrow=classic;startFill=1;endArrow=classic;endFill=1;
```

### Dashed Arrow
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#666666;strokeWidth=1;dashed=1;endArrow=classic;endFill=1;
```

## GCP Container Colors

| Container Type | Fill | Stroke |
|----------------|------|--------|
| GCP Project | #F1F8E9 | #558B2F |
| VPC-SC | #E8F5E9 | #4CAF50 |
| Region | #E3F2FD | #1976D2 |
| Zone | #FFF3E0 | #FF9800 |
| Subnet | #E8EAF6 | #3F51B5 |
| Security | #FFEBEE | #F44336 |
| External | #F3E5F5 | #9C27B0 |

---

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
| Customer Enablement | #C7131F | Magenta/Red |
| Databases | #C925D1 | Purple |
| Developer Tools | #C925D1 | Purple |
| Management & Governance | #E7157B | Pink |
| Networking | #8C4FFF | Purple |
| Security & Identity | #DD344C | Red |
| Serverless | #ED7100 | Orange |
| Storage | #3F8624 | Green |

### AWS Text & Line Colors
| Element | Hex | Notes |
|---------|-----|-------|
| Font Color | #232F3E | Universal dark gray for all labels |
| Arrow Stroke | #232F3E | 2pt stroke width |
| Group Border (neutral) | #879196 | Generic groups |

### AWS Service Icon Style Pattern
```
outlineConnect=0;fontColor=#232F3E;gradientColor=none;fillColor={CATEGORY_COLOR};strokeColor=none;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;pointerEvents=1;shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.{SHAPE_NAME}
```
**IMPORTANT:** `{SHAPE_NAME}` uses **spaces** not underscores (e.g., `kinesis data streams`, `step functions`, `elastic beanstalk`). Single-word names are unaffected (e.g., `lambda`, `ec2`, `sqs`). Look up exact names in `assets/aws-icons.json` or `.archive/aws4-available-shapes.txt`.

### AWS Arrow Style
```
edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=open;endSize=4;
```
Open arrow, size 4, orthogonal routing, 2pt stroke.

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

Example: `value="Google Cloud Platform&#xa;VPC-SC"`
