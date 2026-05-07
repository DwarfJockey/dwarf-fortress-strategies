# Building designs

A uniform way to draw fortress layouts so every page on this site looks and reads the same.

## Approach

Use **ASCII art inside fenced code blocks**, following the tile-character convention used on [dwarffortresswiki.org](https://www.dwarffortresswiki.org/). For multi-z-level designs, stack the levels in [content tabs](https://squidfunk.github.io/mkdocs-material/reference/content-tabs/). When the design should be directly buildable in-game, pair the diagram with a [Quickfort](https://docs.dfhack.org/en/stable/docs/guides/quickfort-guide.html) blueprint in a second tab.

Why this shape:

- **ASCII renders identically on every device** — no images to chase, diffs are reviewable.
- **Quickfort is the de-facto blueprint format** in the DFHack community, so pairing the two means readers using DFHack can build the design verbatim.
- Both fit in plain code fences, so no extra MkDocs plugins are needed beyond the ones already configured (`pymdownx.tabbed`, `pymdownx.superfences`).

## Tile legend

| Char | Meaning            | Char | Meaning                  |
|------|--------------------|------|--------------------------|
| `#`  | wall (undug rock)  | `>`  | down stair               |
| `.`  | floor (dug-out)    | `<`  | up stair                 |
| `+`  | door               | `X`  | up/down stair            |
| `O`  | fortification      | `,`  | down ramp                |
| `~`  | water              | `o`  | up ramp                  |

Workshops, traps, and other special tiles get **single-letter labels with a per-diagram legend**. Pick mnemonic letters (`F` forge, `S` still, `T` trap, `B` bed, `D` door if `+` is ambiguous, etc.) and list them next to the diagram.

## Single-level example

A small fortified airlock:

```text
####+####    Legend:
#.......#      # wall
#.OOOOO.#      + door
#.......#      O fortification
#...T...#      T weapon trap
#.......#      > down stair into fortress
#...>...#
####+####
```

Keep diagrams **≤ ~30 columns wide** so they don't horizontal-scroll on mobile readers. For larger structures, split across multiple diagrams or use a wider macro view alongside zoomed sub-views.

## Multi-z-level example

Use content tabs, one tab per z-level, ordered top-down (`z+1`, `z+0`, `z-1`, …):

=== "z+0 (surface)"

    ```text
    #########
    #.......#
    #...>...#
    #.......#
    ####+####
    ```

=== "z-1 (entrance)"

    ```text
    ####.####
    #.OOOOO.#
    #.......#
    #...<...#
    #########
    ```

!!! note "Tab indentation"

    `pymdownx.tabbed` requires the body of each `===` block to be **indented by exactly 4 spaces**. The fenced code block then sits inside that indent. Mis-indenting silently breaks the tabs.

## Paired with a Quickfort blueprint

Two-tab pattern: the diagram on the left, the buildable blueprint on the right. The Quickfort CSV can be saved as a `.csv` file and run with `quickfort run path/to/file.csv` from the DFHack console.

=== "Diagram"

    ```text
    +--+--+    Legend:
    |B.|.B|      + corner / wall corner
    |..|..|      | wall (vertical)
    +--+--+      - wall (horizontal)
    |B.|.B|      B bed
    |..|..|      . floor
    +--+--+
    ```

=== "Quickfort (`#build`)"

    ```csv
    #build label(starter-bedrooms) start(1;1) message(2x2 starter bedrooms)
    b,d,b
    ,,
    b,d,b
    ,,
    b,d,b
    ```

See the [Quickfort user guide](https://docs.dfhack.org/en/stable/docs/guides/quickfort-guide.html) for the full blueprint syntax (`#dig`, `#build`, `#place`, `#zone`, etc.).

## Authoring tips

- **Use `text` as the fence language tag.** Material renders it in monospace with no syntax highlighting — exactly what ASCII diagrams want.
- **One legend per diagram.** Even when the chars are "standard," readers skim. Repeating the legend costs nothing.
- **Annotate with adjacent prose**, not in-diagram comments. Don't try to squeeze arrows or labels into the grid; describe them in the paragraph that follows.
- **Show the unhappy path.** When a design has a critical failure mode (e.g. a pump stack with the wrong magma-safe materials), put a `!!! warning` block beneath it.
- **Cross-link.** When a diagram references a concept covered elsewhere, link it: `[fortifications](../military/defenses.md)`, `[strange moods](../moods/strange-moods.md)`.
