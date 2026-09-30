# Building an animated system diagram

How `streamliner/OX6dWZqL5.html` is built, written so a second one can be built
without reverse-engineering the first. Tokens, colour and the delivery contract
live in [`FRAMER_ASSET_DESIGN_SYSTEM.md`](FRAMER_ASSET_DESIGN_SYSTEM.md); this
file is only the parts specific to *a request moving through a system*.

**Live:** https://souvikb93.github.io/framer-assets/streamliner/OX6dWZqL5.html

---

## 1. What this asset type is for

One request type travels a fixed route through named systems, and the reader
watches it happen. It answers *what touches what, in what order* — not *how many*
and not *what it looks like*. If the answer is a count, build a chart. If the
route never changes and nothing moves, build a static SVG; the animation is only
worth its complexity when the **sequence** is the point.

It is a poor fit when there are more than ~25 nodes, when routes branch
conditionally, or when the reader needs to compare two routes side by side.

---

## 2. Everything is data, nothing is drawn by hand

Five objects. Change these and the diagram redraws; never edit the SVG.

```js
const N    = { id: {x, y, w, h | r, t: 'Label', k: 'm'|'s'|'p'} }  // nodes
const GRP  = { id: 'mod'|'crm'|'ppl'|'data'|'log'|'fin' }          // semantic group
const E    = { 'from-to': [ [x,y], … ] }                           // edge waypoints
const F    = { flow: [ [nodeId, working, done, milestone, icon] ] } // the sequences
const HOPS = { 'from-to': [x,y] }                                  // where a line hops another
```

`k` is the node kind: `m` module, `s` system, `p` person or channel. It drives
shape, not colour — see §5.

A flow step is five fields and every one earns its place:

| field | shows in | example |
|---|---|---|
| node id | which node lights | `'vr'` |
| working text | readout while it runs | `Business rules: policy period, CRE history` |
| done text | readout when it finishes | `Approved` |
| milestone | the progress rail, or `''` to omit | `Rules check` |
| icon key | what the token carries | `ok` |

Only steps with a milestone appear on the rail. That is how 18–28 steps compress
to 7–10 rail stops without a second data structure.

---

## 3. Geometry

- **viewBox `0 0 1372 635`.** Fixed. Nothing is positioned in CSS pixels, so the
  whole board scales with the frame and the lock never changes.
- **`M = 362`** is the main row. The happy path runs along it left to right;
  everything above is upstream/finance, everything below is logistics/data.
- **`W = 124`**, node height 38, circles `r = 40`.
- **Equal edge gaps, not equal centres.** Nodes differ in width, so spacing by
  centre leaves visually uneven gaps. Space by the gap between edges — in this
  board, 77 units.
- **Anchors, never raw coordinates.** `P(id, 't'|'r'|'b'|'l')` returns the point
  on a node's edge. Every waypoint list starts and ends with one, so moving a
  node moves its lines with it.
- **`RET`** is the shared return corridor along the bottom. Flows that end back
  at the customer reuse it instead of each drawing their own long way home.

---

## 4. Edge routing

`rpath(points, hop, r = 18)` turns waypoints into a path with rounded corners.
Two rules do the work:

- **Corner radius is clamped** to half the shorter adjoining segment, so a tight
  dogleg rounds less rather than overshooting into a loop.
- **Crossings hop.** Where a horizontal line must cross another, `HOPS` names the
  point and the path draws a 7-unit arc over it. A line that crosses without a
  hop reads as a junction that does not exist.

Draw order is fixed and matters: **edges → trail → flow → nodes → token**. Nodes
sit above lines so a connector ends at an edge instead of running across a label;
the token sits above everything because it is the thing being followed.

---

## 5. Colour

Palette and the opaque-tint rule are in the design system (§11–§13). What is
specific here:

```js
const PAL = {mod:'#0672CB', crm:'#7C3AED', ppl:'#57575C',
             data:'#B02071', log:'#A15C00', fin:'#0B6E4F'};
const CO  = id => PAL[GRP[id]] || DEEP;
```

