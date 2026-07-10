# Galaxy Farm — Design Document v1.1

## Vision Statement

A cozy, Harvest Moon-inspired farming game set across a galaxy of small, walkable planets. The player terraforms barren worlds into thriving farms, raises animals, upgrades tools, builds structures, and connects planets into a personal farming empire — all with the charm and daily loop of SNES/N64-era Harvest Moon but with a unique space-exploration twist.

**Core Retention Drivers (in priority order):**
1. Farming loop — plant, water, grow, harvest, sell, repeat
2. Animal husbandry — daily care, products, breeding
3. Tool & skill upgrades — Harvest Moon-style tiered tools that let you do more with less effort
4. Building & decoration — farmhouse, barns, infrastructure
5. Social & exploration — NPCs, traveling merchant, town life, galaxy viewing

---

## Current State (What Exists)

### Architecture
- Single HTML file using Three.js (r160) via ES module import map
- 3D third-person camera with dual-joystick mobile controls + mouse/keyboard desktop
- Procedurally generated nebula skybox, stars, day/night cycle with sun/moon

### Existing Mechanics
| Feature | Status | Notes |
|---------|--------|-------|
| Planet traversal | ✅ Working | 3 planets, gravity-based movement, jump arcs between planets |
| Plow tool | ✅ Working | Creates dirt mounds on planet surface |
| Seed planting | ✅ Working | Plants a seed on a mound (no growth cycle yet) |
| Hammer tool | ✅ Working | Destroys props, fences, mounds; harvests wood/stone |
| Fence building | ✅ Working | Costs 1 wood, rotatable |
| Tree/Rock props | ✅ Working | Spawned at planet creation, yield wood/stone |
| Sheep creatures | ✅ Working | Hop around planets, "Baaa!" speech bubbles |
| Weeds | ✅ Working | Pulsing green spheres on planets |
| Inventory (basic) | ✅ Working | Wood and stone counters only |
| Audio | ✅ Working | Procedural melody loop + SFX (jump, pop, thud, baaa) |
| Day/Night cycle | ✅ Working | Arched HUD sun/moon, lighting changes |
| Mobile controls | ✅ Working | Dual virtual joysticks, pinch zoom |
| Desktop controls | ✅ Working | WASD + mouse look + scroll zoom |

### Known Gaps / Missing Systems
- No crop growth over time
- No seasons or calendar
- No terraforming (changing planet biome/terrain)
- No economy (selling, buying, shops)
- No NPC interaction or dialogue
- No building system (house, barn, silo)
- No animal husbandry beyond cosmetic sheep
- No save/load
- No stamina/energy
- No tool upgrades
- No progression or goals

---

## Design Pillars

1. **Satisfying Progression** — Better tools, bigger harvests, more efficient farming. The player should always feel like they're getting stronger.
2. **Cozy Daily Loop** — Farm, harvest, sell, upgrade. Satisfying in 10-minute sessions.
3. **Living World** — Animals to care for, merchants that visit, weather that matters, a town with people.
4. **Terraforming & Exploration** — Transform barren rocks into lush farmland. Each planet is a project.
5. **Scope Control** — Ship a polished core before expanding. Every feature must serve the loop.

---

## Core Game Loop

```
Wake Up → Tend Animals → Water/Harvest Crops → Explore/Terraform → Sell at Bin or to Merchant → Upgrade Tools/Build → Sleep
```

### Daily Cycle
- Day has a fixed real-time length (~5 minutes of active play)
- Player has a stamina bar that depletes with actions
- Sleeping advances to next day, restores stamina
- Crops grow overnight based on type and watering
- Animals produce goods each morning if cared for

---

## Planet Structure

### Planet Types
| Planet | Size | Role | Biome |
|--------|------|------|-------|
| Home Green | Medium (r=15) | Main farm, starter planet | Temperate grassland |
| Red Wastes | Small (r=10) | Desert farming, rare hot-climate crops | Arid desert |
| Blue Crags | Medium (r=16) | Cold farming, mining-rich | Tundra/ice |
| Ember Rock | Small (r=8) | Volcanic, exotic flowers, gems | Volcanic |
| Aqua Drift | Medium (r=12) | Water planet, fishing, aquatic crops | Ocean/marsh |
| Town Planet | Large (r=25) | Social hub, shops, NPCs, festivals | Urban/park |
| Wild Moon | Small (r=7) | Late-game, rare resources, harsh weather | Barren/crystal |

