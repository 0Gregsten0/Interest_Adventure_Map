# Interest Adventure Map - Build Plan

## Concept
A living 2D map. Nodes are stationary near you, but rearrange themselves
in the fog (dist ≥ 3) via a continuous force-directed simulation.
Camera follows the focused node smoothly.

## Layout — Continuous Force Simulation
- Force sim runs every frame on ALL nodes
- Nodes at dist 0-2: high damping (~0.3), nearly frozen — stable to navigate
- Nodes at dist 3+: normal damping (~0.88), still simulating — rearranging in the fog
- When you navigate toward a hidden area, those nodes "settle" into place as you approach
- Initial seed positions by domain cluster to keep things readable:
    mechanical → left, electrical → bottom-right, IT → top-right, bridge → center

## Forces
- Repulsion: all pairs (or spatial grid for perf), push apart
- Attraction: along edges, pull connected nodes together
- Cluster gravity: gentle pull toward domain centroid
- Boundary: soft wall to keep nodes from flying away

## Camera
- camX/camY lerp toward focused node world pos each frame
- render: ctx.translate(W/2 - camX*zoom, H/2 - camY*zoom)

## Visibility (BFS from focused)
- dist 0:   full bright, r=72, glow
- dist 1:   visible, r=50, alpha=1.0
- dist 2:   dimmed, r=34, alpha=0.4
- dist ≥ 3: hidden (not rendered)

## Navigation
- Arrow key → pick neighbor whose world-angle is closest to that direction
- Backspace/U: history back
- Click: focus node
- PgUp/PgDn + scroll: zoom
- Drag: pan (offset from camera track)
- Home: reset pan offset + zoom
