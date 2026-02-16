# DrawIO Coordinate System Guide (AWS)

## Coordinate Basics

DrawIO uses a standard screen coordinate system:
- **Origin (0,0)**: Top-left corner of the canvas
- **X-axis**: Increases to the right
- **Y-axis**: Increases downward
- **Units**: Pixels

```
(0,0) ────────────────────────► X
  │
  │     (100,50)
  │        ┌─────────┐
  │        │  Shape  │
  │        └─────────┘
  │
  ▼
  Y
```

## Shape Positioning

### mxGeometry Attributes

```xml
<mxGeometry x="100" y="200" width="64" height="64" as="geometry" />
```

| Attribute | Description |
|-----------|-------------|
| `x` | Left edge position (pixels from canvas left) |
| `y` | Top edge position (pixels from canvas top) |
| `width` | Shape width in pixels |
| `height` | Shape height in pixels |

### Position Reference Points

The position (x, y) refers to the **top-left corner** of the shape:

```
     x
     │
     ▼
y ─► ┌─────────────┐
     │             │
     │    Shape    │  height
     │             │
     └─────────────┘
           width
```

## Standard Sizes

### AWS Service Icons

| Type | Width | Height |
|------|-------|--------|
| Standard icon | 64 | 64 |

### Containers

| Type | Typical Width | Typical Height |
|------|---------------|----------------|
| AWS Cloud | 900 | 700 |
| Region | 800 | 600 |
| VPC | 700 | 500 |
| Availability Zone | 350 | 400 |
| Subnet | 300 | 250 |
| Security Group | 250 | 200 |

## Spacing Guidelines

### Between Icons

```
┌──────┐         ┌──────┐
│ Icon │◄─120px─►│ Icon │
└──────┘         └──────┘
```

**Recommended spacing:**
- Horizontal: 120px between icon edges
- Vertical: 100px between icon edges
- Minimum: 80px (tight layouts)

### Inside Containers

```
┌─────────────────────────────────────┐
│ VPC                                 │
│  ┌──────────────────────────────┐  │
│  │        Content Area          │  │ ◄─ padding-top: 50px
│  │  ┌────┐       ┌────┐        │  │
│  │  │Icon│       │Icon│        │  │
│  │  └────┘       └────┘        │  │
│  └──────────────────────────────┘  │
│ ◄─────── padding: 20px ──────────► │
└─────────────────────────────────────┘
```

**Standard padding:**
- Top: 50px (space for container label)
- Left/Right: 20px
- Bottom: 20px

## Layout Patterns

### Left-to-Right Flow

```
┌──────┐     ┌──────┐     ┌──────┐     ┌──────┐
│ In   │────►│ Svc  │────►│ Svc  │────►│ Out  │
└──────┘     └──────┘     └──────┘     └──────┘
  x=50        x=234        x=418        x=602
```

**Positions:**
- Start at x=50 or x=100
- Increment by 184px (icon width 64 + spacing 120)

### Top-to-Bottom Hierarchy

```
        ┌───────────┐
        │  Trigger  │  y=50
        └───────────┘
              │
              ▼
        ┌───────────┐
        │  Process  │  y=214
        └───────────┘
              │
              ▼
        ┌───────────┐
        │  Storage  │  y=378
        └───────────┘
```

**Positions:**
- Start at y=50 or y=100
- Increment by 164px (icon height 64 + spacing 100)

## Nested Containers

### Child Position Relativity

When a shape has a `parent` attribute pointing to a container, positions are **relative to the container's origin**:

```xml
<!-- Container at (100, 100) -->
<mxCell id="vpc" ... parent="1">
  <mxGeometry x="100" y="100" width="700" height="500" />
</mxCell>

<!-- Child at (50, 80) RELATIVE to container -->
<mxCell id="service" ... parent="vpc">
  <mxGeometry x="50" y="80" width="64" height="64" />
</mxCell>

<!-- Actual canvas position: (150, 180) -->
```

### Container Content Area

Account for container label when positioning children:

```
┌──────────────────────────────────┐ (0, 0) container origin
│ VPC                              │
│──────────────────────────────────│ y=40 (label height)
│                                  │
│   ┌────┐ (20, 60) - safe start  │
│   │Icon│                         │
│   └────┘                         │
│                                  │
└──────────────────────────────────┘
```

**Safe content area start:** (20, 50-60)

## Canvas Size

### mxGraphModel Dimensions

```xml
<mxGraphModel dx="1434" dy="844" ... pageWidth="1600" pageHeight="900">
```

### Common Page Sizes

| Name | Width | Height |
|------|-------|--------|
| Letter | 850 | 1100 |
| A4 | 827 | 1169 |
| Wide/16:9 | 1600 | 900 |
| Large | 2000 | 1500 |

## Quick Reference

### Spacing Cheat Sheet

| Relationship | Distance |
|--------------|----------|
| Icon to icon (horizontal) | 120px |
| Icon to icon (vertical) | 100px |
| Container padding (sides) | 20px |
| Container padding (top) | 50px |
| Container to container | 60-80px |

### Position Formulas

```python
# Grid position
x = startX + (col * (64 + hSpacing))
y = startY + (row * (64 + vSpacing))

# Center in container
x = (containerWidth - 64) / 2
y = labelHeight + ((containerHeight - labelHeight - 64) / 2)

# Equal distribution of N icons across width W
spacing = W / (N + 1)
positions = [spacing * i for i in range(1, N + 1)]
```
