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

