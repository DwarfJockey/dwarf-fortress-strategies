# World generation

World gen sets the fort's long-term FPS ceiling. Every civ, site, and historical figure generated here keeps getting simulated off-screen forever, so the knobs below pay back across the whole life of the fort — not just embark.

!!! info "Version"

    Written against the current Steam release. Legacy advanced world-gen parameter names differ; the [DF Wiki — Advanced world generation](https://dwarffortresswiki.org/index.php/Advanced_world_generation) page is the cross-reference.

## World size

Smaller worlds simulate less per tick. Fewer civs, fewer sites, fewer historical figures churning in the background = better late-fort FPS. The tradeoff is a thinner legends mode and fewer trade partners.

> **TODO:** which size I actually pick (pocket / smaller / small / medium / large) and the FPS difference I've seen.

## History length

Every year of pre-fort history populates historical figures, artifacts, beast slayings, and entities the game keeps tracking forever. Shorter history = faster world-gen + lower per-tick load once you embark.

> **TODO:** the exact year count I use and why.

## Civilization count

I really only need 1–2 dwarven civs, a couple of trade partners (humans / elves), and one or two hostile civs (goblins, kobolds). More than that just pads off-screen simulation without changing how the fort plays.

> **TODO:** exact civ counts I set.

## Mineral scarcity

I leave this at default ("Sparse"). "Everywhere" trivializes flux and magma-safe planning, which is half of what makes site selection interesting.

> **TODO:** any tweaks I make and the reason.

## Beasts and megabeasts

Forgotten beasts and titans accumulate the longest and are the biggest late-game FPS cost in this group. Semi-megabeasts and megabeasts mostly burn out before play, so they matter less.

> **TODO:** exact beast counts I land on.

## Advanced parameters I touch

> **TODO:** any other knobs (savagery, mineral density per layer, evil-region rarity, embark points, mountain peak count, etc.) and why.

## DFHack notes

> **TODO:** post-gen survey tools I use — [`embark-assistant`](https://docs.dfhack.org/en/stable/docs/tools/embark-assistant.html) for site search, [`prospect`](https://docs.dfhack.org/en/stable/docs/tools/prospect.html) at the embark screen.

## References

- [DF Wiki — World generation](https://dwarffortresswiki.org/index.php/World_generation)
- [DF Wiki — Advanced world generation](https://dwarffortresswiki.org/index.php/Advanced_world_generation)