**Total: 7 planets** (5 farmable + 1 town + 1 challenge planet)

### Planet Connections
- Players can build **Star Bridges** between planets they own
- A Star Bridge is a permanent structure (costs significant resources) placed on two planets
- Once connected, travel between those planets is instant (no jump arc needed)
- Connected planets share a **weather zone** — rain on one means rain on connected neighbors
- This creates a "farming network" where the player optimizes which planets to connect
- Unlocked progressively: first bridge available mid-Phase 2

### Local Weather (Per-Planet)
- Each planet rolls its own daily weather: Clear, Cloudy, Rain, Storm, Snow (biome-dependent)
- Rain auto-waters all crops on that planet (huge time saver)
- Storms prevent outdoor work (stamina drain doubled, some crops damaged)
- Snow slows crop growth on cold planets, but some crops require it
- Connected planets share weather influence (70% chance of same weather)

---

## Feature Scope (Prioritized Phases)

### Phase 1 — Core Farming Loop
> Goal: A complete, satisfying farm loop on Home Green. The game is "playable" after this.

| Feature | Description |
|---------|-------------|
| Crop growth system | Seeds → Sprout → Mature → Harvestable over N days |
| Watering can tool | New tool, required daily or crops wilt/die |
| Harvest mechanic | Interact with mature crop to collect produce |
| Stamina system | Actions cost stamina; sleep restores it |
| Day/calendar system | Days advance, 4 seasons (7 days each = 28 day year) |
| Crop variety (6 crops) | Turnip, Potato, Tomato, Corn, Pumpkin, Starfruit |
| Shipping bin | Drop items in bin, receive Star Coins next morning |
| Currency (Star Coins) | Core economy currency |
| Save/Load | localStorage persistence of full game state |
| Basic tool upgrades | Hoe/Watering Can/Axe/Hammer: Basic → Copper → Silver → Gold |
| Upgrade effects | Higher tier = affects more tiles per use + costs less stamina |
| Bed/sleep mechanic | Interact with bed to end day |

### Phase 2 — Animals & Buildings
> Goal: Passive income streams and farm infrastructure. This is where the game gets its hooks.

| Feature | Description |
|---------|-------------|
| Animal types | Sheep (wool), Cow (milk), Chicken (eggs), Goat (cheese) |
| Animal care loop | Feed daily + pet/brush = happy animal = better products |
| Animal happiness | Neglect reduces product quality; love produces gold-star items |
| Barn building | Place on planet, houses 4 large animals |
| Coop building | Place on planet, houses 6 small animals (chickens) |
| Farmhouse | Player home with bed, storage chest, upgrade-able |
| Silo | Stores animal feed, auto-feeds if adjacent to barn/coop |
| Crafting station | Cheese press, loom, mayonnaise maker, preserves jar |
| Processed goods | Raw → Processed doubles or triples sell value |
| Incubator | Hatch eggs into new chickens; breed animals for traits |

### Phase 3 — Terraforming & Planet Progression
> Goal: Make exploration meaningful. Give purpose to every planet.

| Feature | Description |
|---------|-------------|
| Planet unlock system | New planets cost Star Coins + specific materials |
| Terraform meter | Farming/building actions fill terraform bar (0-100%) |
| Visual terraform stages | Planet shifts color, gains grass/water/trees as bar fills |
| Biome-specific crops | Ice Melon (Blue Crags), Fire Pepper (Ember Rock), etc. |
| Star Bridges | Build connections between planets for instant travel |
| Shared weather zones | Connected planets influence each other's weather |
| Planet upgrades | Increase soil fertility, expand farmable area per planet |
| Warp pad (early travel) | Cheaper than bridge, but costs stamina to use |

### Phase 4 — Social & Economy
> Goal: The world feels alive. People to meet, things to buy, reasons to sell.

| Feature | Description |
|---------|-------------|
| Town Planet | Large social hub with permanent shops and NPCs |
| Traveling Merchant | Visits Home Green on random days (2-3x per season) |
| Merchant stock | Rotates weekly: rare seeds, animal treats, cosmetics, blueprints |
| Merchant haggling | Friendship with merchant = better prices over time |
| Shop NPCs | Seed shop, tool smith, animal dealer, general store (on Town Planet) |
| NPC friendship | Gift items to build friendship, unlock recipes & events |
| Seasonal festivals | Harvest Fest, Star Festival, Snow Dance, Flower Fair |
| Quests/requests | "Bring me 5 tomatoes" bulletin board style |
| Light story arc | Restore the galaxy's dying farmlands, NPC backstories |

