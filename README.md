# Framer assets

Every HTML embed used on [souvikb.net](https://www.souvikb.net), one repo, split by
project. Each file is standalone — no build step, no dependencies beyond the Uncut Sans
webfont from jsDelivr.

**Live:** https://souvikb93.github.io/framer-assets/

```
streamliner/          Dell Streamliner case study
streamliner/blueprint/  the as-is and to-be service blueprints
member-portal/        Member Portal case study
docs/                 the rules every asset follows
```

Files in `streamliner/` are named after the Framer node that hosts them, so the mapping
is unambiguous. A few older ones are named after what they are instead.

## Why one repo

These assets used to live in five separate repos plus inline HTML inside Framer. A change
to a shared token — a type step, a radius, a weight — meant six commits across six
remotes, and the inline ones could not take a shared fix at all. One repo makes it one
commit.

## Wiring an asset into Framer

Embed node → **URL** mode → the Pages address → **Height: Fixed**, matching the value in
the table below. URL embeds cannot auto-measure; Framer shows "URL embeds do not support
auto height" until the height is fixed.

## Changing an asset

```bash
vim streamliner/KhZOFyrxi.html
open streamliner/KhZOFyrxi.html     # check it renders, measure its height
git commit -am "what changed" && git push
```

Pages redeploys in under a minute. The Framer node never changes unless the height does.

## The rules

- [`docs/FRAMER_ASSET_DESIGN_SYSTEM.md`](docs/FRAMER_ASSET_DESIGN_SYSTEM.md) — type scale,
  weights, radius, colour, motion, the verification loop
- [`docs/STREAMLINER_ASSET_GUIDELINES.md`](docs/STREAMLINER_ASSET_GUIDELINES.md) — project
  specifics
- [`docs/ROLLBACK.md`](docs/ROLLBACK.md) — tags and how to go back
- [`docs/ANIMATED_SYSTEM_DIAGRAM.md`](docs/ANIMATED_SYSTEM_DIAGRAM.md) — how to build
  another asset like the to-be system diagram

Type, weight and radius are tokens, declared once per file and restepped at 1024 and 768.
Nothing hardcodes a px size any more.
