# Rollback points

Two things changed in the type/weight/radius pass: **files in six git repos** and
**attributes on Framer embed nodes**. Git covers the first. Framer's own version
history covers the second, but the table below records every value so a node can
be put back by hand without hunting through it.

## Git

Every repo carries two tags:

| tag | what it points at |
|---|---|
| `pre-type-scale` | the last commit **before** the scale was applied |
| `type-scale-v1` | the state **after** it |

| repo | pre-type-scale | type-scale-v1 |
|---|---|---|
| `streamliner-final` | `bba654b` | `2539880` |
| `stremliner_dell_service_blueprint` | `9289087` | `455821e` |
| `streamliner_synthesis` | `f670652` | `da8237b` |
| `streamliner_overview` | `b086de1` | `e739645` |
| `streamline_world-map` | `fa8c1c4` | `515fb58` |
| `Stramliner_timeline` | `dd38cb9` | `fb96eb3` |

To see the old version of one asset without touching anything:

```bash
git show pre-type-scale:assets/KhZOFyrxi.html > /tmp/old.html
```

To roll one asset back and publish it:

```bash
git checkout pre-type-scale -- assets/KhZOFyrxi.html
git commit -m "Roll KhZOFyrxi back to the pre-scale version"
git push
```

To roll a whole repo back:

```bash
git revert --no-commit type-scale-v1
git commit -m "Roll back the type scale"
git push
```

Pages redeploys in under a minute and the Framer nodes need no change, because they
point at URLs. **Reset the node height too** — the old file renders at the old height.

## Framer nodes

Seven embeds were inline HTML and are now URL embeds. Rolling the repo back does not
undo that; it does not need to, since the URL then serves the old file. Only restore
the inline HTML if you want the asset out of git entirely, and that HTML lives in
Framer's version history.

| node | before | after |
|---|---|---|
| `LhtrzwMXl` | inline html, `fit-content` | url, 453 |
| `irYUR150B` | inline html, `fit-content` | url, 674 |
| `zX_dFoAE6` | inline html, `fit-content` | url, 282 |
| `cJPS6EXdE` | inline html, `fit-content` | url, 659 |
| `riEAs3sap` | inline html, `fit-content` | url, 478 |
| `hCEOXL0l5` | inline html, `170.5px` | url, 213 |
| `gdBqnnNCX` | inline html, `fit-content` | url, 374 |

Height-only changes, all already URL embeds:

| node | before | after |
|---|---|---|
| `CLTQi8f4o` | 445 | 490 |
| `KhZOFyrxi` | 389 | 359 |
| `OX6dWZqL5` | 681 | 674 |
| `TKuq5vJQK` | 486 | 488 |
| `ju9AZVRUT` | 747 | 744 |
| `M6ISaWJh2` | 658 | 672 |
| `CN6IB8Tax` | 303 | 290 |
| `k3rxHcFvx` | 541 | 529 |
| `k567uewrD` | 643 | 634 |
| `iVhhBwFBm` | 678 | 678 (unchanged) |

Stale `html` attributes were cleared on the nodes above so the project stops carrying
two copies of each asset. The content is identical to the file the node now loads.

## Not covered

- `regional-rule-layer`, `jDPWO0srd-discovery`, `jDPWO0srd-session` — files are tagged
  like everything else, but their node IDs are unknown, so no height was touched.
- The "IA Editor" embed on the page belongs to the Member Portal project and was not
  changed.