### Phase 5 — Multiplayer & Polish
> Goal: Show off your galaxy. Late-stage feature, built on solid single-player foundation.

| Feature | Description |
|---------|-------------|
| Galaxy Viewer | See other players' galaxy layouts (read-only) |
| Visit mode | Walk around another player's planets (no editing) |
| Trade post | Async trading — leave items for other players to buy |
| Leaderboards | Biggest farm, most terraformed, richest, etc. |
| Weather system polish | Visual rain/snow particles, thunder sounds |
| Fishing tool | Fish in ponds on Aqua Drift, sell or gift |
| Decoration items | Cosmetic placeables (paths, flowers, lights, signs) |
| Music system | Unique ambient track per planet + season |
| Achievement system | Milestone unlocks (ship 1000 items, terraform 3 planets, etc.) |

---

## Tool Upgrade System (Harvest Moon Style)

This is a core progression mechanic. Tools define how efficient the player is.

### Upgrade Tiers
| Tier | Cost | Effect |
|------|------|--------|
| Basic | Starting | 1 tile, full stamina cost |
| Copper | 500 coins + 5 copper ore | 2 tiles in a line, -20% stamina |
| Silver | 2000 coins + 5 silver ore | 3 tiles in a line, -40% stamina |
| Gold | 8000 coins + 5 gold ore | 3x3 area, -60% stamina |

### Tool List
| Tool | Basic Use | Upgraded Use |
|------|-----------|--------------|
| Hoe | Plow 1 tile | Plow line/area |
| Watering Can | Water 1 tile | Water line/area, larger capacity |
| Axe | Chop 1 tree (3 hits) | Chop in 2 hits → 1 hit |
| Hammer | Break 1 rock (3 hits) | Break in 2 hits → 1 hit |
| Scythe | Cut 1 weed | Cut line/area of weeds + harvest crops |

### Upgrade Process
- Bring materials + coins to the Tool Smith (on Town Planet or via Traveling Merchant)
- Tool is unavailable for 2 days while being upgraded
- Player must plan around the downtime

---

## Traveling Merchant

A key social/economic feature that adds variety to the daily loop without requiring the player to leave Home Green.

### Behavior
- Arrives on Home Green 2-3 times per season (semi-random, never on festival days)
- Stays for 1 full day (dawn to dusk), then leaves
- Parks a small ship/cart near the player's farmhouse
- Has a unique cheerful personality, remembers the player

### Stock (Rotates Each Visit)
| Category | Examples |
|----------|----------|
| Rare seeds | Out-of-season seeds, planet-exclusive seeds at markup |
| Animal treats | Boost animal happiness, temporary production bonus |
| Blueprints | Unlock new buildings/machines not available in town |
| Cosmetics | Player outfit pieces, decorative items |
| Materials | Small quantities of ore/gems at premium prices |
| Mystery bag | Random item, could be junk or rare — gamble element |

### Friendship with Merchant
- Gift items the merchant likes → better prices, exclusive stock
- At max friendship: merchant visits more often, sells legendary seeds

---

## Multiplayer Concept (Phase 5 — Future Scope)

### Philosophy
Multiplayer is **asynchronous and social**, not competitive or real-time cooperative farming. Think Animal Crossing visiting, not Stardew co-op.

### Features
| Feature | Description |
|---------|-------------|
| Galaxy View | See a star map of friends' galaxies with planet previews |
| Planet Visit | Walk around a friend's planet as a ghost (read-only, no editing) |
| Trade Post | Leave up to 5 items in your trade post; friends can buy with their own coins |
| Photo Mode | Snapshot your farm, share to a community board |
| Gift Box | Send 1 gift per day to a friend (arrives next morning) |

### What Multiplayer is NOT
- Not real-time co-op farming on the same planet
- Not competitive (no stealing, no PvP)
- Not required for any progression or content
- Not an MMO — small friends list, intimate scale

### Technical Approach (Later)
- Simple backend (Firebase or similar) for async state
- Each player's galaxy is a JSON blob stored server-side
- Visit mode downloads their state and renders it locally
- No real-time sync needed

---

## Data Model (Target for Phase 1+2)

