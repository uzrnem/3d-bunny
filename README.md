# Magic World (Remake)

This is a remake project inspired by *Magic World* by Funny Arts.  
The goal is to rebuild the original missions (Desert, Forest, Rocks, etc.) using **Three.js** in modern browsers.

Background Images are from Magic World, a game by Funny Arts. The images are used to create a nostalgic and immersive experience for players, reminiscent of classic games.  
https://www.funnyarts.com/magicworldindex.php  
https://www.funnyarts.com/magicworld

---

## ✅ Current Implementation (in `index.html`)

- **Game Framework**
  - Three.js rendering
  - HUD with moves, time, best score (saved in localStorage)
  - Basic menu (Start, Restart, Win/Lose screens)
  - Smooth camera following behind the ball (tank-style controls)
  - Ball movement with step-by-step tile navigation

- **Level Elements**
  - `floor`: Static tiles
  - `hero`: Starting position
  - `goal`: Exit tile (activates after collecting all crystals)
  - `lift`: Moving platforms (with pause at ends)
  - `teleporter`: Instant travel between two points
  - `stair`: Multi-height navigation
  - `bonuscrystall`: Collectible crystals (required to unlock the goal)

- **Game Flow**
  - Win/lose handling (fall detection, reaching goal)
  - Level progression (currently 2 demo levels with instructions in Level 2)
  - Collectibles system (goal unlocks after all crystals are collected)
  - Local best score tracking (time & moves)

---

## 📂 Mission Files Structure

Mission files (`mission-desert.txt`, `mission-forest.txt`, `mission-rocks.txt`) define levels in a **custom DSL**.  
Each level specifies:

- **Hero**: starting position & facing direction  
- **Floor**: walkable tiles (single or ranges)  
- **Exit**: goal position  
- **Obstacles**: ElectroShock, FalseFloor, etc.  
- **Enemies**: Mummy, Skeleton, Monkey, Ghost, etc.  
- **Bonuses**: Crystal, Life, Time, Shield, Fire, Bombs, Freeze  
- **Interactive Objects**: Teleporter, Lift, Stair  
- **Custom Models**: palm trees, obelisks, rocks, barrels, etc.  
- **Instructions**: tutorial messages  
- **DefaultTime**: level timer  

---

## ❌ Missing Features (Not Yet Implemented in `index.html`)

### 1. Core Gameplay Elements
- **Bonuses**
  - `BonusLife`, `BonusTime`, `BonusFreeze`, `BonusShield`
  - `BonusFire`, `BonusBombs`
- **Enemies**
  - `Mummy`, `Skeleton`, `Monkey`, `Ghost`, `Bat`, `Alligator`, `Scorpion`, `Mage`
- **Obstacles**
  - `ElectroShock` (damage)
  - `FalseFloor` (collapses after stepping)
- **Environment**
  - Custom models (`palm01.x`, `obelisk_desert.x`, etc.)

### 2. Level/Gameplay Logic
- Time limit (`DefaultTime`) per level
- Mission **Instructions** (story text shown before/during level)
- Enemy AI (movement, attack, detection)
- Bonus effects (extra time, extra life, freeze enemies, etc.)
- Hazard interactions (shock damage, floor collapse)
- Multi-level terrain with stairs and height changes
- Teleporter transitions with effects

### 3. Game Structure
- Parsing mission files (`.txt`) into level definitions
- Level progression across **multiple worlds** (Desert, Forest, Rocks…)
- Full asset pipeline (models, textures, sounds)

---

## 🚀 Roadmap for Developers

1. **Parser**
   - Build a parser to load `.txt` mission files into JSON structures compatible with `index.html`.

2. **Entity System**
   - Create classes for `Enemy`, `Bonus`, `Obstacle`, `Teleporter`, `Stair`, etc.
   - Each should handle rendering + behavior.

3. **Game Loop Integration**
   - Extend collision detection for:
     - Picking up bonuses
     - Triggering hazards
     - Fighting or avoiding enemies

4. **Assets**
   - Replace placeholder cubes with models (`.glb` or `.gltf`) for trees, statues, enemies.

5. **UI/UX**
   - Add instruction messages before each level
   - Display timers, crystals collected, lives remaining

---

## 📌 Contribution Guidelines

- Each developer should pick **one missing feature** (enemy, bonus, teleporter, etc.).
- Implement it inside a modular class (e.g., `class Teleporter`).
- Add rendering + interaction logic.
- Update `loadLevel()` to support the new entity type.
- Test with one of the mission `.txt` files.

# Object Analysis Across Missions

