# Interest Adventure Map — Software Architecture

## Overview

Single-file web application (`index.html`) — all logic, styles, and UI live in one file.
Data is stored separately in `data.json` but is **not loaded at runtime** — it is currently
embedded as a hardcoded `DATA` object inside the script block. The JSON file is the source of
truth for editing; see the integration workflow below.

---

## File Map

```
index.html          Main application — all JS, CSS, HTML in one file (~1100 lines)
data.json           Source-of-truth node/edge store (510 nodes, 783 edges)
DATA_SCHEMA.md      Node and edge attribute reference
PLAN.md             Original design notes (force sim, camera, BFS, navigation)
ARCHITECTURE.md     This file
/tmp/expand_network.py   Last-used integration script (adds nodes/edges to data.json)
```

---

## index.html — Section Map

All code is inside a single `<script>` block starting after the HTML body.

| Line range | Section | Key symbols |
|---|---|---|
| 6–32 | **CSS** | `#info`, `#hint`, `#vfx`, `#search-wrap`, `#legend`, `#bc`, `#zoomlabel` |
| 34–100 | **HTML** | `<canvas id="c">`, search box, info panel, hint, VFX sliders, legend |
| 82–109 | **Domain styles** | `DC` — fill/stroke/text/tag colours per domain |
| 110–120 | **Domain labels** | `DL` — human-readable name per domain key |
| 121–130 | **Zoom thresholds** | `ZOOM_THR[0..3]`, `BASE_R[1..4]`, `zoomAlpha()` |
| 131–629 | **DATA object** | Hardcoded nodes array + edges array (mirrors data.json) |
| 631–643 | **Graph build** | `ADJ` adjacency list, `NM` node map, `EMAP` edge map |
| 645–651 | **Physics init** | `phys[id] = {x, y, vx, vy, fx, fy}` seeded from `n.seed` |
| 653–657 | **BFS** | `bfsDist(from)` → `dist` map of hop counts |
| 659–735 | **Force sim** | `simStep(dist)` — spatial-grid repulsion + attraction + cluster gravity + integrate |
| 711–734 | **Async warmup** | 200 iterations spread over rAF frames (`_warmTick`) |
| 736–744 | **State** | `focused`, `camX/Y`, `zoom`, `panOffX/Y`, `orbScale`, `maxDist`, alpha tables |
| 746–762 | **Canvas / helpers** | `canvas`, `ctx`, `w2s(wx,wy)` world→screen, `drawGrid()` |
| 764–820 | **drawEdges()** | Batched by style key; viewport culled; uses `maxDist`, `EDGE_ALPHA` |
| 822–875 | **drawNode(id)** | Radial gradient orb + label + glow for focused; viewport culled; uses `maxDist`, `NODE_ALPHA` |
| 877–898 | **draw()** | `clearRect` → `drawGrid` → `drawEdges` → bucketed nodes far→near → `updateHUD` |
| 900–915 | **updateHUD()** | Updates `#ititle`, `#dtag`, `#idesc`, `#icn`, `#ires`, `#ipre`, `#bc` |
| 917–928 | **Main loop** | `loop()` — `simStep` → lerp camera → `draw()` → `requestAnimationFrame` |
| 930–940 | **Navigation** | `goTo(id)`, `goBack()`, `navDir(angle)` |
| 942–1000 | **Input handlers** | Keyboard (`keydown`), mouse (`mousedown/move/up/leave`), wheel zoom |
| 1002–1020 | **VFX sliders** | Brightness/contrast → `canvas.style.filter`; orb size → `orbScale` |
| 1022–1027 | **Depth slider** | Updates `maxDist` (1–4 hops); controls visibility radius |
| 1029–1068 | **Search** | `runSearch(q)`, keyboard nav, `'/'` shortcut to focus |

---

## Core Data Structures

```js
// Node physics
phys[id] = { x, y, vx, vy, fx, fy }

// BFS distances from focused node
dist[id] = 0..N   // 0 = focused, undefined = unreachable

// Adjacency list (bidirectional)
ADJ[id] = [id, id, ...]

// Node metadata lookup
NM[id] = { id, label, domain, level, desc, tags?, resources?, prereqs?, seed? }

// Edge lookup by pair
EMAP["a|b"] = { from, to, type, weight }
```

