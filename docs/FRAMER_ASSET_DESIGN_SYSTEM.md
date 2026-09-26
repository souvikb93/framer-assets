# Framer embed asset — design system

Project-agnostic rules for every interactive HTML asset embedded in a Framer case-study page.
Colour, copy and data change per project. Everything below does not.

Companion file: `STREAMLINER_ASSET_GUIDELINES.md` holds the Dell/Streamliner-specific values
(palette, node ids, per-asset notes). Start a new project by copying that file, not this one.

---

## 1. Delivery contract

One asset = one self-contained `.html` file = one Framer `<Embed>` node.

    <Embed width="1fr" height="fit-content" maxWidth="1200px" type="html" radius="0px" zoom="1" html="…"/>

- **No build step, no external CSS/JS** except a pinned CDN (fonts, d3, topojson).
- **Transparent background** on `html, body` so the Framer section colour shows through.
- The pipeline is: local file → `s.replace('\n',' ')` → `html.escape(s, quote=True)` → the `html` attribute.
  Because newlines are collapsed, **never use `//` comments in the JS** — use `/* */` or none.
- Push with `updateXmlForNode`, always `zoomIntoView: false`.

### Height: Framer measures and writes back
Framer measures the embed document and writes the measured height into the node. Two consequences:

- `height="fit-content"` is the default — it lets each breakpoint measure itself.
- Pin the desktop height with a lock so the node never drifts:
  `@media (min-width:900px){body{min-height:<H>px}}`
- For a frame that must never change size at all (a card whose content swaps), pin the document too:
  `html,body{height:<H>px;overflow:hidden}` and release it in the tablet query.

**Measure at 1072px, not 1200px.** `maxWidth:1200` minus the section's 64px side padding is the real
render width on desktop. A lock derived from a 1200px measurement will clip.

### Size budget
| | max |
|---|---|
| width | 1200 (renders at 1072) |
| height | 700 |

Anything taller gets scrolled past. If content does not fit, do not shrink the type —
scroll it horizontally or stage it over time (§5).

---

## 2. Breakpoints

Framer M (810) and S (390) are **replicas** of L (1200): they inherit L's children with zero
overrides, so one `<Embed>` height is shared across all three widths, and per-breakpoint overrides
on replica children are not reachable through the MCP. All responsive behaviour therefore lives
**inside the HTML**.

Three tiers, always in this order:

    @media (min-width:900px){ body{min-height:<desktop lock>px} }   /* desktop lock   */
    @media (max-width:820px){ … }                                   /* tablet reflow  */
    @media (max-width:440px){ … }                                   /* phone density  */

Rules:
- Multi-column grids collapse to `1fr` at 820.
- Use `minmax(0,1fr)`, never `1fr`, and `min-width:0` on grid/flex children — otherwise long
  unbreakable strings force horizontal overflow.
- Wide diagrams (blueprints, journey maps, swimlanes) **scroll horizontally** below 760px
  (`overflow-x:auto` on the wrapper + `min-width:<Npx>` on the grid). Never squeeze them.
- Padding steps down: 24 → 16 → 12. Gaps 16 → 12 → 10.
- Verify at **1072 / 682 / 350** (the three real inner widths).

---

## 3. Type

