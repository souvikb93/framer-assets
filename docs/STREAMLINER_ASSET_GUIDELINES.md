# Streamliner case study — asset guidelines

Every visual on the Framer page `/projects/streamliner` (HTML embeds, diagrams, charts, cards) follows this kit, so the case study reads as one project.

## 1. Colour tokens

Use only these. Each hue has a five-step ramp: text/solid → mid → line → soft fill → tint.

| Role | Solid / text | Mid | Line | Soft fill | Tint |
|---|---|---|---|---|---|
| Brand / active (Dell blue) | `#0672CB` | `#7DB9EA` | `#C8E3F7` | `#E3F0FA` | `#F2F8FD` |

Dell blue replaces the earlier navy everywhere. Use `#0672CB` for fills, strokes, the token and selected states. Use **`#0063B8` for blue text** (on white or on blue tints: 4.6:1 or better). On a Dell-blue fill, text is white; secondary text is `#F2F8FD`.
| Success / done | `#1E8A4C` | `#9ED9B0` | `#D6F2E1` | — | `#F2FAF5` |
| Warning / regional | `#A15C00` | `#F5A524` | `#F5C97A` | `#FFF1DB` | `#FFF9EE` |
| Danger / reject | `#C62828` | `#F2A7A7` | `#FBDADA` | — | `#FDF5F5` |

Neutrals:

| Token | Hex | Use |
|---|---|---|
| ink | `#121212` | Headings and primary text. Never used as a fill. |
| ink-2 | `#454548` | Secondary text and icons |
| muted | `#696970` | Body copy, captions, caps labels (4.5:1 on white) |
| subtle | `#A1A1AA` | Disabled text and placeholder bars |
| line-strong | `#D4D4D8` | Idle node outlines, unused lines |
| line | `#E5E5E5` | Card borders, dividers |
| track | `#F0F1F4` | Tab tracks, progress tracks, chips |
| canvas | `#F4F6FA` | Diagram and map backgrounds, image wells |
| surface | `#FFFFFF` | Cards and nodes |

Map only: the region pins may use a categorical set (Dell blue, `#2E7DDB`, `#7DB9EA`, green, amber, red, `#7C3AED`). Nothing else uses categorical colour.

### State semantics (same in every asset)

- **Dell blue = active or selected**: the current step, a selected tab, a highlighted stat, the loader.
- **Green = done or approved**: finished steps, approved outcomes, positive results.
- **Grey = not yet or not relevant**: upcoming steps are grey outlines. Items outside the current flow are faded to 55%.
- **Amber** = regional or configurable variation. **Red** = rejected or blocked.
- Never rely on colour alone. Always pair it with a label, an icon or a tick.

## 2. Typography

The only font is Inter (400, 500, 600, 700).

| Style | Size / weight | Notes |
|---|---|---|
| Stat number | 40–44 / 700 | Tracking -0.03em |
| Card title (H4) | 15–19 / 600 | Tracking -0.01em |
| Status title | 18 / 600 | e.g. the diagram's "what is happening" line |
| Body | 14–14.5 / 400 | Line-height 1.55, colour: muted |
| Small | 12–12.5 / 500 | Chips, captions |
| Caps label | 12 / 600 | Uppercase, tracking 0.12em, colour: muted (or Dell blue when it names the active item) |

Diagram node labels are 12 / 600 ink.

## 3. Shape and spacing

- **Radius**: cards 20px; inner items 12px; chips 8px; tabs and pills 999px; diagram nodes 10px.
- **Borders**: 1px `line` on cards; 1.5px strokes on diagram nodes and lines.
- **Spacing**: multiples of 4; card padding 18–26px; grid gaps 12–20px.
- **Shadows**: none on cards. Only the selected tab gets `0 1px 3px rgba(11,59,140,.3)`.

## 4. Components

- **Tabs / toggle**: track `#F0F1F4`, 3px padding, 999px radius. Items are 13px / 600. Selected: Dell blue fill, white text. Always one colour, never a colour per option.
- **Progress stepper**: done = green dot with a white tick and green connector. Current = Dell blue pill with white text and a pulsing dot. Upcoming = white dot with grey outline. When there isn't room, only the current step keeps its label.
- **Cards**: white, 1px line border, 20px radius, no shadow. A highlighted card is a Dell blue fill with white and `#C8E3F7` text.
- **Chips / tags**: soft fill plus solid text of the same hue (e.g. `#C8E3F7` / `#0672CB`), 8px radius.
- **Buttons**: text only, no icons in CTAs. Icon-only controls need an `aria-label`.

