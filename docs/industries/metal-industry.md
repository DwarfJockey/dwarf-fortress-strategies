# Metal industry

## Prerequisites

> **TODO:** ore, fuel (charcoal or magma + coke), flux for steel, anvil, smelter, forge.

## Bronze, iron, steel

```mermaid
graph LR
  Iron[Iron ore] --> Smelter
  Flux --> Smelter
  Coke --> Smelter
  Smelter --> PigIron[Pig iron]
  PigIron --> Smelter2[Smelter]
  Iron2[Iron bars] --> Smelter2
  Flux2[Flux] --> Smelter2
  Coke2[Coke] --> Smelter2
  Smelter2 --> Steel
  Steel --> Forge
  Forge --> Weapons[Weapons & armour]
```

> **TODO:** ratios for steel production, magma forge benefits.

## What to forge

> **TODO:** military priority order — picks, axes, then armour, then weapons.
