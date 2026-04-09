# 🦠→🌌 Evo2048: Game Design Specification

## Core Concept

2048 with an evolutionary progression — tiles are life forms evolving from microscopic to cosmic. Same merge mechanics, more personality. Built as a single-page web app (HTML/CSS/JS or React), mobile-first, desktop-friendly.

## Tile Progression (18 tiers)

| Value | Emoji | Life Form | Color Temp |
|-------|-------|-----------|------------|
| 2 | 🦠 | Microbe | Cool blue-green |
| 4 | 🐜 | Ant | Cool green |
| 8 | 🐟 | Fish | Teal |
| 16 | 🐸 | Frog | Green |
| 32 | 🐍 | Snake | Yellow-green |
| 64 | 🦅 | Eagle | Yellow |
| 128 | 🐺 | Wolf | Warm yellow |
| 256 | 🐻 | Bear | Orange |
| 512 | 🦈 | Shark | Deep orange |
| 1024 | 🐘 | Elephant | Amber |
| 2048 | 🐋 | Whale | Red-amber |
| 4096 | 🐉 | Dragon | Red |
| 8192 | 🦕 | Dinosaur | Deep red |
| 16384 | 🌚 | Moon | Silver-violet |
| 32768 | 🌎 | Earth | Blue-violet |
| 65536 | 🌞 | Sun | Gold-white |
| 131072 | 🌌 | Universe | Deep purple/cosmic |
| 262144+ | ❓ | ??? | Black with stars |

### Color Philosophy

Tiles shift from cool (microscopic life) → warm (apex creatures) → red (mythic) → cosmic purple/gold for the celestial tiers. The gradient means you can read the board state by color at a glance even before parsing emojis.

### Tile Rendering

- Emoji is displayed prominently, centered in the tile
- Numeric value is small, in the bottom-right corner
- Background color per tier as described above
- The ❓ tier (262144+) displays with "???" as the value — if someone reaches it, let them screenshot it for clout

## Game Mechanics

### Standard 2048 Rules

- 4×4 grid
- Each move slides all tiles in the chosen direction
- Matching adjacent tiles merge into one tile of double the value
- One merge per tile per move (a 2-2-2-2 row becomes 4-0-4-0, not 8)
- After each move, a new tile spawns in a random empty cell

### Spawn Rules

**Normal mode:** 90% chance of 2 (🦠), 10% chance of 4 (🐜)

**Crazy 8s mode:** 80% chance of 2 (🦠), 12% chance of 4 (🐜), 8% chance of 8 (🐟)

### Win Condition

Reaching 🐋 (2048) triggers a win animation/message, but gameplay continues. The real game is chasing 🌌.

### Game Over

No valid moves remaining (board full, no adjacent matches). Triggers the Creature Parade (see below).

## Scoring

### Points

Standard 2048 scoring: every merge awards points equal to the resulting tile's value. Merging two 128s (🐺+🐺→🐻) = 256 points added to score.

### High Scores Page

- Persistent local storage, top 10 entries
- Each entry stores:
  - Final score
  - Highest tile reached (displayed as emoji + value, e.g., 🐉 4096)
  - Total number of moves
  - Number of undos used
  - Date
  - Whether Crazy 8s was active (flagged with 🎱 badge)
  - Whether No Undo was active (flagged with 🔒 badge)
- Default sort by score, tappable column headers to re-sort by highest tile
- All modes share one leaderboard. Badges tell the story: 🔒 = no undo, 🎱 = Crazy 8s, 🔒🎱 = unhinged.

## Undo System

### Multi-Step Undo

- Every move pushes a full snapshot (board state + current score) onto an undo stack
- Player can undo repeatedly, all the way back to the start of the game
- Stack depth: unlimited within the session
- Undo button displays available undo count: `↩ (14)`
- Each undo used is tracked and shown on the high scores page — a clean 2048 with 0 undos hits different than one with 47
- No score penalty for undoing. The counter IS the penalty.
- Undoing past a Crazy 8s spawn removes the spawned 8. The chaos giveth and the undo taketh away.

### Controls

- **Desktop:** Ctrl+Z / Cmd+Z
- **Mobile:** Undo button, always visible and thumb-reachable

## No Undo Mode 🔒

Toggled from the main menu alongside Crazy 8s. When active:

- Undo button is hidden entirely (not grayed out — gone)
- Ctrl+Z / Cmd+Z does nothing
- High score entries flagged with a 🔒 badge
- Combinable with Crazy 8s (🔒🎱 — chaos with no safety net, for absolute psychopaths)