---

## Force Simulation

Runs every frame in `simStep(dist)`:

1. **Spatial-grid repulsion** — CELL=200 units; only checks 3×3 neighbouring cells → O(n·k)
2. **Edge attraction** — each edge pulls endpoints together proportional to `weight`
3. **Cluster gravity** — each node pulled toward its domain's `SEEDS` centroid
4. **Integration** — Euler step; damping varies by BFS distance:
   - dist ≤ 1 → damp 0.05 (nearly frozen — stable while navigating)
   - dist = 2 → damp 0.25
   - dist ≥ 3 → damp 0.86 (free-moving in the fog)

Constants: `REPULSE=20000`, `ATTRACT=0.016`, `CLUSTER=0.004`, `BOUND=900`

Domain cluster seeds (world coordinates):

| Domain | x | y |
|---|---|---|
| mechanical | −520 | 80 |
| electrical | 420 | 320 |
| it | 380 | −340 |
| bridge | 0 | 0 |
| physics | −940 | −200 |
| aerospace | −340 | −580 |
| biomedical | 800 | −80 |
| energy | 80 | 720 |
| mathematics | 80 | −720 |
| chemistry | −780 | 400 |
| systems | 200 | −520 |

---

## Rendering Pipeline (per frame)

```
clearRect
  → drawGrid()          one beginPath/stroke for all grid lines
  → drawEdges()         batched by {color|lineWidth|dash|alpha} key
  → bucket nodes        one pass over DATA.nodes → _byDist[0..maxDist]
  → drawNode() ×N       dist=maxDist first (background), dist=0 last (on top)
  → updateHUD()         DOM writes only when focused changes
```

Viewport culling: nodes/edges outside screen bounds are skipped before any canvas calls.

---

## Visibility System

```
zoom < ZOOM_THR[1] (0.45)  →  only level-1 nodes visible
zoom < ZOOM_THR[2] (1.10)  →  levels 1–2
zoom < ZOOM_THR[3] (1.90)  →  levels 1–3
zoom ≥ 1.90                →  all levels (1–4)

BFS dist > maxDist (slider) →  node/edge not rendered (default maxDist=2)
```

`zoomAlpha(level, zoom)` returns 0–1 smooth transition for each level threshold.

---

## Adding Nodes / Edges

1. Edit `data.json` (use the schema in `DATA_SCHEMA.md`)
2. Copy the new entries into the `DATA` object in `index.html` (nodes array + edges array)
3. Or run an integration script like `/tmp/expand_network.py` that writes to `data.json`, then sync back to `index.html`

> **Note:** `index.html` and `data.json` are currently kept in sync manually.
> The `DATA` object in the script (~line 131) must mirror `data.json`.

---

## UI Panels

| Element | Position | Purpose |
|---|---|---|
| `#info` | bottom-left | Focused node: title, domain tag, description, connections, resources, prereqs |
| `#bc` | top-left | Breadcrumb navigation trail |
| `#hint` | top-right | Keyboard shortcut reference |
| `#zoomlabel` | below hint | Current zoom level + active detail layer |
| `#vfx` | right-center | Display sliders: brightness, contrast, orb size, depth |
| `#search-wrap` | top-center | Search input + dropdown results (`/` to open) |
| `#legend` | bottom-right | Domain colour key |

---

## Key Constants to Tune

| Constant | Location | Effect |
|---|---|---|
| `REPULSE` | line ~660 | Node repulsion strength |
| `ATTRACT` | line ~660 | Edge attraction strength |
| `CLUSTER` | line ~660 | Pull toward domain centroid |
| `BOUND` | line ~660 | Max world coordinate before soft wall |
| `ZOOM_THR` | line ~122 | Zoom values at which each level appears |
| `BASE_R` | line ~123 | Base radius per level (px at zoom=1) |
| `NODE_ALPHA` | line ~742 | Opacity per BFS distance [dist0..dist4] |
| `EDGE_ALPHA` | line ~743 | Edge opacity per BFS distance |
