# Evo2048

2048 with an evolutionary progression. Tiles are life forms evolving from microscopic to cosmic. Same merge mechanics, more personality.

## Tile Progression

| Value | Creature | | Value | Creature |
|-------|----------|-|-------|----------|
| 2 | 🦠 Microbe | | 1024 | 🐘 Elephant |
| 4 | 🐜 Ant | | 2048 | 🐋 Whale |
| 8 | 🐟 Fish | | 4096 | 🐉 Dragon |
| 16 | 🐸 Frog | | 8192 | 🦕 Dino |
| 32 | 🐍 Snake | | 16384 | 🌚 Moon |
| 64 | 🦅 Eagle | | 32768 | 🌎 Earth |
| 128 | 🐺 Wolf | | 65536 | 🌞 Sun |
| 256 | 🐻 Bear | | 131072 | 🌌 Universe |
| 512 | 🦈 Shark | | 262144+ | ❓ ??? |

Colors shift cool (microscopic) → warm (apex) → red (mythic) → cosmic purple/gold.

## How to Play

Open `index.html` in any browser. No build step, no dependencies.

### Controls

| Action | Desktop | Mobile |
|--------|---------|--------|
| Move | Arrow keys | Swipe |
| Undo | Ctrl/Cmd+Z or ↩ button | ↩ button |
| New game | N | New button |
| Menu | — | Menu button |

Reaching 🐋 (2048) shows a win banner but gameplay continues. The real goal is 🌌.

## Modes

**Crazy 8s 🎱** — Changes spawn rates so 🐟 (8) occasionally appears, cluttering the board with mid-tier tiles that may not pair with what you're building. Gift and mess simultaneously.

**No Undo 🔒** — Hides the undo button entirely. Ctrl+Z does nothing. For people who want to feel something.

Both modes are toggled from the Menu and lock in when a new game starts. All scores share one leaderboard — badges tell the story. A 🔒 run at 80k hits different than a naked run at 120k.

## Undo System

Every move pushes a full snapshot (board + score + stats) onto an unlimited undo stack. The button shows how many undos are available: `↩ (14)`. Undo count is tracked and shown on the high scores page — a clean run with 0 undos is its own kind of achievement.

## High Scores

Top 10 scores saved to localStorage. Each entry shows: score, highest tile reached, move count, undo count, mode badges, and date. Click the Score or Best column headers to re-sort.

## Game Over: Creature Parade

When the board locks up, every species you merged into existence marches across the screen — smallest to largest, with a count. Your evolutionary census. It softens the loss.

## Technical

Single `index.html` file. Vanilla JS, no dependencies. Tile sliding uses CSS transform transitions; merge pop and spawn use CSS keyframe animations. State is plain arrays — the undo stack is just 16 numbers + a score per entry.