## 5. Diagrams and animation

- Lines are orthogonal with 16px rounded corners. They meet a node at the centre of a side. There are no junction dots; branches curve off smoothly. Where two lines must cross, draw a small hop.
- Lines in the current flow are solid `#A1A1AA`. Unused lines are dashed `#D4D4D8`. A travelled line turns green.
- The moving token is a Dell blue circle with a white stroke. It travels under the nodes, along the lines, through node centres, and never jumps.
- The working state is a light-blue fill plus a Dell blue outline that fills from left to right as the loader. Done is a green outline and fill, with no tick inside the node.
- Timing: each system works for 2.6–3.2s, holds 1.6s, and handovers take at least 1.6s. Leave time to read.
- Respect `prefers-reduced-motion`.

## 6. Framer embed rules

- Start every embed's CSS with `html,body{width:100%}body{display:block!important}body>div{width:100%}`. Framer centres and shrinks embed content otherwise.
- Keep the background transparent. The page supplies white.
- Max width 1200px. Set the embed height so the whole asset fits one viewport.
- No `//` comments in scripts: newlines are collapsed when the HTML is pushed, which breaks them.
- Source files live in the session scratchpad `kit/` folder, one file per embed.

## 7. Embed inventory (desktop, page `LIdyc6Q9n`)

| Section | Embed node | Source |
|---|---|---|
| Overview stats | `CLTQi8f4o` | kit/overview.html |
| Problem | `k3rxHcFvx` | kit/problem.html |
| Process | `hCEOXL0l5` | kit/process.html |
| Research map | `TKuq5vJQK` | kit/map.html |
| 17 → 7 → 3 → 1 funnel | `gdBqnnNCX` | kit/funnel.html |
| As-is customer journey (replaced Wix iframe) | `cJPS6EXdE` | kit/journey.html |
| Synthesis: findings → insights → opportunities | `k567uewrD` | kit/synthesis.html |
| Design response per opportunity (section `mODXqWS9E`) | `LhtrzwMXl` | kit/response.html |
| Co-creation workshops (in section `QeGM9YKIQ`) | `i0e_jTpCf` | kit/cocreate.html |
| Five modules | `KhZOFyrxi` | kit/modules.html |
| One core, local rules | `rDjTt_Ep8` | kit/adaptive.html |
| To-be system (animated) | `OX6dWZqL5` | kit/system.html |
| Verify & Review rules | `CN6IB8Tax` | kit/rules.html |
| Decisions | `irYUR150B` | kit/decisions.html |
| Impact | `zX_dFoAE6` | kit/impact.html |


New embeds start from `_base.css`: every `*.src.html` has `{{BASE}}` substituted into it to build the `.html`.

### Journey-map patterns
- Action cards are white.
- Thought cards use the warning tint (`#FFF9EE` / `#F5C97A`).
- Opportunity cards use the Dell-blue tint.
- Touchpoints are pill chips with stroke SVG icons, never emoji.
- Feeling is a dashed curve. Its dots and labels are green for positive, grey for neutral and red for negative.
- Detail opens in a kit modal: radius 20, a close button and Esc to dismiss.

## Fixed-frame pattern (world map, TKuq5vJQK)

Data-driven embeds must never resize when the selected record changes. The rule:

1. Outer `.wrap` gets an explicit `height`; children use `height:100%;min-height:0` (never `min-height:Npx`).
2. Every block with variable text is either **reserved** (fixed `height` + `overflow:hidden`) or **absorbing** (`flex:1;min-height:0;overflow:hidden`). Exactly one absorber per card.
3. Single-line values: `white-space:nowrap;overflow:hidden;text-overflow:ellipsis` + a `title` attribute with the full string.
4. Multi-line values: `-webkit-line-clamp` with a matching `-webkit-box-orient:vertical`.
5. Below the stacking breakpoint, release the locks: `height:auto` on wrap/side/card, `flex:none` on the absorber.
6. Set the Framer `<Embed height>` to `wrap height + wrap padding*2`. Never leave it `fit-content` — an HTML embed collapses.
7. **Pin `html,body` too.** Framer measures the embed document and writes the measured value back into the node height (observed: 460px silently became 566px). A fixed `.wrap` is not enough — set `html,body{height:<embed height>;overflow:hidden}` and release it in the stacked media query.

Verify with a headless probe that forces every record and compares `getBoundingClientRect().height` and `scrollHeight`; all records must report identical numbers and `scrollHeight <= clientHeight`.