Single family, variable weight, self-hosted from a pinned commit:

    @font-face{font-family:'<Family>';
     src:url(https://cdn.jsdelivr.net/gh/<owner>/<repo>@<commit-sha>/<path>.woff2) format('woff2');
     font-weight:300 700;font-style:normal;font-display:swap}

    font-family:'<Family>',-apple-system,sans-serif

Pin the commit — a bare `@main` is not reproducible. Never load Google Fonts in an embed.
SVG text needs the family named explicitly (`font-family="<Family>"`), it does not inherit.

### Scale

Measured from the published Framer page at 1200 / 810 / 390, then mapped onto Uncut Sans.
The page sets its headings in BDO Grotesk; **embeds do not use BDO.** An embed is UI sitting
inside a section, not a second voice competing with the section heading, so it stays on the
body family and starts one step below the Framer heading it sits under. What we take from
Framer is the *hierarchy and the responsive steps*, not the typeface.

Framer's own scale, for reference:

| role | 1200 | 810 | 390 |
|---|---|---|---|
| H3 | 56/64, -1.5 | 40/48, -1 | 28/36, -.5 |
| H4 | 40/48, -1 | 30/38, -.5 | 22/30, -.5 |
| H5 | 30/38, -.5 | 22/30 | 18/26 |
| H6 | 24/32 | 18/26 | 16/24 |
| body | 16/26 | 16/26 | 15/26 |
| body small | 14/21.7 | 14/21.7 | 14/21.7 |

**Embed tokens.** These are the only sizes an asset may use:

| token | ≥1024 | 768–1023 | <768 | weight | tracking |
|---|---|---|---|---|---|
| `--t-display` stat number | 40/48 | 30/38 | 22/30 | 600 | -.02em |
| `--t-title-lg` group heading inside an asset | 24/32 | 18/26 | 16/24 | 600 | -.01em |
| `--t-title` card title | 18/26 | 16/24 | 15/22 | 600 | -.01em |
| `--t-body` default | 14/21 | 14/21 | 14/21 | 400 | 0 |
| `--t-caption` supporting line | 12/20 | 12/20 | 12/20 | 400 | 0 |
| `--t-micro` chip, dense diagram cell | 10/16 | 10/16 | 10/16 | 500 | .02em |

- `--t-body` is Framer's **body small**, not its body. An embed is denser than page prose.
- **Exception — reading-heavy assets** (synthesis, decisions, anything that is mostly
  sentences rather than diagram): use Framer body exactly — 16/26, dropping to 15/26 below
  768 — so the embed does not read as fine print beside the page copy.
- Diagram-internal labels that must fit a fixed node may go to `--t-micro` and no further.
  10px is the floor.

Ship them as custom properties and redeclare in two media queries. Nothing else:

    :root{--t-display:40px;--t-title-lg:24px;--t-title:18px;--t-body:14px;--t-caption:12px;--t-micro:10px}
    @media (max-width:1023px){:root{--t-display:30px;--t-title-lg:18px;--t-title:16px}}
    @media (max-width:767px){:root{--t-display:22px;--t-title-lg:16px;--t-title:15px}}

### Weight

Three weights, no others. Framer uses 400 for body and 600 for its Uncut heading; assets match.

| weight | use |
|---|---|
| 400 | body, captions, everything by default |
| 500 | chips, micro labels, tab labels |
| 600 | card titles, stat numbers, the active tab |

**700 is not in the system.** It was in ten assets and reads heavier than anything Framer
renders. **450 and 300 are not in the system either** — 300 only ever appeared as the
`font-weight:300 700` range in `@font-face`, which is the variable axis, not a usage.

### Case

| element | case |
|---|---|
| card title, stat label, tab label | **Title Case** — matches every Framer H4/H5 on the page |
| body, captions, supporting lines | sentence case |
| acronyms | stay capitalised because they are acronyms (CRE, SODS, SFDC) |

- **Never `text-transform:uppercase`.** It reads as machine-generated.
- An asset **never prints its own eyebrow or section heading.** The Framer H4 directly above
  the embed already names the section; a second label inside the frame says it twice.
- Tracking `-.01em` on titles, default on body.
- Numbers in columns, prices and timers: `font-variant-numeric:tabular-nums`.

---

## 3b. Radius

Taken from the Framer card components on the page — 24 is the dominant surface radius there,
with 16 and 12 used inside. Five steps, and they carry hierarchy: the further in you nest, the
tighter the corner.

| token | value | use |
|---|---|---|
| `--r-surface` | 24px | the outermost frame of an asset, when it has one |
| `--r-card` | 16px | a card sitting on that surface |
| `--r-inner` | 12px | a chip, control, or card nested inside a card |
| `--r-node` | 8px | dense diagram nodes, blueprint cells |
| `--r-xs` | 4px | bars, ticks and swatches under 16px tall, where 8px would look like a lozenge |
| `--r-pill` | 999px | pills, toggles, dots |

**Exemption.** The two service blueprints (`iVhhBwFBm`, `riEAs3sap`) keep their own
8.5–9.5px type. Their swimlane geometry — lane bands, dividers, card heights — is computed
from those sizes, and restepping the text reflows the lanes. They follow the weight, case and
radius rules like everything else; only the size remap is skipped. Any future fixed-geometry
diagram gets the same treatment, and says so in a comment at the top of the file.

Before this rule the assets used sixteen different radii (2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 14,
16, 18, 20, 22, 999). Anything not on the list above is drift.

---

## 4. Colour

Four families, defined once per project:

| token | job |
|---|---|
| **brand** + 4 tints | active state, links, the one accent |
| **positive / caution / negative** + tints | outcome states only |
| **ink / ink-2 / muted** | text, three levels |
| **line-strong / line / track / canvas** | borders, rules, fills |

Rules that do not change per project:
- **Text contrast ≥ 4.5:1.** A `#A1A1AA`-class grey is 2.5:1 on white — it is a *border* colour,
  never a text colour. The lightest usable body/label grey sits around `#696970` (5.4:1).
- Colour never carries meaning alone — pair it with an icon, a label or a position.
- One accent per asset. Outcome colours are reserved for outcomes.
- **Per-entity colour** (a system, a swimlane, an owner) is a separate axis from state colour.
  Give each entity a hue, keep saturation and lightness consistent across the set, and let state
  ride on opacity / ring / shadow instead of hue.

---

## 5. Motion

Every asset that can loop, loops. The loop teaches the diagram.

### The staged-chain pattern
Do not cross-fade between finished states. Build the relationship one beat at a time:

    node A → connector A→B → node B → connector B→C → node C …

- card / node beat **500–600ms**
- connector draw **600–700ms** (`stroke-dasharray` + `stroke-dashoffset` → 0, `pathLength="100"`)
- hold at end of a chain **2000–2600ms**, then advance
- lead-in before the first beat **~360ms**

### Connectors are never idle
Paths start `opacity:0; stroke-dasharray:100 100; stroke-dashoffset:100` and are revealed only
while their step is active. A resting web of grey lines makes the card look noisy and is the single
most common reason a diagram reads as cluttered.

### Connector geometry
- Route horizontals through a **shared channel** inside the gap between rows, offset by a constant
  step (`y = rowBottom + pad + (n-1)*16`) so parallel links stay equidistant and never overlap.
- Leave a node at its centre; enter the target at `width * n/(count+1)` so multiple incoming links
  land at even fractions (⅓, ⅔).
- Same column → plain vertical. Corner radius capped at 12px.
- Keep channels clear of section labels; give the label an opaque background so a line that must
  cross reads as passing behind it.

### Controls, every time
- **hover pauses**, `mouseleave` resumes
- **click holds** that state (15–20s) then releases
- `visibilitychange` stops the timer when the tab is hidden
- state is announced in an `aria-live="polite"` status line, not only by colour

### Reduced motion
Every asset with `@keyframes` needs:

    @media (prefers-reduced-motion:reduce){ … }

and the block must **leave content visible** — an entrance animation that starts at `opacity:0`
must be forced to `opacity:1; transform:none; animation:none`, or reduced-motion users see nothing.
Infinite decorative loops (pulses) stop entirely.

Other limits: 150–300ms for micro-interactions, transform/opacity only (never width/height/top/left),
ease-out in / ease-in out, one or two animated elements per view.

---

## 6. Layout patterns

### Fixed-frame card
A card whose content swaps must not resize. Exactly **one absorber** per card:

    .absorber{flex:1;min-height:0;overflow:hidden}

Everything else is reserved: fixed `height` + `overflow:hidden` on rows, `nowrap` + ellipsis +
`title` on single-line values, `-webkit-line-clamp` on multi-line ones. Pin `html,body` as well —
pinning only the inner wrapper still lets Framer measure a taller document.

### Swimlane / blueprint
- Fixed label rail on the left (86–104px), scrolling canvas on the right.
- Lanes are bands; the standard dividers (line of interaction, of visibility, of internal
  interaction) are dashed rules that span the full canvas width, labelled in the rail.
- Cells are uniform-width cards on a fixed column pitch. One colour per system, header strip
  in the system colour, white body.
- Annotations are **callouts for the active step only**, not a permanent litter of sticky notes.

### Stepper / progress
Long sequences (20+ steps) get a progress bar plus `Step n of N`, and the viewport auto-pans to
keep the active node centred. Offer a **Fit all** toggle that scales the canvas to the frame so the
whole map can be read at once.

---

## 7. Interaction & accessibility floor

Non-negotiable in every asset:

- Real `<button>` for anything clickable — never a `div` with a click handler.
- `:focus-visible{outline:2px solid <brand>;outline-offset:2px}` on every control.
- `aria-label` on any control whose text is not visible (progress dots, icon buttons).
- Tap target ≥ 24px, and ≥ 40px at the phone tier. A control that must stay visually small
  (a 4px progress bar) gets an invisible `::before` overlay that extends the hit area without
  changing layout:

      .dot{position:relative}
      .dot::before{content:"";position:absolute;left:0;right:0;top:-12px;bottom:-12px}

- Truncated text carries a `title` with the full string.
- `cursor:pointer` on clickables; visible hover *and* active states.
- Icons are inline SVG from one family, one stroke width. Never emoji.

---

## 8. Verification before pushing

Headless Chrome, every time — do not eyeball it in the canvas:

    chrome --headless=new --disable-gpu --hide-scrollbars --allow-file-access-from-files \
           --window-size=W,H --virtual-time-budget=T --dump-dom|--screenshot

Chrome floors the window width at 500px, so **test narrow widths through an iframe harness page**
that sizes an `<iframe>` to the exact width, not through `--window-size`.

Checklist:
1. `body.scrollHeight` at 1072 **equals the desktop lock**, and ≤ 700.
2. `body.scrollWidth - W === 0` at 1072, 682 and 350 — no horizontal overflow.
3. Animated assets: dump the DOM at several `--virtual-time-budget` values and assert the step
   sequence (how many connectors are drawn, which card carries the active class).
4. Screenshot mid-sequence and late-sequence; check nothing is clipped at the frame edge.
5. Truncation probe: cycle every data state and compare `scrollWidth`/`scrollHeight` against
   `clientWidth`/`clientHeight` for each text node that could overflow.
6. Re-run 1–2 after *any* edit, including a font or colour change.

---

## 9. Framer MCP gotchas

- `duplicateNode` on a **section** appends the copy to the end of the page root, and the MCP
  **cannot reorder root children**. To place a new block in a specific section, duplicate an
  **embed inside that section** instead — the copy lands as the last child of the same parent.
- `duplicateNode` returns `[object Promise]`. Recover the new node id by re-reading the parent.
- `getNodeXml` on a large embed exceeds the token limit; the result is written to a tool-results
  file — parse it with grep/python rather than reading it back.
- A push returns the full before/after diff. Expect it, and keep payloads as small as the asset allows.
- Writes fail with `Method: setAttributes, is unavailable while the project is read-only` when the
  project is open read-only or another editor holds the lock. Nothing is partially written; re-push
  once edit access is back.

---

## 10. New-asset checklist

- [ ] Copies the project guidelines file's palette and type tokens, adds no new ones
- [ ] Pinned variable font, no Google Fonts link, SVG text names the family
- [ ] Sentence case everywhere, no `text-transform:uppercase`
- [ ] Desktop lock measured at 1072 and ≤ 700 tall
- [ ] Tablet reflow at 820, phone density at 440, horizontal scroll for wide diagrams
- [ ] Loop with staged chain; connectors hidden until their step
- [ ] Hover pause, click hold, `visibilitychange`, `aria-live` status
- [ ] `prefers-reduced-motion` block that leaves content visible
- [ ] `:focus-visible` on every control, `aria-label` where text is not visible, ≥40px targets at phone
- [ ] All text ≥ 4.5:1 contrast
- [ ] Headless checks 1–6 pass
- [ ] Source saved to `kit/<name>.html` **and** `kit/<nodeId>.html`, mirrored to the delivery folder
- [ ] Asset documented in the project guidelines file

---

## Appendix A — verification gotchas found the hard way

**`requestAnimationFrame` does not advance under headless virtual time.**
`--virtual-time-budget` drives `setTimeout` fine, but a headless page is never "visible", so rAF is
throttled to a first frame and stops. A rAF-driven animation therefore screenshots frozen at step 1
no matter how large the budget, and `--disable-background-timer-throttling` does not fix it.

Ship rAF (correct for production — it pauses when the tab is hidden), but verify through a
throwaway copy with the driver swapped:

    probe = src.replace("requestAnimationFrame(frame)}", "setTimeout(()=>frame(performance.now()),16)}")

Screenshot the probe at several budgets, ship the original. Same code path, only the clock differs.

**A scaled-up active node needs frame clearance.**
If the active state scales a node (1.3–1.5×), every node within half a scaled-height of the canvas
edge will bleed past it, and the pulse ring adds more on top. Budget
`rowY + (h/2)*scale + maxRingGrowth` against the viewBox height before choosing row positions, and
raise the `z-index`/paint order of the active node so it lifts over its neighbours instead of
being clipped by them.

**A travelling token must hide when it arrives.**
A courier dot that stays parked on the node it just reached sits on top of that node's label. Show
it during the move phase only; the node's own active state carries the "I am working" signal.

**`duplicateNode` can detach the copy from its parent.**
A duplicate that read back as the last child of its Stack was later found absolutely positioned on
the canvas (`position="absolute"`, its own `top`/`left`, fixed px width) — outside the layout
entirely. Re-read the parent after duplicating *and* after the first content push, and check the
copy still carries the parent's layout attributes (`width="1fr"`, `maxWidth`) rather than
`position="absolute"`. Passing layout attributes in the push does not re-parent it.

---

## 11. Semantic colour groups (system diagrams)

A system or architecture diagram must never colour nodes individually. Group every
node by **what kind of thing it is**, give the group one colour, and reuse that
colour everywhere the node appears — border, label, icon, pulse ring, loader arc,
travelling token, connector on approach, and the readout swatch.

### Group model

| Group | Meaning | Token |
|---|---|---|
| `mod` | The product being designed (primary) | `#0672CB` |
| `crm` | System of record / case system | `#7C3AED` |
| `ppl` | People and channels (human actors) | `#57575C` |
| `data` | Data and catalogue services | `#B02071` |
| `log` | Logistics and physical movement | `#A15C00` |
| `fin` | Finance and payments | `#0B6E4F` |

All six sit in the same blue-violet-teal band as the rest of the kit; none is a
hue the brand does not already use. Every one clears 4.5:1 on white, so the same
token can carry 12px labels — no separate "text version" of a group colour.

Contrast as shipped (on `#FFFFFF`): mod 4.62, crm 6.72, ppl 5.63, data 6.14,
log 5.74, fin 6.19. Two candidates were rejected on this test and darkened:
`#2F8F7F` → `#A15C00`, `#0E7C9B` → `#B02071`.

### Fill vs outline

Hue alone does not create hierarchy — six equally-weighted colours read as a
rainbow. Weight does:

- **Primary group only** is filled (`fill=col`, white label). Everything the
  project actually built is solid.
- **Every other group** is white-filled with a group-coloured border:
  `stroke-opacity` `.45` idle → `1` working → `.3` done.

The result: the five modules are visibly the subject, the surrounding
enterprise systems are legible context.

### Implementation

```js
const PAL={mod:'#0672CB',crm:'#7C3AED',ppl:'#57575C',data:'#B02071',log:'#A15C00',fin:'#0B6E4F'};
const GRP={ce:'mod',vr:'mod',/* … */ arb:'log',om:'fin'};
const CO=id=>PAL[GRP[id]]||DEEP;
```

One lookup, never a per-node hex. Adding a node means adding one `GRP` entry —
if the colour is wrong the group is wrong, which is the useful failure.

### Rules

1. Six groups is the ceiling. Past six, hue stops carrying meaning.
2. Group colour is **state-independent**. State changes opacity, stroke width,
   shadow and scale — never hue. A node that changed colour on activation would
   read as a different system.
3. A legend is optional only while every group is named in the running readout.
   A static export of the same diagram needs a legend.
4. Idle borders never drop below `stroke-opacity:.45` — below that the group
   colour is no longer identifiable.

---

## 12. The toggle

Every asset that switches between two or more views uses the same control. Not a
segmented track, not a row of buttons — one white capsule with a navy pill that
slides behind the active label.

```css
.tabs{position:relative;flex:none;display:flex;gap:3px;background:#fff;
 border:1px solid #D9D9DE;padding:4px;border-radius:999px;
 box-shadow:0 1px 2px rgba(18,18,18,.04)}
.tabs button{font:inherit;position:relative;z-index:1;display:flex;align-items:center;
 height:34px;padding:0 14px;border:0;background:transparent;border-radius:999px;
 color:#454548;font-size:13px;font-weight:600;cursor:pointer;transition:color .25s}
.tabs button:hover{color:#121212}
.tabs button.on{color:#fff}
.tabs button:focus-visible{outline:2px solid #0672CB;outline-offset:3px}
.tabs .pill{position:absolute;top:4px;left:4px;height:34px;border-radius:999px;
 background:#06356E;z-index:0;
 transition:transform .42s cubic-bezier(.34,1.24,.5,1),width .42s cubic-bezier(.34,1.24,.5,1)}
@media (prefers-reduced-motion:reduce){.tabs .pill{transition:none}}
```

The pill is measured from the active button, never hard-coded:

```js
function movePill(){const b=document.querySelector('#tabs button.on');if(!b)return;
 _pill.style.width=b.offsetWidth+'px';_pill.style.height=b.offsetHeight+'px';
 _pill.style.transform='translateX('+(b.offsetLeft-4)+'px) translateY('+(b.offsetTop-4)+'px)'}
```

Reading `offsetHeight` as well as `offsetWidth` matters: mobile media queries raise
the button to a 44px touch target, and a hard-coded 34px pill would sit short of it.

Three things must re-run it or the pill drifts:

1. **after the click** — `setTimeout(movePill,0)` so it lands after the existing handler
2. **`document.fonts.ready`** — the webfont changes label widths on arrival
3. **a `ResizeObserver` on the container** — catches reflow the other two miss

### The status dot

The dot is **optional and rare**. Only an asset whose toggle selects a *running*
process earns one — in this set that is the to-be system diagram alone, where the
dot blinks green (`#3DDC84`, 1.3s, opacity 1 → .4 plus an expanding glow) to say
the simulation is live.

Every other asset switches between static views. Those use the toggle with **no
dot at all** — not a grey dot, not a still dot. A dot that never changes is decoration
pretending to be status.

---

## 13. Route colour: a rule, not a choice

A route is three things at three different times, and each one has a fixed colour.

**Before it runs** — `#C6C8CD`, 1.5px. Plumbing that exists but has not fired.

**While the payload moves along it** — a gradient from the **source node's group
colour** to the **target node's group colour**, drawn in user space along the leg:

```js
const from=CO(seq[i][0]), col=CO(seq[i+1][0]);
g.setAttribute('x1',pts[0][0]); g.setAttribute('y1',pts[0][1]);
g.setAttribute('x2',pts[pts.length-1][0]); g.setAttribute('y2',pts[pts.length-1][1]);
```

So data → finance leaves magenta and arrives green; finance → Streamliner leaves
green and arrives blue. The line always *starts* in the colour of where it came
from, which is what makes direction readable without an arrowhead.

**After it completes** — green `#1E8A4C` at `.55` opacity, 2px. Same green as the
progress-rail ticks, so "done" means one thing across the whole asset.

A single `<linearGradient>` in `defs` is mutated per leg rather than one gradient
per edge — only one leg is ever in flight, and a finished leg is repainted solid
green, so nothing keeps a stale reference.

### Every node kind must have all three states

Audit finding worth recording: filled nodes had no visible `done` state. Outlined
nodes dropped `stroke-opacity` `.45 → 1 → .3` across idle/work/done, but filled
nodes were pinned at `fill-opacity:1`, so a completed module looked identical to an
active one. Fixed:

```js
if(isMod){sh.setAttribute('fill',col);
          sh.setAttribute('fill-opacity',s==='done'?.42:1);
          sh.setAttribute('stroke-opacity',s==='done'?.42:1)}
```

If a state exists, it must be legible on **every** node kind. A state that renders
on some shapes and not others is worse than no state at all, because the viewer
learns a rule that then silently fails.

---

## 14. Progress rail: one shape, three states

A step indicator has three states and **all three are pills**. The common failure
is giving only the active and completed steps a surface and leaving upcoming ones
as naked text — the row then reads as two components rather than one control with
three states.

| State | Surface | Border | Dot | Label |
|---|---|---|---|---|
| Upcoming | `#fff` | `#E9E9EC` | hollow, `#C9C9CE` ring | `#696970`, weight 500 |
| Working | current group colour | transparent | pulsing white centre | `#fff`, weight 600 |
| Done | `#E9F5EE` | `#C9E4D4` | green fill, white check | `#454548`, weight 500 |

Same height, same radius, same padding in every state — only surface, border and
dot change. Nothing moves when a step advances.

```css
.s{height:42px;padding:0 11px 0 9px;border-radius:12px;
   border:1px solid var(--line);background:#fff;
   transition:background .3s,color .3s,border-color .3s}
.s.now {color:#fff;border-color:transparent;font-weight:600}
.s.done{color:var(--ink2);background:#E9F5EE;border-color:#C9E4D4}
```

**Labels are Title Case here**, unlike body copy elsewhere in the kit. A step
indicator is a set of named checkpoints, not a sentence, and Title Case is what
separates "Exchange Approved" as a label from "Exchange approved" as a statement.
This is the one deliberate exception to the sentence-case rule in §6.

### Two traps this component hit

**A global find-and-replace on label text will silently corrupt data.** The same
string appears as both a milestone label and a step's result text. Replacing
`,'Case created','` matched the *result* field first, so results became Title Case
while the labels stayed lowercase. Rewrite the row through a regex with capture
groups and edit the field by position, never by value.

**Check which rule actually renders at the target width.** Padding was being tuned
in the base `.s` rule while a `@media (max-width:1100px)` block was the one in
force at 1072px, so two rounds of edits changed nothing. Measured width stayed
identical to the byte — that identical number is the tell.

### Correction to §14 — the rail has one colour language

The working state was painted in the *running node's* group colour, so it changed
hue step to step: blue, then magenta, then amber. Done was always green. A viewer
reading that sees two unrelated systems, and the working state looks like a
category rather than a status.

Status colour is now independent of subject colour. The rail is green throughout:

| State | Surface | Border | Dot |
|---|---|---|---|
| Upcoming | `#fff` | `#E9E9EC` | hollow, grey ring |
| Working | `#fff` | `var(--green)` + `0 0 0 3px rgba(30,138,76,.12)` | green ring, pulsing green centre |
| Done | `#E9F5EE` | `#C9E4D4` | solid green, white check |

Group colour still identifies *what* a node is, everywhere in the diagram. It must
never also encode *where the process is* — one variable, one meaning.

The rail also fills the card: `.sep` is `flex:1 1 0` with no `max-width`, so the
separators absorb all slack and the first and last pill land on the frame edges.

### Correction to §13 — a faded fill is not a done state

The `done` state for filled nodes was `fill-opacity:.42` with the label left white.
White on 42% blue over white measures **1.85:1**. It looked washed out because it
*was* illegible — well under the 4.5:1 floor, on the five nodes that matter most.

A filled node's done state is a light tint of its own colour with the label
switched to that colour, never white text on a weakened fill:

```js
sh.setAttribute('fill-opacity', s==='done' ? .06 : 1);
if(n.txt) n.txt.forEach(t=>t.setAttribute('fill', s==='done' ? col : '#fff'));
```

`.06` is not arbitrary: it is the highest tint at which **every** group colour still
clears 4.5:1 against its own tinted background. At `.08` the worst group falls to
4.41, at `.10` to 4.29. Pick the alpha from the worst case in the palette, not from
the one you happen to be looking at.

### Correction to §13 again — a tint must be opaque, not translucent

The done tint was `fill-opacity:.06`, which is not a light fill, it is a **hole**.
Routes are drawn centre-to-centre beneath the node layer, so the completed green
line ran straight through every finished pill and across its label. It looked like
the pill had been recoloured; it had actually gone transparent.

Compute the tint as a solid colour and paint it at full opacity:

```js
const MIX=(hex,a)=>{const n=parseInt(hex.slice(1),16),r=n>>16&255,g=n>>8&255,b=n&255,
 m=v=>Math.round(v*a+255*(1-a));
 return '#'+((1<<24)+(m(r)<<16)+(m(g)<<8)+m(b)).toString(16).slice(1)};
n.tint = MIX(n.col,.06);          // #0672CB -> #F0F7FC
sh.setAttribute('fill', s==='done' ? n.tint : col);
sh.setAttribute('fill-opacity', 1);
```

Rule: **opacity is for fading things out, not for making things lighter.** If the
intent is a lighter colour, compute the lighter colour. Anything behind the shape
will otherwise show through, and on a diagram there is always something behind.

Label contrast against the opaque tints: mod 4.54, log 4.78, crm 5.21, fin 5.72,
data 5.80, ppl 6.59.

### Unused routes are dimmed, not hidden

Hiding them entirely removed the ghost-rectangle problem but also removed the
system: a viewer could no longer see that the other paths exist. They now match
how unused *nodes* are treated — same `.34`, one step lighter in stroke:

```js
EL[k].setAttribute('stroke', u ? '#C6C8CD' : '#D9D9DE');
EL[k].setAttribute('opacity', u ? 1 : .34);
```

Dimming rather than dashing is what keeps the ghost boxes away — it was the dash
pattern, not the visibility, that made unused orthogonal routes read as containers.

---

## 15. Shape carries category

Colour was already doing one job (which group a node belongs to). Shape now does
another: **a human actor is a circle, a system is a pill.**

| Node | Shape | Why |
|---|---|---|
| Customer, Care agent | circle, `r:40` | people |
| Channels | tall rounded bar | a stack of contact methods, not a person |
| E-support | pill | a self-service portal, not a person |
| Everything else | pill | systems and services |

A viewer can now tell a human from a system before reading a single word, which
is the point of shape coding. It also means the two ends of the flow — the person
who asks and the person who helps — read as the same kind of thing.

### Sizing inside a circle

A circle is a worse container than a rectangle: usable width shrinks as you move
away from the centre line. At `r:40`, the chord 18px below centre is 73px wide,
which is what sets the label budget.

```js
const ic=mk('g',{transform:`translate(${n.x-11},${n.y-25}) scale(.92)`, …});
ic.innerHTML=ACT[n.ico||'user'];          // 22px glyph, 1.9 stroke
n.txt=[txt(n.x,n.y+18,n.t,11,inner,600,col)];   // 11px label, ≤73px wide
```

Icon 22px, label 11px, both centred on the vertical axis with the icon above. The
earlier attempt used a 10px label in an `r:36` circle — legible only just, and
visibly smaller than every other label in the asset. If a label has to drop below
the body size to fit its container, the container is too small; grow the container
rather than shrinking the type.

Actor icons live in one `ACT` map keyed by `n.ico`, so adding an actor is one entry
plus one node property — the renderer does not grow a special case per person.

---

## 16. One node component, three states (supersedes the fill/outline split in §11)

Every node on the diagram is now the same component. There is no filled variant
and no outlined variant, no `isMod` branch in the renderer, and no rule about
which node kinds get white labels.

| State | Fill | Stroke | Label / icon |
|---|---|---|---|
| Idle | white | group colour, 1.5px, `.45` | group colour |
| Working | white | group colour, **2px, `1.0`** + lift shadow + 1.3× + loader arc | group colour |
| Done | **6% tint of its own colour** | group colour, 1.5px, `.45` | group colour |

```js
function nodeState(id,s,p){const n=N[id],sh=n.shape,col=CO(id);
 sh.setAttribute('stroke',col);
 sh.setAttribute('stroke-width', s==='work'?2:1.5);
 sh.setAttribute('fill', s==='done' ? n.tint : '#fff');
 sh.setAttribute('fill-opacity',1);
 sh.setAttribute('stroke-opacity', s==='work'?1:.45);
 …}
```

The tint is the *only* fill on the canvas, so "has been through" is readable at a
glance across the whole diagram rather than being a property of one node family.

**What this gave up, deliberately.** The five Streamliner modules used to be solid
colour so the product read as the subject and the enterprise systems as context.
That emphasis is gone: ABACUS and Create & Edit now carry equal visual weight.
Hierarchy rests entirely on hue — blue is reserved for Streamliner and nothing
else. Chosen knowingly in favour of a single, uniform component.

**The working state is now quieter**, because fill no longer distinguishes it. It
reads through four other signals at once — 1.3× scale, lift shadow, 2px stroke at
full opacity, and the loader arc sweeping the perimeter. Four weak signals
together beat one strong one that breaks the system.

The travelling token stays solid group colour. It is the only filled shape in
motion, so the eye tracks it without competition.

**This rule stops at the diagram.** The progress rail keeps its own green state
language (§14) — status colour and subject colour are different variables and must
not be merged.

### §16 addendum — the loader fills the node, and the token changes layer

**The work state is now the done state, arriving.** A perimeter arc sweeping the
stroke said "busy" but had nothing to do with the state that followed it. The node
now fills with its own done tint, wiped in from the left, so when the wipe reaches
100% the node is already in its finished state — no snap, no second animation.

```js
const cp=mk('clipPath',{id:'cw_'+id},defs);
n.wipe=mk('rect',{x:x0,y:y0,width:0,height:h},cp);
n.fillShape= /* same geometry as the node */ {fill:n.tint,'clip-path':`url(#cw_${id})`};
// idle → width 0 | work → width * p | done → full width
```

The tint lives on a second shape clipped by a growing rect, not on the node's own
`fill`, so the outline and label never change while it fills. The clip path is in
the node's own user space, so it scales with the 1.3× transform for free.

**The token always travels under the nodes**, arriving and departing alike. The
layer order is fixed once at build time — `trail, flow, tokLayer, nodeLayer` — with
no runtime reordering.

An earlier version raised the token above the node layer past the halfway point so
it would land *on* the pill. Symmetric is better: the token belongs to the
connective layer, and a payload that ducks behind the thing it is arriving at
reads as entering it. Landing is already carried by the drop scale and the fill
that follows.

### §14 replaced — stepper on a track, labels beneath

The pill rail is gone. Steps are now markers on a connecting track with their
labels underneath, which is the conventional stepper and reads faster: the eye
follows one continuous line instead of scanning ten separate containers.

```css
.s{display:flex;flex-direction:column;align-items:center;gap:7px;padding:0 7px}
.s .dot{width:20px;height:20px;border-radius:50%;border:1.5px solid #C9C9CE;background:#fff}
.sep{flex:1 1 0;min-width:8px;height:2px;margin:9px 0 0}
```

`margin-top:9px` on the connector is `(20 − 2) / 2` — it puts the track on the
circle's centre line. Derive it from the marker size rather than nudging until it
looks right, or it breaks the moment the marker changes.

| State | Marker | Track behind it | Label |
|---|---|---|---|
| Upcoming | white, 1.5px `#C9C9CE` ring | `#D9D9DE` | `#696970`, 500 |
| Working | white, **2.5px green** ring, pulsing green centre | green | **green, 600** |
| Done | solid green, white check | green | `#454548`, 500 |

Still green throughout — status colour stays independent of the group colours in
the diagram (§16).

Dropping the pill surfaces freed horizontal space, so the labels sit directly
under their markers with no container padding, and the connectors take the slack
via `flex:1 1 0`. The rail gained 12px of height, taken back out of the viewBox
(`639 → 635`) so the asset still measures exactly 700.

### §14 addendum — the track fills, it does not switch

A connector that flips from grey to green on arrival tells you a step finished.
A connector that fills as the token travels tells you a step is *in progress*,
which is the thing a stepper exists to show.

The fill is driven from the sequence, not from the milestone that owns it. Each
connector stores the index of the milestone before it, and the fill is that span's
completion:

```js
const pos = phase==='move' ? (i-1)+q : i;          // q = t/dur
const f = clamp((pos - jp) / (j - jp));            // jp = previous milestone index
el.style.setProperty('--f',(f*100).toFixed(1)+'%');
```

This matters because milestones are sparse: between "Case Created" and "Credit
Raised" the token visits two nodes that have no marker. Keyed to the milestone
alone, the connector would sit at 0% through both and then rush; keyed to the
span, it advances the whole way.

Painted through a CSS custom property on a pseudo-element, so the connector keeps
one DOM node and the width animates without touching layout:

```css
.sep{position:relative;overflow:hidden;background:var(--line2)}
.sep::after{content:"";position:absolute;left:0;top:0;bottom:0;width:var(--f,0%);background:var(--green)}
```

`paintSeps()` runs every frame, and must sit **after** the phase `if/else` chain
in `frame()` — dropping it between two `else if` branches is a syntax error that
kills the whole script.

### The opening beat

The customer no longer plays a work phase at the start. The sequence opens with
the token already leaving: `phase='done'` with a 0.45s hold, so the first thing
that moves is the request itself rather than the actor sending it.
