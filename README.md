# Magic World (Remake)

This is a remake project inspired by *Magic World* by Funny Arts.  
The goal is to rebuild the original missions (Desert, Forest, Rocks, etc.) using **Three.js** in modern browsers.

Background Images are from Magic World, a game by Funny Arts.
https://www.myplaycity.com/animal_retreat/
https://www.funnyarts.com/magicworldindex.php  
https://www.funnyarts.com/magicworld

https://imagetostl.com/convert/file/x/to/glb

---
## ✅ Current Implementation (in `index.html`)

- **Game Framework**
  - Three.js rendering (with textures and skybox)
  - HUD with moves, time, best score (localStorage)
  - Basic menu (Start, Restart, Win/Lose screens)
  - Smooth camera behind the ball (tank-style controls)
  - Ball movement with tile-based navigation
  - Fall detection (lose when falling below Y threshold)

- **Level Elements**
  - `floor`: Static tiles
  - `hero`: Starting position
  - `goal`: Exit tile (activates after collecting all crystals)
  - `lift`: Moving platforms (with pause at ends)
  - `teleporter`: Instant travel between two points
  - `stair`: Multi-height navigation
  - `bonuscrystall`: Collectible crystals (required to unlock the goal)
  - `falsefloor`: Implemented as basic collapsing floor (needs polish)
  - `ghost`: Placeholder object (basic presence only)
  - `custom`: Placeholder for decorative models

- **Game Flow**
  - Win/lose handling (fall detection, reaching goal)
  - Level progression (currently 2 demo levels with instructions in Level 2)
  - Collectibles system (goal unlocks after all crystals are collected)
  - Local best score tracking (time & moves)

- **Levels**
  - Currently **two demo levels** are hardcoded in JSON format inside `index.html`.

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

### Core Gameplay
- **Bonuses**
  - `BonusLife`, `BonusTime`, `BonusFreeze`, `BonusShield`
  - `BonusFire`, `BonusBombs`

- **Enemies**
  - `Mummy`, `Skeleton`, `Monkey`, `Bat`, `Alligator`, `Scorpion`, `Mage`

- **Obstacles**
  - `ElectroShock` (damage hazard)
  - `FalseFloor` (implemented but not collapsing yet)

- **Environment**
  - Decorative models (`palm01.x`, `obelisk_desert.x`, `tree01.x`, `stone_big.x`, etc.)
  - Animations and sounds missing

### Gameplay Logic
- `DefaultTime` countdown per level
- Mission `Instructions` display before levels
- Enemy AI (movement, detection, attack)
- Bonus effects (extra time, lives, shields, freeze, bombs, etc.)
- Multi-level terrain with full stair support
- Teleporter visual effects

### Game Structure
- Parser for `.txt` mission files (Desert, Forest, Rocks)
- Progression across multiple worlds (Desert → Forest → Rocks)
- Sound effects and background music

---

## 🚀 Roadmap

1. **Parser**
   - Load `.txt` mission files into JSON level data.

2. **Entity System**
   - Create modular classes for `Enemy`, `Bonus`, `Obstacle`, etc.
   - Each handles rendering + behavior.

3. **Game Loop Integration**
   - Extend collision/interaction detection:
     - Collect bonuses
     - Trigger hazards
     - Handle enemy movement

4. **UI/UX**
   - Display mission instructions before each level
   - Show timers, crystals collected, lives remaining

5. **World Expansion**
   - Add support for Desert, Forest, Rocks missions
   - Replace placeholders with actual `.glb` or `.gltf` models

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

---

## ✅❌ TODO Checklist

### Implemented
- [x] Floors, hero, goal
- [x] Lifts & teleporters
- [x] Stairs (basic support)
- [x] Crystals collection system
- [x] False floors (basic, not collapsing yet)
- [x] Ghost placeholder
- [x] HUD (moves, time, best scores)
- [x] Menu + level restart
- [x] Two demo levels (hardcoded)

### Pending
- [ ] Parser for mission `.txt` files
- [ ] Full false floor collapsing logic
- [ ] Timer per level (`DefaultTime`)
- [ ] Mission instructions display
- [ ] Bonus items (`Life`, `Time`, `Shield`, `Fire`, `Bombs`, `Freeze`)
- [ ] Enemy AI (`Mummy`, `Skeleton`, `Monkey`, `Bat`, `Alligator`, `Scorpion`, `Mage`)
- [ ] Hazard (`ElectroShock`)
- [ ] Multi-world support (Desert, Forest, Rocks)
- [ ] Decorative models & proper `.glb` assets
- [ ] Sounds & animations

---

## 🔍 Summary

- **Added so far**: floors, hero, goal, lifts, teleporters, stairs, crystals, ghost (basic), false floors (basic), HUD, level system.  
- **Pending**: most enemies, all bonuses, ElectroShock, time system, mission instructions, parser, multi-world expansion.  
- **Next steps**: implement parser, add bonus/enemy systems, connect all mission worlds.


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