- **Colour carries the group, shape carries the kind.** Six groups, each ≥4.5:1
  on white, minimum 46° apart in hue. A reader learns six colours once.
- **The moving token takes the colour of the node it is travelling *to*.** The
  line ahead is the destination's colour, so the eye is pulled forward rather
  than trailing what already happened.
- **Unused routes dim, they do not disappear.** `focusRoute()` sets used edges to
  `#C6C8CD` at full opacity and everything else to `#D9D9DE` at `.34`. Hiding
  them would imply the connection does not exist.

---

## 6. Node states

Three states, and **every node kind must implement all three** — that is the bug
this design kept producing. `nodeState(id, state, progress)`:

| state | stroke | width | fill |
|---|---|---|---|
| idle | group colour at `.45` | 1.4 | white |
| work | group colour at 1 | 1.4 → 2.2 as it fills | white + a wipe |
| done | group colour | 2.2 | white |

The working node also **scales to 1.3** via a spring, not a CSS transition:

```js
const k = 150, c = 2 * Math.sqrt(k) * 0.78;   // critically damped, slightly under
n.vel += ((n.target - n.sc) * k - n.vel * c) * dt;
n.sc  += n.vel * dt;
```

A spring settles; an ease-out with a fixed duration fights the next state change
if steps land close together.

---

## 7. The token and its phases

One state machine, five phases, driven by `requestAnimationFrame`:

```
move  → land → work → done → next   … and at the end, end → start(next flow)
```

| constant | value | what it governs |
|---|---|---|
| `SPEED` | 104 units/s | travel speed; duration is derived from path length, never fixed |
| `WORK` | 2.5s | a node processing |
| `WORK_KEY` | 3.1s | a node whose step is a milestone — held longer |
| `HOLD` | 1.5s | the beat after a node finishes |
| `END` | 3.6s | the pause before the next flow starts |

Deriving move duration from `pathL / SPEED` is what keeps a short hop and a long
return trip feeling like the same object travelling, rather than every segment taking
the same time regardless of distance.

`ease` is `easeInOutCubic`. The trail draws with `stroke-dasharray` tied to
`getTotalLength()`, and a second dashed path runs under the token to suggest
flow direction.

---

## 8. Progress rail

Built from the milestone steps only. Three states — done, now, ahead — and the
separators fill between them. The full component is §14 of the design system,
including the two traps it hit. One rule bears repeating here: **the rail has one
colour language.** It is progress, not category; do not colour rail stops by
group.

---

## 9. Checklist for a new one

1. List the nodes and group them. If you cannot assign a group, the group model
   is wrong, not the node.
2. Lay out the main row at `M`, upstream above, downstream below. Equal edge gaps.
3. Write `E` using anchors only. Add `HOPS` wherever two lines cross.
4. Write one flow end to end. Get it moving before writing the second.
5. Mark milestones — aim for 7–10 across the whole flow.
6. Add the remaining flows. They will reuse most edges; add only what is missing.
7. Run the verification loop in §8 of the design system: `node --check`, height
   and overflow at 1072 / 682 / 350, contrast on every label pair, and a
   timer-driven probe for animation state.

---

## 10. Pitfalls this asset actually hit

- **`//` comments die.** If the file is ever inlined into a Framer `html`
  attribute, newlines collapse and a `//` comment eats the rest of the script.
  Use `/* */`.
- **rAF does not advance under headless virtual time.** A probe copy must swap
  `requestAnimationFrame(frame)` for `setTimeout(() => frame(performance.now()), 16)`
  or the asset cannot be verified in CI.
- **A translucent tint is a hole.** `fill-opacity` on a "done" node let the
  completed route show through the label. Compute an opaque tint with `MIX()`.
- **Opacity fades, it does not lighten.** A faded fill is not a done state; it
  reads as disabled.
- **Duplicate ids.** Generated element ids must come from one counter. Ids built
  from nested loop indices collided, and `getElementById` silently returned the
  first — which looked like an interaction bug, not an id bug.