This is a pride badge, not a separate leaderboard. All modes share one leaderboard. A 🔒 run at 80k is more impressive than a naked run at 120k and everyone knows it.

## Crazy 8s Mode 🎱

Toggled from the main menu or settings. When active:

- Spawn rates change as described above (80/12/8)
- Board border gets a subtle pulsing glow or accent color shift
- A 🎱 icon displays in the header near the score
- An 8 (🐟) spawning skips a merge step — free evolution — but clutters the board with mid-tier tiles that may not pair with what you're building. Gift and mess simultaneously.

## Controls & Responsiveness

### Desktop

- Arrow keys for movement
- Ctrl+Z / Cmd+Z for undo
- `N` for new game
- `C` to toggle Crazy 8s on/off (from menu/pre-game only, not mid-game)

### Mobile

- Swipe gestures for movement
  - Implement a dead zone / minimum swipe distance so small taps don't trigger moves
- Undo button fixed at bottom of screen, always thumb-reachable
- Board scales to viewport width with comfortable padding
- Lock viewport scale — no pinch-zoom issues
- Touch targets for buttons minimum 44×44px

### Both Platforms

- **Merge animation:** Tiles slide to new position, colliding tiles pop/scale-up briefly (the creature just evolved and is excited about it)
- **Spawn animation:** New tile fades in with a subtle scale-up
- **No sound effects** (may be added later as optional)

## Layout

### Mobile-First Layout (top to bottom)

```
┌─────────────────────────┐
│  EVO2048          🎱     │
│  Score: 1,284           │
│  Best:  4,096           │
├─────────────────────────┤
│                         │
│    ┌──┐ ┌──┐ ┌──┐ ┌──┐ │
│    │🦠│ │  │ │🐜│ │🦠│ │
│    │ 2│ │  │ │ 4│ │ 2│ │
│    ├──┤ ├──┤ ├──┤ ├──┤ │
│    │  │ │🐟│ │  │ │🐸│ │
│    │  │ │ 8│ │  │ │16│ │
│    ├──┤ ├──┤ ├──┤ ├──┤ │
│    │🐜│ │  │ │🦠│ │  │ │
│    │ 4│ │  │ │ 2│ │  │ │
│    ├──┤ ├──┤ ├──┤ ├──┤ │
│    │  │ │🦠│ │  │ │🐜│ │
│    │  │ │ 2│ │  │ │ 4│ │
│    └──┘ └──┘ └──┘ └──┘ │
│                         │
│  ↩ (7)    [New] [Menu]  │
└─────────────────────────┘
```

### Desktop

Same layout, centered on screen with max-width constraint (~500px). Comfortable whitespace around the board.

## Creature Parade (Game Over Animation)

When the game ends:

1. Board dims with a semi-transparent overlay
2. Final score and highest creature display prominently
3. A parade animation plays: every species you created during the run marches/scrolls across the screen, smallest to largest, with a count of how many of each you merged into existence
4. Example: `🦠×47  🐜×23  🐟×12  🐸×6  🐍×3  🦅×2  🐺×1`
5. After parade, buttons for "New Game" and "View High Scores"

The parade doubles as your evolutionary census. It softens the loss and makes every run feel like it produced something.

## Technical Notes

### State Management

- Board state: 4×4 array of tile values (0 = empty)
- Undo stack: array of `{ board: number[][], score: number }` snapshots
- Track per-game: moves count, undos used, highest tile reached, creatures merged (for parade)
- High scores: persist to localStorage

### Animations

- Tile movement: CSS transform/transition (~150ms ease)
- Merge pop: CSS scale transform (1.0 → 1.15 → 1.0, ~200ms)
- Spawn fade: opacity 0→1 + slight scale-up (~150ms)
- Creature parade: horizontal scroll/march animation

### Performance

- No heavy dependencies needed — vanilla JS or lightweight React
- Animations via CSS transitions, not JS frame loops
- Undo stack is just arrays of 16 numbers + a score — negligible memory even for hundreds of undos

## Open for Implementation

- Exact hex values for tile color palette (spec says cool→warm→cosmic gradient; implementer picks specific colors)
- Creature parade animation style (horizontal scroll, vertical stack reveal, or something else creative)
- Exact font choices and sizing
- Whether Crazy 8s toggle is accessible mid-game or only on new game (recommendation: new game only to keep leaderboard integrity)