## Common Objects Across All Missions
- **Hero** → Single coordinate `(x, y, z)` + direction. Comment: Not needed.
- **Floor** → Usually a range `(x1, y, z1) - (x2, y, z2)`. Comment: Only if special (e.g., FalseFloor).
- **Stair** → Single coordinate + direction (+ optional height). Comment: Not needed unless unique.
- **Exit** → Single coordinate. Comment: Not needed.
- **Teleporter** → Two positions `(start) - (destination)`. Comment: Helpful if target is far/non-obvious.
- **BonusCrystall** → Single coordinate. Comment: Not needed.
- **FalseFloor** → Single coordinate or short range. Comment: Recommended.
- **Lift** → Range `(start - end)`. Comment: Not needed.

## Objects Specific to Mission Desert
- **BonusShield, BonusLife, BonusFire, BonusBombs, BonusTime, BonusFreeze** → Single coordinates.
- **ElectroShock** → Single coordinate.
- **Mummy, Mage** → Single coordinate + direction.
- **Custom** (palm01.x, palm02.x, palm03.x, obelisk_desert.x, figure1.x) → Decorative, single coordinates.

## Objects Specific to Mission Forest
- **Monkey, Alligator** → Single coordinate + direction.
- **Barrel, Tree (tree01.x, tree02.x, tree03.x)** → Decorative, single coordinates.
- **Figure1.x** → Decorative, single coordinates.

## Objects Specific to Mission Rocks
- **Ghost, Bat, Scorpion** → Single coordinate + direction.
- **Stones, Obelisk_rocks.x, Stone_big.x** → Decorative, single coordinates.

## Positioning Rules
- **Single position (one coordinate)**: Hero, Exit, all Bonus items, all Enemies, all Custom decorative objects.
- **Range (two points)**: Floor, FalseFloor (sometimes), Lift, Teleporter.
- **Mixed**: Stairs (single coordinate but may include height parameter), Floor segments (single or range depending on size).

## Commenting Guidelines
- **Needs Comment:**
  - Teleporters (to explain where they lead).
  - FalseFloors (to warn about traps).
  - Special puzzle-critical objects.
- **Does NOT need Comment:**
  - Static objects (Floor, Exit, Hero).
  - Decorative objects (Custom, Trees, Barrels, Stones).
  - Common bonuses (Crystals, Shields, Life, Bombs, Time).

## Table: Object Presence, Position Type, Comment Need

| Object Type       | Desert | Forest | Rocks | Position Type       | Comment Needed?       |
|-------------------|--------|--------|-------|---------------------|-----------------------|
| Hero              | Yes    | Yes    | Yes   | Single              | No                    |
| Floor             | Yes    | Yes    | Yes   | Single or Range     | Only if special       |
| Stair             | Yes    | Yes    | Yes   | Single (+height)    | No                    |
| Exit              | Yes    | Yes    | Yes   | Single              | No                    |
| Teleporter        | Yes    | Some   | Some  | Range (start–end)   | Yes (if non-obvious)  |
| Lift              | Yes    | Yes    | Yes   | Range (start–end)   | No                    |
| FalseFloor        | Yes    | Yes    | Some  | Single or Range     | Yes                   |
| BonusCrystall     | Yes    | Yes    | Yes   | Single              | No                    |
| BonusShield       | Yes    | No     | No    | Single              | No                    |
| BonusLife         | Yes    | Some   | Yes   | Single              | No                    |
| BonusFire         | Yes    | No     | No    | Single              | No                    |
| BonusBombs        | Yes    | No     | Yes   | Single              | No                    |
| BonusTime         | Yes    | Some   | No    | Single              | No                    |
| BonusFreeze       | Some   | Yes    | Some  | Single              | No                    |
| ElectroShock      | Yes    | No     | Some  | Single              | No                    |
| Mummy             | Yes    | No     | No    | Single + direction  | No                    |
| Mage              | Yes    | Some   | No    | Single + direction  | No                    |
| Monkey            | No     | Yes    | No    | Single + direction  | No                    |
| Alligator         | No     | Yes    | No    | Single + direction  | No                    |
| Ghost             | No     | No     | Yes   | Single + direction  | No                    |
| Bat               | No     | No     | Yes   | Single + direction  | No                    |
| Scorpion          | No     | No     | Yes   | Single + direction  | No                    |
| Custom (Trees, Palms, Stones, Obelisks) | Yes | Yes | Yes | Single | No |






forest 8 Stair    (13, 7, 9)               right (2)
      { "type": "floor", "from": { "x": 7, "y": 6, "z": 7 }, "to": { "x": 12, "y": 6, "z": 12 } },
      { "type": "stair", "position": { "x": 13, "y": 7, "z": 9 }, "direction": "right", "count": "2" },
      { "type": "floor", "from": { "x": 15, "y": 10, "z": 9 }, "to": { "x": 17, "y": 10, "z": 9 } },


Desert 2 Stair    (0, -5, -14)               front  (4) , -5, -4, -3, -2
floors are -6 to 2
Floor    (-9, 2, 10) - (9, 2, 10)

Stair    (0, 3, -9)               front  (4)
Floor    (-4, 10, 5) - (4, 10, 5)