```js
const gameState = {
    day: 1,
    season: 'spring',       // spring, summer, fall, winter
    seasonDay: 1,           // 1-7 within season
    time: 0,                // 0.0 - 1.0 (fraction of day)
    money: 0,
    stamina: { current: 100, max: 100 },
    
    inventory: {
        wood: 10, stone: 5,
        copperOre: 0, silverOre: 0, goldOre: 0,
        seeds: { turnip: 0, potato: 0, tomato: 0, corn: 0, pumpkin: 0, starfruit: 0 },
        produce: {},
        animalProducts: {},
        processed: {}
    },

    tools: {
        hoe:    { tier: 0 },  // 0=basic, 1=copper, 2=silver, 3=gold
        water:  { tier: 0 },
        axe:    { tier: 0 },
        hammer: { tier: 0 },
        scythe: { tier: 0 }
    },

    planets: [
        {
            id: 'home-green',
            name: "HOME GREEN",
            unlocked: true,
            terraformLevel: 0,
            weather: 'clear',
            connectedTo: [],        // planet IDs linked via Star Bridge
            tiles: [],              // { x, z, crop, waterDay, growthStage }
            buildings: [],          // { type, position, data }
            animals: [],            // { type, name, happiness, daysOwned }
            props: []               // trees, rocks (persistent)
        }
    ],

    merchant: {
        friendship: 0,
        lastVisitDay: -1,
        nextVisitDay: 4,
        stock: []
    },

    npcs: {},                       // { id: { friendship, giftsToday, dialogue } }
    quests: [],
    achievements: [],
    flags: {}                       // story/progression flags
};
```

---

## Art Style Guidelines

- **Toon/cel-shaded** — MeshToonMaterial with outlines (already in use)
- **Rounded, soft geometry** — Spheres, capsules, rounded boxes
- **Pastel + bold accent colors** — Planet palettes should feel distinct
- **Readable at distance** — Props need clear silhouettes since planets are small
- **Juice** — Pop/bounce animations on every interaction (already started)
- **Town Planet** — Slightly more detailed, colorful buildings, market stalls, paths

---

## Controls Reference (No Changes Planned)

| Input | Desktop | Mobile |
|-------|---------|--------|
| Move | WASD | Left stick (bottom-left) |
| Look | Mouse | Right stick (bottom-right) |
| Jump | Space | JUMP button |
| Interact | Click | Tap (top 75% of screen) |
| Zoom | Scroll wheel | Pinch |
| Tool select | Toolbar click | Toolbar tap |

---

## Scope Boundaries (What We Are NOT Building)

These are explicitly out of scope to prevent creep:

- ❌ Real-time co-op farming (async social only)
- ❌ Procedural infinite planets (fixed curated set of 7)
- ❌ Combat system of any kind
- ❌ Deep skill trees or RPG stat leveling (tools upgrade, player doesn't "level")
- ❌ Realistic graphics or PBR materials
- ❌ Voice acting or skeletal animation rigs
- ❌ Mobile app store release (web-only for now)
- ❌ Crafting chains deeper than 2 steps (raw → processed → finished)
- ❌ Competitive multiplayer or economy
- ❌ Survival mechanics (hunger, thirst, death)
- ❌ Dungeon/mining caves (resources found on planet surfaces)

---

## Open Questions (For Discussion)

1. **Season length** — 7 days per season feels snappy. Too short? 10 days?
2. **Tool upgrade downtime** — 2 days without your tool. Fun tension or frustrating?
3. **Merchant frequency** — 2-3 visits per 7-day season. Enough anticipation?
4. **Planet size for Town** — Should Town Planet be walkable or have a menu-based shop UI?
5. **Star Bridge cost** — How expensive? Should be a mid-game milestone.
6. **Animal limits** — Cap per planet? Per barn? How many barns allowed?
7. **Multiplayer priority** — Build the hooks early (save to server) or bolt on later?

---

## Implementation Order (Next Steps)

1. **Refactor game state** — Extract all loose variables into a single `gameState` object
2. **Save/Load** — localStorage so nothing is lost between sessions
3. **Crop growth** — Seeds advance stages day-over-day
4. **Watering can** — New tool, required for growth
5. **Stamina** — Actions cost energy, bed restores it
6. **Calendar HUD** — Show day/season, advance on sleep
7. **Shipping bin + money** — Complete the earn loop
8. **Tool upgrades** — Tiered progression with area effects
9. **Animals** — Start with chickens (simplest care loop)
10. **Buildings** — Coop first, then barn, then farmhouse upgrades

Each step builds on the last. The game stays playable at every point.

---

*Document created: July 9, 2026*  
*Last updated: July 9, 2026*
