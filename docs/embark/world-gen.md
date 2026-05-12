# World generation

World gen sets the fort's long-term FPS ceiling. Every civ, site, and historical figure generated here keeps getting simulated off-screen forever, so the knobs below pay back across the whole life of the fort — not just embark.

!!! info "Version"

    Written against the current Steam release. Legacy advanced world-gen parameter names differ; the [DF Wiki — Advanced world generation](https://dwarffortresswiki.org/index.php/Advanced_world_generation) page is the cross-reference.

## World size

**Medium (`[DIM:129:129]`).** Smaller than Medium and there isn't enough map area to deliver real elevation contrast — mountains, valleys, and rivers end up squeezed on top of each other. Larger than Medium and the off-screen sim cost climbs past the point I want to pay for in a long-running fort. Medium is my sweet spot for "interesting terrain that still runs at year 10."

## History length

**`[END_YEAR:100]`.** A hundred years is enough for civs to spread out, found a real road network, fight a couple of wars, and seed a respectable artifact list — but not so long that legends mode chokes or that every region is saturated with named historical figures the game has to keep ticking forever.

## Civilization count

**`[TOTAL_CIV_NUMBER:30]`, `[TOTAL_CIV_POPULATION:5000]`, `[SITE_CAP:500]`.** Thirty civs leaves room for multiple dwarven and goblin civs to coexist across a Medium world, but the population and site caps are what actually hold the line on background simulation. `TOTAL_CIV_POPULATION` is the single biggest world-gen lever on long-run FPS — every off-map dwarf, human, elf, and goblin gets ticked, forever.

## Mineral scarcity

**`[MINERAL_SCARCITY:2000]`.** Slightly richer than the default Sparse (2500), still nowhere near the "Everywhere" trivialization. I want flux and magma-safe stone to be a real site-selection question, not a guarantee, but with enough copper / iron / tetrahedrite that an unlucky embark isn't a dead embark.

## Beasts and megabeasts

**`[MEGABEAST_CAP:25]`, `[SEMIMEGABEAST_CAP:25]`, `[TITAN_NUMBER:9]`.** Roughly half the defaults. I don't want a forgotten-beast-of-the-month subscription clogging the cavern sim for the entire fort — that's a real late-game FPS sink as bodies and lairs accumulate. Half-default still gives plenty of "first FB" drama early on.

## Advanced parameters I touch

Four groups, each aimed at one of: more terrain contrast, more rivers, more biome variety, more cavern variety.

### Terrain — more elevation contrast

```text
[ELEVATION_FREQUENCY:1:3:1:1:1:3]
[PEAK_NUMBER_MIN:30]
[EROSION_CYCLE_COUNT:100]
[PERIODICALLY_ERODE_EXTREMES:0]
```

Weight the elevation distribution toward the low and high bands (instead of the default flat `1:1:1:1:1:1`), force more named peaks, cut erosion roughly in half, and stop the periodic extreme-smoothing pass. Net effect: sharper mountains, deeper valleys, more visually dramatic embark options.

### Rivers — more of them, in more places

```text
[RIVER_MINS:200:200]
[RAINFALL:25:100:200:200]
[RAIN_FREQUENCY:1:1:1:2:3:3]
[OROGRAPHIC_PRECIPITATION:1]
```

Lower the minimum river-source elevation from default 400 so rivers can start outside just the high peaks. Bump the rainfall floor up from 0 so dry regions still grow some water, and weight the rainfall distribution toward the wet end. Orographic precipitation makes the windward sides of mountains get extra rain, which produces more headwaters where the elevation tweaks above just created new ridges.

### Biomes — more good, slightly more evil, more variety

```text
[GOOD_SQ_COUNTS:100:200:400]
[EVIL_SQ_COUNTS:50:100:200]
[SUBREGION_MAX:1500]
```

Roughly 4× the good-tile floor and 2× the evil-tile floor relative to the default `25:50:100` — much more alignment variety on embark without flooding the map with reanimating biomes. Bumping `SUBREGION_MAX` lets the map break into more distinct biome patches instead of one big undifferentiated swamp.

### Caverns — full variety, not three near-identical layers

```text
[CAVERN_LAYER_OPENNESS_MIN:0]
[CAVERN_LAYER_OPENNESS_MAX:100]
[CAVERN_LAYER_PASSAGE_DENSITY_MIN:0]
[CAVERN_LAYER_PASSAGE_DENSITY_MAX:100]
[CAVERN_LAYER_WATER_MIN:0]
[CAVERN_LAYER_WATER_MAX:100]
```

Full 0–100 ranges on openness, passage density, and water mean the three cavern layers come out genuinely different — one cramped and dry, one cathedral-sized, one half-flooded — instead of three near-identical levels. Separately, `[VOLCANO_MIN:30]` gives more magma embark options without changing the cavern shape.

## Drop-in overrides

When I run a new world with this preset, these are the tokens I override on top of the **Medium Region** default. Anything not listed keeps the default value.

```text
# Size and history
[DIM:129:129]
[END_YEAR:100]

# Civ + site caps (long-run FPS)
[TOTAL_CIV_NUMBER:30]
[TOTAL_CIV_POPULATION:5000]
[SITE_CAP:500]

# Beasts (cut accumulation, keep early drama)
[MEGABEAST_CAP:25]
[SEMIMEGABEAST_CAP:25]
[TITAN_NUMBER:9]

# Terrain — sharper mountains and valleys
[ELEVATION_FREQUENCY:1:3:1:1:1:3]
[PEAK_NUMBER_MIN:30]
[EROSION_CYCLE_COUNT:100]
[PERIODICALLY_ERODE_EXTREMES:0]

# Rivers — more sources, more rain
[RIVER_MINS:200:200]
[RAINFALL:25:100:200:200]
[RAIN_FREQUENCY:1:1:1:2:3:3]
[OROGRAPHIC_PRECIPITATION:1]

# Biomes — more good, slightly more evil, more variety
[GOOD_SQ_COUNTS:100:200:400]
[EVIL_SQ_COUNTS:50:100:200]
[SUBREGION_MAX:1500]

# Caverns — full variation across the three layers
[CAVERN_LAYER_OPENNESS_MIN:0]
[CAVERN_LAYER_OPENNESS_MAX:100]
[CAVERN_LAYER_PASSAGE_DENSITY_MIN:0]
[CAVERN_LAYER_PASSAGE_DENSITY_MAX:100]
[CAVERN_LAYER_WATER_MIN:0]
[CAVERN_LAYER_WATER_MAX:100]

# Resources
[MINERAL_SCARCITY:2000]
[VOLCANO_MIN:30]
```

!!! warning "If world gen stalls, relax the terrain constraints first"

    Low erosion + extreme elevations + lots of rivers means world gen will reject more candidate worlds before it lands a valid one. If gen takes too many rejections, bump `EROSION_CYCLE_COUNT` up toward 200 or set `PERIODICALLY_ERODE_EXTREMES` back to 1 — both relax the constraints without changing the overall shape of the world.

## DFHack notes

After world gen, I run [`embark-assistant`](https://docs.dfhack.org/en/stable/docs/tools/embark-assistant.html) at the site-finder screen and filter for the combination of flux + magma + soil + aquifer depth I'm after. This preset produces a lot of viable sites and I'd rather query them than scroll. [`prospect`](https://docs.dfhack.org/en/stable/docs/tools/prospect.html) on the embark rectangle before final confirm catches the actual ore and gem profile of the tiles I'm about to claim.

## References

- [DF Wiki — World generation](https://dwarffortresswiki.org/index.php/World_generation)
- [DF Wiki — Advanced world generation](https://dwarffortresswiki.org/index.php/Advanced_world_generation)
