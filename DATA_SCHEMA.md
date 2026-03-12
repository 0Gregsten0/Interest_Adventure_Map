# Node Data Schema — Interest Adventure Map

## Zoom ↔ Detail Philosophy

Zoom level maps to topic specificity:

```
zoom < 0.5   →  Level 1 only   (disciplines: "Mechanical Engineering")
zoom 0.5–1.2 →  Level 1 + 2   (fields: "PCB Design", "Lightweight Structures")
zoom 1.2–2.0 →  Level 1–3     (topics: "Signal Integrity", "Carbon Fiber Layup")
zoom > 2.0   →  Level 1–4     (subtopics: "Differential pair routing rules")
```

Nodes at higher levels start tiny and grow into full size as zoom increases past
their threshold. Nodes below the current zoom threshold fade out / shrink away.

---

## Node Attributes

```json
{
  "id":       "pcb-design",
  "label":    "PCB\nDesign",
  "domain":   "electrical",
  "level":    2,
  "desc":     "Schematic capture, layout, routing, DRC...",
  "tags":     ["hardware", "design", "manufacturing"],
  "resources": [
    { "label": "KiCad Docs", "url": "https://docs.kicad.org" }
  ],
  "prereqs":  ["elec", "analog"],
  "seed":     { "x": 400, "y": 200 }
}
```

### Required
| Attribute | Type   | Description |
|-----------|--------|-------------|
| `id`      | string | Unique slug, lowercase-hyphen |
| `label`   | string | Short display name. Use `\n` for line break (max 2 lines) |
| `domain`  | enum   | `mechanical` / `electrical` / `it` / `bridge` / `root` |
| `level`   | int    | 1–4, controls at what zoom the node becomes visible (see above) |
| `desc`    | string | 1–3 sentences for the info panel |

### Optional
| Attribute   | Type     | Description |
|-------------|----------|-------------|
| `tags`      | string[] | Keywords for future search/filter |
| `resources` | object[] | External links `{label, url}` — shown in info panel |
| `prereqs`   | string[] | Soft dependencies (nodes you should understand first) |
| `seed`      | object   | `{x, y}` hint for force-directed layout starting position |
| `icon`      | string   | Future: emoji or icon name |
| `color`     | string   | Optional hex override for stroke color |

---

## Edge Attributes

```json
{
  "from":   "pcb-design",
  "to":     "signal-integrity",
  "type":   "subtopic",
  "weight": 1.0
}
```

### Edge types
| Type           | Meaning |
|----------------|---------|
| `subtopic`     | `to` is a more specific aspect of `from` |
| `related`      | Peer concepts, neither is parent of the other |
| `prerequisite` | Understanding `from` requires `to` |
| `bridge`       | Cross-domain connection (links domains together) |

### Edge weight
- `1.0` default — normal attraction in force sim
- `2.0` strong — closely coupled topics, drawn nearer
- `0.5` weak — loosely related, can sit further apart

---

## Domain Values
| Value        | Color  | Use for |
|--------------|--------|---------|
| `mechanical` | Blue   | Physical systems, materials, manufacturing, fluids |
| `electrical` | Amber  | Circuits, power, RF, PCB, sensors |
| `it`         | Green  | Software, algorithms, networking, AI/ML |
| `bridge`     | Purple | Interdisciplinary topics spanning 2+ domains |
| `root`       | Grey   | Top-level entry points only |

---

## Level Guidelines

| Level | Zoom range | Examples |
|-------|-----------|---------|
| 1 | always visible | Mechanical Eng., Electrical Eng., IT, Robotics |
| 2 | zoom ≥ 0.5  | PCB Design, Embedded Systems, ML, Lightweight Structures |
| 3 | zoom ≥ 1.2  | Signal Integrity, Carbon Fiber, FreeRTOS, Computer Vision |
| 4 | zoom ≥ 2.0  | Diff-pair routing, T700 fiber specs, Kalman filter tuning |

Rule of thumb:
- L1 = "What field is this?"
- L2 = "What topic within that field?"
- L3 = "What technique or concept?"
- L4 = "What specific implementation detail?"

---

## Adding New Nodes — Checklist
- [ ] `id` is unique and lowercase-hyphen
- [ ] `label` fits in 2 short lines
- [ ] `level` chosen based on specificity (when should it appear?)
- [ ] `domain` reflects primary field; use `bridge` if it genuinely spans ≥2
- [ ] At least 1 edge connects it to an existing node
- [ ] `desc` is concise (2–3 sentences max)
- [ ] `seed` placed near domain cluster if you know roughly where it should sit