🎭 3D Models (.x files)
Root folder:
alligator.x, bat.x, bomb.x, bombbox.x, freeze.x, ghost.x, hero_die.x, hero_jump.x, hero_stay1.x, hero_stay2.x, life.x, mage.x, magicbox.x, monkey.x, mummy.x, scorpion.x, shield.x, time.x
Desert folder:
palm01.x, palm02.x, palm03.x, teleporter.x, exit.x, shock.x, floor.x, stair.x, lift.x, figure1.x, falsefloor.x, crystall.x, obelisk_desert.x
Forest folder:
tree01.x, tree02.x, tree03.x, barrel.x, teleporter.x, exit.x, floor.x, stair.x, lift.x, figure1.x, falsefloor.x, crystall.x, cust1.x
Rocks folder:
obelisk_rocks.x, stones1.x, stones2.x, stone_big.x, teleporter.x, exit.x, shock.x, floor.x, stair.x, lift.x, figure1.x, falsefloor.x, crystall.x
🖼️ Textures (image files)
Root:
BATDIF1.jpg, Bk_obj01.jpg, button_selected.jpg, explosion_par.jpg, explosion_ring.jpg, explosion.jpg, flame.jpg, flash.jpg, ghostdif1.png, HERO01.jpg, magic.jpg, ORANGDIF4.jpg, particle.jpg, REPTDIF4.jpg, scorpion.jpg, slim.jpg, … (+ UI PNGs like btn_common.png, stats.png)
Desert:
bg_top.jpg, bg_left.jpg, bg_back.jpg, bg_right.jpg, bg_front.jpg, bg_bottom.jpg, Bk_obj01.jpg, bk_scn02.jpg, Bk_scn22.png
Forest:
bg_top.jpg, bg_left.jpg, bg_back.jpg, bg_right.jpg, bg_front.jpg, bg_bottom.jpg, Bk_obj01.jpg, Bk_scn01.jpg, Bk_scn12.png, green1.jpg
Rocks:
bg_top.jpg, bg_left.jpg, bg_back.jpg, bg_right.jpg, bg_front.jpg, bg_bottom.jpg, Bk_obj01.jpg, bk_scn03.jpg, Bk_scn32.jpg, Bk_scn32.png

# .X Models and Textures Mapping

## Step to add JPG texture in Blender
1. Select your 3D model (imported `.x` file).
2. Switch to **Shading tab** (Shader Editor).
3. Ensure your object has a material. If not, click **New**.
4. In the Shader Editor, add a **Image Texture node** (`Shift + A → Texture → Image Texture`).
5. Open your `.jpg/.png/.tga/.bmp` file inside this node.
6. Connect the **Color output** of the Image Texture node to the **Base Color input** of the Principled BSDF shader.

That’s how you give color to your `.x` model using `.jpg` texture in Blender.

---

## Texture-to-Model Mapping from Graphics.zip

### 🐊 Characters & Creatures
- **alligator.x** → `REPTDIF4.jpg`
- **bat.x** → `BATDIF1.jpg`
- **ghost.x** → `ghostdif1.png`
- **hero_die.x / hero_jump.x / hero_stay1.x / hero_stay2.x** → `HERO01.jpg`
- **mage.x** → `ORANGDIF4.jpg`
- **monkey.x** → `ORANGDIF4.jpg`
- **mummy.x** → `Bk_obj01.jpg`
- **scorpion.x** → `scorpion.jpg`

### 🧰 Items & Effects
- **bomb.x** → *(no explicit texture, maybe just geometry or particle applied in-game)*
- **bombbox.x** → *(no texture found)*
- **freeze.x** → *(no texture found)*
- **life.x** → *(no texture found)*
- **magicbox.x** → `magic.jpg`
- **shield.x** → `Bk_obj01.jpg`
- **time.x** → *(no texture found)*

### 🌴 Desert Assets
- **palm01.x / palm02.x / palm03.x** → `Bk_obj01.jpg`
- **teleporter.x** → `Bk_obj01.jpg`
- **exit.x** → `Bk_obj01.jpg`
- **shock.x** → `Bk_obj01.jpg`
- **floor.x** → `Bk_obj01.jpg`
- **stair.x** → `Bk_obj01.jpg`
- **lift.x** → `Bk_obj01.jpg`
- **figure1.x** → `Bk_obj01.jpg`
- **falsefloor.x** → `Bk_obj01.jpg`
- **crystall.x** → `Bk_obj01.jpg`
- **obelisk_desert.x** → `Bk_obj01.jpg`

### 🌲 Forest Assets
- **tree01.x / tree02.x / tree03.x** → `green1.jpg`
- **barrel.x** → `Bk_obj01.jpg`
- **teleporter.x / exit.x / floor.x / stair.x / lift.x / figure1.x / falsefloor.x / crystall.x / cust1.x** → `Bk_obj01.jpg`

### 🪨 Rock Assets
- **obelisk_rocks.x / stones1.x / stones2.x / stone_big.x** → `Bk_obj01.jpg`
- **teleporter.x / exit.x / shock.x / floor.x / stair.x / lift.x / figure1.x / falsefloor.x / crystall.x** → `Bk_obj01.jpg`

---

✅ Now you have a clear mapping of which `.jpg/.png` belongs to each `.x` model.




forest 8 Stair    (13, 7, 9)               right (2)
      { "type": "floor", "from": { "x": 7, "y": 6, "z": 7 }, "to": { "x": 12, "y": 6, "z": 12 } },
      { "type": "stair", "position": { "x": 13, "y": 7, "z": 9 }, "direction": "right", "count": "2" },
      { "type": "floor", "from": { "x": 15, "y": 10, "z": 9 }, "to": { "x": 17, "y": 10, "z": 9 } },


Desert 2 Stair    (0, -5, -14)               front  (4) , -5, -4, -3, -2
floors are -6 to 2
Floor    (-9, 2, 10) - (9, 2, 10)

Stair    (0, 3, -9)               front  (4)
Floor    (-4, 10, 5) - (4, 10, 5)