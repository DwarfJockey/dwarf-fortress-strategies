# CLAUDE.md

Guidance for Claude Code sessions working in this repository.

## Purpose

A personal playbook of the **efficient** strategies the author actually uses to keep his Dwarf Fortresses alive — across both **vanilla DF** (Steam release / Classic) and **DFHack** (current stable). It is **not** a general DF reference; that role belongs to [dwarffortresswiki.org](https://www.dwarffortresswiki.org/). Pages here record what *he* does, what's worked, and *why it's efficient* — measured by FPS, resource economy, or player-side labor-hours saved. Built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and deployed to GitHub Pages.

Live site: https://dwarfjockey.github.io/dwarf-fortress-strategies/
Source: https://github.com/DwarfJockey/dwarf-fortress-strategies

## Stack

- **MkDocs** + **Material for MkDocs** theme
- Python deps managed by **uv** (`pyproject.toml`, `uv.lock`)
- Pages auto-deploy on push to `main` via `.github/workflows/docs.yml`

## Local commands

```sh
uv sync                       # install/update deps into .venv
uv run mkdocs serve           # live preview at http://127.0.0.1:8000
uv run mkdocs build --strict  # what CI runs; fails on broken links / orphan pages
```

## Adding a page

1. Create `docs/<section>/<slug>.md`.
2. Register it under `nav:` in `mkdocs.yml`. CI builds with `--strict`, so unreferenced pages and broken internal links fail the build.
3. Cross-link to related pages with relative Markdown links — `[strange moods](../moods/strange-moods.md)` — not absolute URLs.

## Writing conventions

### Vanilla vs. DFHack

When a topic has both a vanilla and a DFHack approach, present them in tabs so the reader can pick:

````markdown
=== "Vanilla"

    Manual procedure using only base-game UI.

=== "DFHack"

    `command-name` from DFHack stable. Link to https://docs.dfhack.org/en/stable/...
````

If a section is DFHack-only, put it under `docs/dfhack/` rather than mixing it into a vanilla page.

### Admonitions

- `!!! tip` — recommended setups, shortcuts.
- `!!! warning` — common ways to lose a fort (FPS death, tantrum spirals, breached caverns, untrained militia).
- `!!! note` — sidebars, lore, edge cases.
- `!!! info` — version-specific behavior callouts.

DF-specific custom types (defined in `docs/stylesheets/extra.css`): `!!! mood` (strange-mood callouts), `!!! fps` (framerate hazards), `!!! quickfort` (DFHack blueprint hints), `!!! lost` (post-mortem / fortress-falls).

### References

Link primary references on first mention in a page:

- Vanilla mechanics → relevant page on **dwarffortresswiki.org**.
- DFHack tools → **docs.dfhack.org/en/stable/** (always link the *stable* docs, not master).

### Diagrams

Use Mermaid for industry chains and dependency graphs:

````markdown
```mermaid
graph LR
  Plot[Underground plot] --> Plump[Plump helmets]
  Plump --> Still
  Still --> Booze
  Booze --> Tavern
```
````

### Building designs

Fortress layouts and room designs go in fenced **`text`** code blocks using the [dwarffortresswiki.org](https://www.dwarffortresswiki.org/) tile chars (`#` wall, `.` floor, `+` door, `O` fortification, `>`/`<`/`X` stairs, `,`/`o` ramps, `~` water). Multi-z designs use content tabs (one tab per z-level). When a design should be reproducible in DFHack, pair the diagram with a [Quickfort](https://docs.dfhack.org/en/stable/docs/guides/quickfort-guide.html) blueprint in a second tab using a `csv` fenced block. Full reference and worked examples: [`docs/conventions/building-designs.md`](docs/conventions/building-designs.md).

### Versioning

DF mechanics shift across releases. If a strategy depends on a specific DF version (e.g. Steam `51.x`) or DFHack release, state it explicitly in an `!!! info` block at the top of the page. Don't assume the reader is on the same version you wrote against.

### Tone

Write in **first person, from lived experience**. This is the author's playbook — "what I do, what's worked for me" — not a third-party wiki. Prefer "I pack 6 picks and 2 axes" over "bring picks and axes" or "you should bring picks and axes". Be concrete and specific: exact counts, exact materials, exact biomes — these are *his* numbers, not averages. Skip hedged general advice ("you may want to consider"); say what he does and why. Cite the wiki and DFHack docs to verify mechanics, not as the page's authority — the authority is the author's experience. Skip LP-style narrative ("then a forgotten beast appeared and..."); this is still a reference, just from one player's seat.

The lens is **efficiency**. Every recommendation should pay off in one of: **FPS** (item count, pathing, unit growth, off-screen simulation, hauling distance), **resources** (especially scarce — flux, magma-safe, shells, adamantine, cut gems), or **player-side labor-hours** (work orders over manual jobs, DFHack tools over repetition, profiles that route work automatically). When recommending something, the "why" line should usually point at one of those three.

## What not to commit

- `site/` — MkDocs build output (gitignored).
- `.venv/` — local virtualenv (gitignored).
- Save game files, raws dumps, screenshots above ~500KB — keep the repo light.