## Responsive rules (all 15 embeds, 2026-09-22)

The page has three Framer breakpoints: `L` 1200 (`LIdyc6Q9n`), `M` 810 (`NlPNOzxrK`), `S` 390 (`Sk8va5bUZ`). M and S are **replicas** — they inherit L's children and hold no overrides, so an `<Embed>` has ONE height shared by all three widths. A fixed px height therefore clips tablet and phone.

Rules now applied to every embed:

1. **`height="fit-content"` on the node.** Framer measures the iframe per breakpoint, so each width gets its own height. (`k567uewrD` had been on fit-content for weeks — it works; the earlier "embeds collapse" note was wrong.)
2. **Desktop lock inside the embed:** `@media (min-width:900px){body{min-height:<old px height>}}`. Desktop content then measures exactly what the fixed height used to be, so switching to fit-content is pixel-neutral. Measure at the REAL embed width (1072 at L after the section's 64px gutters), not 1200.
3. **Phone tier** `@media (max-width:440px)`: padding ≥24→16, 20–23→14, 16–19→12, 12–15→10; gap ≥28→16, 24–27→14, 16–23→12, 12–15→10; display numbers ≥30px→26px.
4. **Wide diagrams scroll, never squeeze.** Journey (`cJPS6EXdE`) and system (`OX6dWZqL5`) get `overflow-x:auto` on the scroll parent plus `min-width:860px` / `900px` on the diagram below 760px. Squeezing truncated every journey card to "C..".
5. **Grid overflow:** use `minmax(0,1fr)` not `1fr`, and drop `white-space:nowrap` below 760px, or min-content forces horizontal scroll (hit `zX_dFoAE6` and `k567uewrD`).

Verify with an iframe harness at 1072 / 682 / 350 / 262 comparing `body.scrollHeight` and `body.scrollWidth`; `scrollWidth` must never exceed the iframe width.

**Still owner-only:** the section padding is `64px` on every breakpoint, leaving 262px of usable width on a 390px phone. Per-breakpoint overrides on replica children are not reachable through the unframer MCP — set them on the M/S canvas (suggest 32px tablet, 20px phone).


## Synthesis + method assets (2026-09-23)

**Synthesis `k567uewrD`** rebuilt to 644px (cap was 700). Decluttered: dropped the F0x chips on insight cards (the lines carry linkage now), findings trimmed to title + 2-line clamp, opportunity cards start white with a blue tint and only go solid `#0672CB` when selected. Band gaps 44 -> 56px so the connectors have room; card padding 12/14 -> 14/16; row gap 12 -> 14.

Behaviour: auto-cycles the three insight chains at 3.6s, hover anywhere pauses (status line says so), clicking any card selects it and holds 20s. Selection dims everything off-chain to .38 and draws the hot paths with a `stroke-dashoffset` 100 -> 0 sweep (every path carries `pathLength=100`). Finding -> insight edges fan out by source index so parallel runs do not overlap.

Evidence chains: I01 <- F01,F03 / I02 <- F01,F04 / I03 <- F02,F04, each insight feeding its matching opportunity.

**Method `jDPWO0srd`** is a new embed appended inside the Discovery section `k6Q8TTr7L`, after the world map and the 17->7->3->1 funnel. 605px. Covers the research protocol: 4 phases (scope / guide / interviews / playback), a 60-minute session broken into 5+25+18+12 minute segments with a colour key, and where each artefact lived.

**Node ordering warning:** `duplicateNode` on a SECTION appends the copy to the END of the page root, not next to the original, and the MCP cannot reorder root children. To add a block in a specific section, duplicate an EMBED inside that section instead - the copy lands as the last child of that same section - then rewrite its html.

## Typeface: Uncut Sans variable (2026-09-23)

Every asset in this project now loads the **Uncut Sans variable** cut, self-hosted from the official repo via jsDelivr and pinned to a commit:

    @font-face{font-family:'Uncut Sans';
     src:url(https://cdn.jsdelivr.net/gh/kaspernordkvist/uncut_sans@b3b42467781e3bd98c68f2d70eba325196e7d9c5/Webfonts/UncutSans-Variable.woff2) format('woff2');
     font-weight:300 700;font-style:normal;font-display:swap}

Stack is `font-family:'Uncut Sans',-apple-system,sans-serif`. The Google Fonts Inter `<link>` was removed from every embed, and SVG text uses `font-family="Uncut Sans"` (journey feeling labels) and `.lab{font-family:'Uncut Sans'}` (system diagram).

**Do not use `@fontsource/uncut-sans`** — that package ships static cuts only, no variable file. The variable woff2 (78 KB, verified `wOF2` magic, served as `font/woff2`) exists only in `kaspernordkvist/uncut_sans` at `Webfonts/UncutSans-Variable.woff2`. Pin the commit; a bare `@main` is not reproducible.

Re-measured every asset at 1072 / 682 / 350 after the swap: no clipping, no horizontal overflow, every desktop height still equal to its lock. The world map's fixed 34px fact rows and 62px header survive the wider face — a 7-region truncation probe reported NO TRUNCATION.

## As-is service blueprint (riEAs3sap, 2026-09-23)

New interactive asset, 560px, in the "Making the Research Explorable" section right after the journey map. Returns methods, USA.

Six phases (request, method, hand over, transit, inspection, refund) by five blueprint layers, with the three standard dividers drawn as dashed rules: **line of interaction**, **line of visibility**, **line of internal interaction**.

Layer colour coding: evidence `#F4F6FA`, customer actions white, frontstage `#F2F8FD`/`#C8E3F7`, backstage white/`#D4D4D8`, support systems `#FFF9EE`/`#F5C97A`. Same stepped `#E3F0FA` phase header, same 150px grey label column and the same click-to-open modal as the journey map. Below 760px the grid scrolls horizontally at `min-width:940px` rather than squeezing.

## No all-caps labels (2026-09-23)

`text-transform:uppercase` is banned across every asset in this project — it reads as machine-generated. All 19 occurrences were removed (`.label`, `.eyebrow`, `.band em`, `.c em`, `.hd em`, `.col em`, `.box em`, `.rule em`, `.insight em`, `.ln em`, `.tag`, `.o em`). Where a rule had tiny-caps tracking, `letter-spacing` was reduced to `.02em` and any `10`/`10.5px` size bumped to `11px`, so sentence case still reads as a label rather than body copy.

Two hard-coded strings were rewritten too: the modules band `STREAMLINER · CRE LIFECYCLE` → `Streamliner · CRE lifecycle`, and the rules-engine source chip `RULES` → `Rules`. Genuine acronyms stay capitalised: CRE, SODS, RIMS, PDSL, SFDC, OMEGA, ABACUS, OTM, ARB, HMW, ANZ, USA.

Sentence case is narrower than uppercase, so nothing re-clipped: every desktop height still equals its lock and there is no horizontal overflow at 350px.

## Synthesis connectors: built, never idle (k567uewrD, 2026-09-23)

The connectors are **not drawn at rest**. Every `path` starts at `opacity:0` with `stroke-dasharray:100 100; stroke-dashoffset:100`; only `.hot` reveals it and animates `stroke-dashoffset` to 0 over `.6s`. Nothing else in the card carries a resting line, so the stage is quiet between cycles.

Each cycle plays one evidence chain as a sequence, not a state flip:

    finding A → line A→insight → finding B → line B→insight
      → insight (highlight + scale pop) → line insight→opportunity → opportunity

Timing: 560ms per card step, 680ms per link step, 2400ms hold at the end, ~360ms lead-in. The insight uses a `pop` keyframe (1 → 1.1 → 1.045) and stays at 1.045 while it is the active card; `prefers-reduced-motion` and every width ≤820px drop the transform entirely.

Line geometry is uniform. Horizontal channels sit at `y1 + 26 + (u-1)*16` — inside the 70px `.gap`, above the section band, 16px apart, identical for every group. Each link leaves its source at the card's horizontal centre and enters its target at `width * u/(c+1)`, so two incoming links land at exactly ⅓ and ⅔. Insight→opportunity share a column, so `curve()` returns a plain vertical. Corner radius is capped at 12px.

The `.gap` is 70px (was 56px) so the stage fills its 644px lock instead of leaving 35px of slack, and the channels get clearance from both rows.

Hover pauses, a click freezes the whole chain for that card (a finding reveals every insight it feeds, plus their opportunities) and releases after 20s.

## To-be system diagram — group palette (OX6dWZqL5)

Nodes are coloured by group, not individually. `CO(id)` resolves every colour.

| Group | Nodes | Hex | Contrast on white |
|---|---|---|---|
| Streamliner modules | Create & Edit, Verify & Review, Process & Track, Return Method, Disposition & Recovery | `#0672CB` | 4.62 |
| Case system | SFDC | `#7C3AED` | 6.72 |
| People & channels | Customer, Channels, Care Agent, E-support | `#57575C` | 5.63 |
| Data services | BIL / SODS, PDSL, Delta · RIMS | `#B02071` | 6.14 |
| Logistics | ARB facility, Logistics, OTM | `#A15C00` | 5.74 |
| Finance | ABACUS, OMEGA, Oracle payments | `#0B6E4F` | 6.19 |

The five modules are the only **filled** nodes — that is what makes them read as
the product. Everything else is a white card with a group-coloured border.

Progress rail ticks are `#1E8A4C` with a white check; completed separators use
the same green. The active node scales to 1.5× on a spring and carries a pulse
ring in its own group colour.

Full rationale: `FRAMER_ASSET_DESIGN_SYSTEM.md` §11.

### Legend (top-left of the canvas)

Two columns inside one 228×204 card at `x:34 y:36`:

- **System group** — colour swatch + name, one row per group, swatch fill is the
  group token itself (no separate legend palette).
- **What moves** — the payload icon carried by the travelling token, so the icon
  in the circle is readable without watching a full cycle:

  | Icon | Meaning | Used by |
  |---|---|---|
  | document | Request | case detail moving into Streamliner |
  | check | Approval | rules-engine verdict moving downstream |
  | box | Package | physical unit in transit |
  | card | Payment | credit or refund |
  | gift | Replacement | replacement order and delivery |
  | tag | Return label | return method and waybill |

To free the top-left block, both return lines (`pay-cust`, `log-cust`) were
rerouted: they still run along the `y:18` lane but now drop down the far-left
edge at `x:16` to `y:306`, then step right into the Customer node's top port.
The old route dropped at `x:50`, straight through where the legend now sits.

### Layout v3 — scale clearance and the return loop

The 1.5× active scale sets the minimum spacing, not aesthetics: a 124px node at
1.5× is 186 wide, so two neighbours need **≥155px between centres** before the
enlarged one touches the idle one. The row was respaced to that rule and the
right-hand column restacked so each Streamliner module sits under the
enterprise system it talks to:

| Row | Nodes (x) |
|---|---|
| y 86 | ARB 1090 · Logistics 1264 |
| y 172 | ABACUS 880 · OMEGA 1090 · Oracle payments 1264 |
| y 360 | Customer 50 · Channels 142 · Care Agent 266 · SFDC 440 · Create & Edit 640 · **Verify & Review 880** · **Process & Track 1090** |
| y 470 | Return Method 880 · Delta · RIMS 1254 |
| y 548 | BIL/SODS 640 · PDSL 1090 · OTM 1254 |

Verify & Review sits under ABACUS, Process & Track under OMEGA. Delta · RIMS and
OTM moved to the bottom-right corner, which was dead space.

The return lines (`pay-cust`, `log-cust`) now close the loop as a full rectangle:
right riser `x:1384` → bottom lane `y:586` → left riser `x:16` → Customer's left
port. That clears the legend completely, so the legend keeps the top-left corner.

### Progress rail

Labels never disappear. The rail wraps to a second line instead
(`flex-wrap:wrap`), and `min-height:76px` reserves both lines so the frame height
does not change between flows. Three visually distinct states:

- **upcoming** — no fill, hollow dot
- **active** — solid pill in the current node's group colour, white text, pulsing dot
- **done** — light green pill `#E9F5EE`, green dot, white check, label intact

Below 760px the rail switches to `nowrap` at `min-width:900px` so it scrolls with
the canvas as one scroll region rather than stacking into four rows.

Heights: 1072 → 700 (content 694), 682 → 679, 350 → 676. Desktop lock unchanged.

### Layout v4 — even pitch, landing, 1.3× scale

**Row pitch is now a single constant: 210px between every centre**, and every pill
on the row is the same 124px wide (SFDC was 100 — widened so the visual gaps match
the centre pitch). Care Agent 265 · SFDC 475 · Create & Edit 685 · Verify & Review
895 · Process & Track 1105.

Active scale dropped 1.5× → **1.3×**. A 124px pill at 1.3× is 161 wide, so the
clearance needed between centres falls from 155 to 143 against the 210 pitch —
81px of air at the tightest point, and nothing reads as stacked any more.

Everything downstream moved with the row: ABACUS/OMEGA to 895/1105, ARB to 1105,
and the whole right-hand column (Logistics, Oracle payments, Delta · RIMS, OTM)
out to 1268. Risers followed: `log-otm` 1372, the return loop 1392.

**The token lands instead of vanishing.** A `land` phase sits between `move` and
`work` (0.3s, collapsed to 0.01s under reduced-motion):

```
p>=1 (move) → phase='land', N[id].target=1.3   // node starts growing on impact
tokSc = u<.32 ? 1+.34*(u/.32) : 1.34*(1-ease((u-.32)/.68))
halo  r 12→38, opacity .3→0                    // impact ripple at the node
opacity holds to u=.58, then fades
```

The token pops 1.34× then collapses into the node while a ring spreads from it,
and the node's spring is already expanding — so the beat reads as a drop onto the
pill, not a disappearance. The progress pill stays lit through `land` so the rail
does not flicker between move and work.

**Toggle dot** on the selected tab blinks green (`--green-dot:#3DDC84`, 1.3s
ease-in-out, opacity 1→.4 plus an expanding 4px glow). Bright green, not the
`#1E8A4C` tick green — the dot sits on the deep navy pill and needs the lift.
Killed under `prefers-reduced-motion`.

Heights unchanged: 1072 → 700, 682 → 679, 350 → 676.

### Palette v2 — hue separation over harmony

The first group palette kept every colour inside the blue–teal–violet band so it
would sit quietly next to the Dell blue. It sat too quietly: SFDC, the data
services and the modules all read as "some kind of blue", which defeats the point
of grouping.

Blue is now reserved for Streamliner alone, and the other five groups are spread
across the wheel instead:

| Group | Hex | Hue | Contrast on white |
|---|---|---|---|
| Streamliner modules | `#0672CB` | 207° blue | 4.91 |
| Case system (SFDC) | `#7C3AED` | 262° violet | 5.70 |
| People & channels | `#57575C` | neutral (5% sat) | 7.18 |
| Data services | `#B02071` | 326° magenta | 6.38 |
| Logistics | `#A15C00` | 34° amber | 5.19 |
| Finance | `#0B6E4F` | 161° green | 6.25 |

Minimum separation between any two saturated groups is **46°** (blue vs green),
and those two also differ sharply in warmth. People is deliberately near-neutral
— humans are not a system, and a grey reads that way without spending a hue.

Amber and green already exist in the Dell kit (`#A15C00`, `#1E8A4C`), so the set
still belongs to the same family. Finance green is darker than the progress-rail
tick green and is only ever an outline or a label, never a filled dot, so the two
do not compete.

### Legend v2

Headings removed — a swatch next to a word and an icon next to a word do not need
to be told what they are. The box tightened from 228×204 at `x:34 y:36` to
**236×160 at `x:16 y:20`**, pushed into the corner, rows starting at `y:48` with
the same 23px pitch. Bottom padding went from 43px to 17px, matching the top.

### Progress rail — label wording and case

A milestone label names **what** reached the state, not just the state. "Approved"
tells a viewer nothing; "Credit approved" and "Exchange approved" tell them which
flow they are watching. Every label is exactly two words, so the pill breaks after
the first word and all ten milestones stay on one line at 1200px:

```js
`<span class="lb">${o.s[3].replace(' ','<br>')}</span>`
```

The pill is 40px tall with `line-height:1.18` and a 14px radius — a capsule radius
on a two-line pill looks like a mistake.

| Credit | Return | Exchange |
|---|---|---|
| Issue reported | Return requested | Issue reported |
| Case created | Case created | Case created |
| Credit raised | Return approved | Exchange created |
| Credit approved | Return method | Exchange approved |
| Credit prepared | Shipping arranged | Shipping arranged |
| Credit paid | Package collected | Package collected |
| Agent notified | Item received | Item inspected |
| Customer credited | Item inspected | Replacement ordered |
| | Refund issued | Replacement shipped |
| | Customer refunded | Replacement delivered |

**Case rule.** Sentence case everywhere, with one exception: a product module keeps
Title Case because it is a proper name — Create & Edit, Verify & Review, Process &
Track, Return Method, Disposition & Recovery. Roles and places are not products and
take sentence case: Care agent, ARB facility, Oracle payments, E-support.
Acronyms stay as they are: SFDC, BIL / SODS, PDSL, OTM, ABACUS, OMEGA.

### Layout v5 — readout removed, uniform inset, unused routes hidden

**The Application / Action readout is gone.** It occupied the whole top-right and
roughly 60px of height for two lines of text that the progress rail already
implies. The toggle keeps the top bar on its own; the reclaimed height went
straight into the diagram (viewBox `0 0 1400 600` → `0 0 1372 671`).

**Unused routes are hidden, not dashed.** This was the real source of visual
noise: every route not used by the active flow was drawn as a faint dashed line,
and because the routes are orthogonal, those dashes formed ghost rectangles that
read as containers that were never there.

```js
EL[k].setAttribute('opacity', u ? 1 : 0)   // was: stroke '#ECECEF' + dasharray '3 6'
```

Unused *nodes* still show at `.34` opacity, so the full system stays legible —
only the phantom plumbing goes away.

**Uniform 14-unit inset.** The viewBox was trimmed from 1400 to 1372 so it ends
where the content ends. Every outer element now sits exactly 14 units from its
edge: legend at `x:14 y:14`, return risers at `x:14` and `x:1358`, bottom lane at
`y:657`. Measured content bounds confirm 14 on all four sides. With 24px of card
padding on top of that, the optical margin is identical all the way round.

**Row pitch** rebuilt for the taller canvas, with every gap checked against the
1.3× active scale:

| Row | y | nearest neighbour clearance |
|---|---|---|
| Logistics chain | 96 | 50 |
| Finance chain | 196 | 37 |
| Disposition & Recovery | 288 | 37 |
| Main Streamliner row | 380 | 60 |
| Return Method · Delta · RIMS | 490 | 48 |
| Data services | 588 | 49 |
| Return lane | 657 | 44 |

### Divider rhythm

24px of clear space above and below the rule that separates the diagram from the
progress rail — `margin-top:24px` for the space above, `padding-top:24px` for the
space below, with the `border-top` sitting between them. That matches the 24px
card padding, so the diagram sits in an even 24px box on all four sides plus the
divider.

The 24px was taken out of the diagram, not added to the frame: the viewBox went
`671 → 639` and every row moved up with it, so the asset still measures exactly
700px at 1200. Row pitch after the compression:

| Row | y | clearance to the row above at 1.3× |
|---|---|---|
| Logistics chain | 92 | — |
| Finance chain | 186 | 44 |
| Disposition & Recovery | 274 | 33 |
| Main Streamliner row | 362 | 33 |
| Return Method · Delta · RIMS | 468 | 56 |
| Data services | 560 | 42 |
| Return lane | 625 | 40 |

### Layout v6 — actor nodes and the closing loop

**Pulse rings removed.** The active node already announces itself three ways: it
scales to 1.3×, it swaps to the lift shadow, and its loader arc sweeps. A fourth
signal expanding outward was noise, and on a dense diagram it read as a collision
with whatever sat next to it.

**The customer is a self-contained node.** Radius 27 → 36 with the label moved
inside the circle at 10px, icon lifted to `y-23`. The idle left edge lands at
`x:14`, matching the legend and the frame inset exactly, and freeing the space
under the circle that the external label used to occupy.

**The loop closes underneath.** The return now runs along the bottom lane and
rises into the customer's *bottom* port rather than coming up the left edge into
the side: `RET = [[1358,625],[50,625]]` → `P('cust','b')`. A request that comes
back from below reads as a cycle closing; one that arrives from the left reads as
another input. The left riser is gone entirely.

**Care agent carries a headset mark.** The only node that is a person doing a job
rather than a system or a place, so it earns an icon. The glyph sits at
`x-39`, the label shifts `+10`, and the pair stays optically centred — driven by
an `n.mark` flag so any node can opt in without special-casing the renderer.

**E-support** lifted 12 units to `y:270`, opening the gap between it and the main
row.

### Layout v7 — even gaps, not even pitch

The main row mixes three widths: circles (80), the channel bar (34) and pills
(124). Equal spacing between *centres* therefore does not look equal — the eye
measures the white space between edges, not the distance between middles.

The row is now spaced on a constant **77-unit edge gap**:

| Node | x | edges |
|---|---|---|
| Customer | 54 | 14 – 94 |
| Channels | 188 | 171 – 205 |
| Care agent | 322 | 282 – 362 |
| SFDC | 501 | 439 – 563 |
| Create & Edit | 702 | 640 – 764 |
| Verify & Review | 903 | 841 – 965 |
| Process & Track | 1104 | 1042 – 1166 |

Derived, not eyeballed: the row spans `14 → 1166`, the seven nodes total 690, so
the 463 units left over divide into six gaps of 77. Every scaled node was then
checked against both idle neighbours — worst clearance is 59 units.

Everything below the row moved with it: E-support and Care agent share `x:322`,
BIL/SODS sits under Create & Edit at 702, Return Method and ABACUS under Verify &
Review at 903, PDSL and OMEGA under Process & Track at 1104.

**Customer feeds E-support from the top of the circle**, not the side:
`'cust-es':[P('cust','t'),[54,270],P('es','l')]`. The right port already carries
the channel route, and two routes leaving the same point made the self-service
path look like a branch of the phone call rather than an alternative to it.

### Layout v8 — upper rows lifted, Disposition on the Create & Edit column

| Row | y (was) | y (now) |
|---|---|---|
| ARB facility · Logistics | 92 | **72** |
| ABACUS · OMEGA · Oracle payments | 186 | **166** |
| Disposition & Recovery | 274 @ x 802 | **234 @ x 702** |
| Main Streamliner row | 362 | 362 |

Disposition & Recovery now sits directly above Create & Edit and to the left of
ABACUS, which collapses its feed into a straight vertical:

```js
'ce-dr':[P('ce','t'),P('dr','b')]      // was a three-point elbow through [702,274]
```

An orthogonal router that produces a straight line is a sign the two nodes belong
in the same column. It is worth moving a node to get one.

`pt-ab` moved off `y:232` to `y:286` — with Disposition at `y:234`, the old lane
ran two units from its edge, which reads as a collision even though it clears.
Parallel lanes need to be obviously parallel or obviously separate; two units is
neither.

Risers followed their rows: `log-otm` and `log-cust` to `y:72`, `pay-cust` to
`y:166`.

Clearances re-checked at 1.3×: ABACUS to Disposition 19 units, Disposition to
Create & Edit 86, ARB to ABACUS 39. Frame margins unchanged at 14 on all sides.

### Layout v9 — Disposition on the ABACUS line

Disposition & Recovery moves to `y:166`, sharing the horizontal with ABACUS,
OMEGA and Oracle payments. It stays at `x:702`, so it is still directly above
Create & Edit and the `ce-dr` feed is still a straight vertical.

That move broke its route to Process & Track — the old lane ran along `y:234`,
and at `y:166` a straight horizontal would pass **through** ABACUS. Rerouted down
and across:

```js
'dr-pt':[P('dr','r'),[802,166],[802,250],[1104,250],P('pt','t')]
```

`x:802` is the only clear riser between them: Disposition's scaled right edge is
782.6, ABACUS's scaled left is 822.4, so it clears both by ~19 units whichever one
is active. The `y:250` lane sits below ABACUS (185) and above the main row (343).

`ce-pt` lifted `330 → 322` for more air under the main row.

### Actor icons scale with their labels

Icons in the actor circles went `scale(.92)` → `scale(1.1)` — about 22px to 26px
against an 11px label. An icon that is only twice its caption reads as an
afterthought; roughly 2.4× is where the glyph becomes the primary content of the
circle and the label becomes its caption.

### Stroke thickens as the node fills

The stroke used to jump to 2px when work started and drop back to 1.5px when it
finished, so the node ended lighter than it was mid-work — backwards, since a
finished node has more to say than an idle one.

| State | Stroke | Opacity |
|---|---|---|
| Idle | 1.4 | `.45` |
| Working | `1.4 + .8·p` | `1` |
| Done | 2.2 | `1` |

Weight and fill now advance together and both hold at the end. `stroke-width` was
also removed from the CSS transition list — with a per-frame value, a 400ms ease
makes the stroke lag its own fill.

## Programme timeline (ju9AZVRUT)

Hosted separately: https://github.com/souvikb93/Stramliner_timeline
Live: https://souvikb93.github.io/Stramliner_timeline/

Seven regional research tracks and five workstreams on a 52-week grid. Bars are
positioned in weeks, never pixels, and the month axis is derived from the same
scale — so rescheduling is editing two numbers.

Phase colours reuse the group palette: Discover `#0672CB`, Synthesise `#7C3AED`,
Co-create `#A15C00`, Design & define `#B02071`, Validate & pilot `#0B6E4F`. Each
clears 4.5:1 on white, so white bar labels are legible at 9.5px.

**Overlap is the content.** A programme plan that reads as a neat staircase is
lying about how the work ran. Synthesis opens in week 8 while four regions are
still in the field; design and the rules layer run together for thirteen weeks.
The playhead exists to make those simultaneities visible — the footer names every
track running at the current week, so two or three names at once *is* the point.

Secondary work (follow-up sessions) uses a thin bar at 11px against the main
19px, so a row can carry two kinds of activity without reading as two rows.

**The schedule is illustrative.** Region names, country lists and interview
counts are real and match the research map. The week numbers are a
reconstruction and are flagged in that repo's README — they need replacing with
the real dates before this is shown to anyone who was on the project.
