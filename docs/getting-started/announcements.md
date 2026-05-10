# Announcement settings

DF's default announcements are tuned for newcomers — every cancellation, every off-screen brawl, every wandering rat surfaces somewhere. After a few forts I always trim `data/init/announcements.txt` so the game only interrupts me for things that actually require a decision.

The file is at `<DF install>/data/init/announcements.txt`. Each line is one announcement type with flags telling DF whether to pause, recenter, popup, log, or hide it. The full reference is on the [DF Wiki — announcements.txt](https://dwarffortresswiki.org/index.php/Announcements.txt) page; what's below is the strategy I apply on top of it.

!!! info "Version note"

    The Steam release exposes some of these via an in-game alerts panel, but the deep tuning still lives in the text file. DF re-reads it on save load — edit, then load (or restart).

## Flag legend

| Flag      | Effect                                                       |
|-----------|--------------------------------------------------------------|
| `A_D`     | Add to log / adventure-mode display                          |
| `D_D`     | Fortress-mode display (the side-categories panel)            |
| `BOX`     | Popup box — modal, requires a click                          |
| `P`       | Pause the game                                               |
| `R`       | Recenter camera on the event                                 |
| `ALERT`   | Surface in the alert panel                                   |
| `UCR`     | Append to all unit combat reports                            |
| `UCR_A`   | Append to active reports only — doesn't open new ones        |

## My four buckets

### Always pause + recenter — fort-ending events

I lose forts the moment I miss one of these:

- **Dwarf injury and death** (combat, syndromes, drowning, falls)
- **Strange moods** — single `STRANGE_MOOD` token covers all five subtypes (fey / secretive / possessed / fell / macabre)
- **Tantrum / berserk / possessed citizen** — limited time before they hurt someone
- **Forgotten beast / titan / megabeast / werebeast spotted**
- **Ambush** — every variant (snatcher, thief, ambusher, mischievous)
- **Undead attack**
- **Feature discovery** (`FEATURE_DISCOVERY` covers cavern breach, aquifer puncture, magma sea — they're all the same token)
- **Cave-in**
- **Adamantine strike** (`STRUCK_DEEP_METAL`) — magma-safety check before I keep digging
- **Demon emergence** (`ENDGAME_EVENT_*`) — the worst-case "drop everything"

Pattern: `BOX:P:R:ALERT` for decisions, `P:R:ALERT` (no popup) for investigations like death.

### Recenter, no pause — informational

I want to know about these but I don't want the game stopped:

- **Migrant wave arrivals** — recenter, watch them in, unpause and assign labors
- **Caravan / liaison / diplomat / noble arrivals** — recenter on first sight; the negotiate-mandates dialog will pause on its own when relevant
- **Notable mineral strike** (`STRUCK_ECONOMIC_MINERAL`) — gold or silver vein worth re-routing miners

Pattern: `A_D:D_D:R` (or add `:ALERT` for caravans where I want a persistent indicator).

### Popup, no pause — mid-priority signal

- **Artifact completed** (`MADE_ARTIFACT`) — celebrate, but the work order can wait until I'm at a stopping point
- **Artifact begun** (`ARTIFACT_BEGUN`) — recenter so I can see who claimed which workshop

Pattern: `A_D:D_D:BOX:R` for completion, `A_D:D_D:R` for begun.

### Log only — never in my face

DF's defaults are already reasonable for these — `A_D:D_D` with no popup, pause, or recenter. I leave them alone:

- **Job cancellations** (`CANCEL_JOB`, `JOB_OVERWRITTEN`) — biggest source of perceived spam, but the default isn't actually intrusive; the work-order system retries automatically
- **Vermin** (`VERMIN_BITE`, `VERMIN_DISTURBED`, `VERMIN_CAGE_ESCAPE`) — cats handle them
- **Pet death** (`PET_DEATH`) — keeps in the log so I know which war-dog died, but doesn't pause

## Reasoning, condensed

The cost of a missed event isn't symmetric. A missed dwarf-death announcement → tantrum spiral. A missed cancellation popup → nothing. So the rule:

**Pause for decisions, recenter for situations, log everything else.**

!!! warning "Don't suppress the log"

    The temptation is to turn off *every* announcement that's spammy. Don't kill the log entries — keep popups and pauses off, but leave `A_D:D_D` on. The first "cancels haul: too injured" after a siege is how I discovered a dwarf bled out off-screen. If I can't see it in the log either, I find out via the corpse stockpile.

## Drop-in patch

Download: [`announcements.txt`](announcements.txt){ download="announcements.txt" } — a partial overrides file covering ~45 of the highest-impact entries.

To apply:

1. Back up your existing `data/init/announcements.txt`.
2. Open it in a text editor.
3. For each entry in the patch, find the matching line in your file and replace it with the patched version.
4. Save, then load your DF save (or restart).

The patch only contains entries I'm actively *changing* from defaults. Everything else uses DF's defaults.

```text title="announcements.txt (partial overrides)"
# === Always pause + recenter ===

[CITIZEN_DEATH:A_D:D_D:UCR_A:ALERT:P:R]
[CAVE_COLLAPSE:A_D:D_D:ALERT:P:R]
[STRANGE_MOOD:A_D:D_D:BOX:P:R:ALERT]
[CITIZEN_TANTRUM:A_D:D_D:ALERT:P:R]
[BERSERK_CITIZEN:A_D:D_D:BOX:P:R:ALERT]
[POSSESSED_TANTRUM:A_D:D_D:BOX:P:R:ALERT]
[MEGABEAST_ARRIVAL:A_D:D_D:BOX:P:R:ALERT]
[WEREBEAST_ARRIVAL:A_D:D_D:BOX:P:R:ALERT]
[UNDEAD_ATTACK:A_D:D_D:BOX:P:R:ALERT]
[BEAST_AMBUSH:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_DEFENDER:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_HERO:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_RESIDENT:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_THIEF:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_THIEF_SUPPORT:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_THIEF_SUPPORT_SKULKING:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_THIEF_SUPPORT_NATURE:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_SNATCHER:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_SNATCHER_SUPPORT:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_AMBUSHER:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_AMBUSHER_NATURE:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_MISCHIEVOUS:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_INJURED:A_D:D_D:ALERT:P:R]
[AMBUSH_OTHER:A_D:D_D:BOX:P:R:ALERT]
[AMBUSH_INCAPACITATED:A_D:D_D:ALERT:P:R]
[FEATURE_DISCOVERY:A_D:D_D:BOX:P:R:ALERT]
[STRUCK_DEEP_METAL:A_D:D_D:BOX:P:R:ALERT]
[ENDGAME_EVENT_1:A_D:D_D:BOX:P:R:ALERT]
[ENDGAME_EVENT_1B:A_D:D_D:BOX:P:R:ALERT]
[ENDGAME_EVENT_2:A_D:D_D:BOX:P:R:ALERT]

# === Recenter, no pause ===

[MIGRANT_ARRIVAL:A_D:D_D:R]
[MIGRANT_ARRIVAL_NAMED:A_D:D_D:R]
[D_MIGRANTS_ARRIVAL:A_D:D_D:R]
[D_MIGRANT_ARRIVAL:A_D:D_D:R]
[CARAVAN_ARRIVAL:A_D:D_D:R:ALERT]
[NOBLE_ARRIVAL:A_D:D_D:R]
[LIAISON_ARRIVAL:A_D:D_D:R]
[DIPLOMAT_ARRIVAL:A_D:D_D:R]
[TRADE_DIPLOMAT_ARRIVAL:A_D:D_D:R]
[STRUCK_ECONOMIC_MINERAL:A_D:D_D:R]

# === Popup, no pause ===

[MADE_ARTIFACT:A_D:D_D:BOX:R]
[ARTIFACT_BEGUN:A_D:D_D:R]
```
