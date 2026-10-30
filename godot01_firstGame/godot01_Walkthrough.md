# Godot 01 Walkthrough — First Game Tutorial (2D Platformer)

**Read this first. Then open `godot01_lastname.md` and do the work as you go.**

---

## Table of Contents

- [What you're doing](#what-youre-doing)
- [The video](#the-video)
- [Before you start: set up your repo](#before-you-start-set-up-your-repo)
- [Things that look different in our version of Godot](#things-that-look-different-in-our-version-of-godot)
- [Day 1 — Player, world, and platforms](#day-1--player-world-and-platforms)
  - [Checkpoint 1: The knight moves and jumps](#checkpoint-1-the-knight-moves-and-jumps)
  - [Checkpoint 2: A level made of tiles](#checkpoint-2-a-level-made-of-tiles)
  - [Checkpoint 3: Platforms, one still and one moving](#checkpoint-3-platforms-one-still-and-one-moving)
- [Day 2 — Coins, dying, and an enemy](#day-2--coins-dying-and-an-enemy)
  - [Checkpoint 4: Coins you can pick up](#checkpoint-4-coins-you-can-pick-up)
  - [Checkpoint 5: Falling restarts the game](#checkpoint-5-falling-restarts-the-game)
  - [Checkpoint 6: A slime that patrols and kills you](#checkpoint-6-a-slime-that-patrols-and-kills-you)
- [Day 3 — Animation, score, sound, and export](#day-3--animation-score-sound-and-export)
  - [Checkpoint 7: The knight runs, jumps, and faces the right way](#checkpoint-7-the-knight-runs-jumps-and-faces-the-right-way)
  - [Checkpoint 8: Signs and a coin score](#checkpoint-8-signs-and-a-coin-score)
  - [Checkpoint 9: Music and a coin sound](#checkpoint-9-music-and-a-coin-sound)
  - [Checkpoint 10: Export (try it)](#checkpoint-10-export-try-it)
- [If you finish early](#if-you-finish-early)
- [Words to know](#words-to-know)

---

## What you're doing

You are going to build a small **2D platformer** in Godot by following a video. A knight runs, jumps, rides a moving platform, collects coins, and dies if she falls or touches a slime.

The point is not this game. The point is that this one small game uses almost everything you need for any 2D game:

- a player that moves, jumps, and animates
- a level built from tiles
- moving objects
- pickups and a score
- an enemy
- dying and restarting
- text, music, and sound effects

This is **individual** work. Everyone on a team does this tutorial on their own computer. Your team's own game starts after this.

You have **three Thursdays** for this. Push to GitHub at every checkpoint, not just at the end.

---

## The video

**How to make a Video Game – Godot Beginner Tutorial** — Brackeys
https://www.youtube.com/watch?v=LOhfqjmasi0

The video is about 1 hour 16 minutes. Plan on it taking you three class periods, because you will pause, type, fix mistakes, and try things.

| Time | Section | Day |
|---|---|---|
| 0:00 | Intro to Godot — **skip the install part, you already have Godot** | 1 |
| 2:16 | What should we make? | 1 |
| 3:22 | Cooking with Assets | 1 |
| 5:32 | How Godot works | 1 |
| 7:31 | Player 1.0 | 1 |
| 16:57 | Worldbuilding 1.0 | 1 |
| 23:28 | Platforms | 1 |
| 28:20 | Pickups | 2 |
| 34:20 | Dying 1.0 | 2 |
| 40:14 | Worldbuilding 2.0 | 2 |
| 41:58 | Enemy | 2 |
| 51:06 | Dying 2.0 | 2 |
| 52:56 | Player 2.0 | 3 |
| 1:00:43 | Text | 3 |
| 1:02:35 | Score | 3 |
| 1:07:46 | Audio | 3 |
| 1:14:16 | Export | 3 |
| 1:15:41 | Where to go from here? | 3 |

The video uses a free asset pack: the knight, slime, coins, tiles, a pixel font, sounds, and music. **You don't need the link in the video description.** The whole pack is already in this repo, in the **`brackeysAssets`** folder next to this file. Download it from there. At 3:22, when he drags his asset folders into Godot, drag in the folders from `brackeysAssets` instead.

Use his art for this tutorial. You'll make your own for your team's game.

At about 2:30 there is an ad for a website called Zenva. You can skip it.

---

## Before you start: set up your repo

Godot keeps a hidden folder called `.godot` with files it rebuilds by itself. That folder should **not** go to GitHub.

1. If you don't already have one, use GitHub Desktop to create a repository named **`gameDev`**. (If you did the Unity tutorial already, use the same `gameDev` repo.)
2. In the `gameDev` folder (not inside a game project), make sure there is a file named `.gitignore` with this in it. If the file is already there from Unity, just add the `.godot/` line.

```
# Godot files that should not go to GitHub
.godot/

# Unity files that should not go to GitHub
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
[Ll]ogs/
[Uu]ser[Ss]ettings/
.vs/
.vscode/
*.csproj
*.sln
```

3. In Godot's Project Manager, click **Create** (some versions say **New**). Put the project **inside** your `gameDev` folder and name it **`FirstGame_Lastname`** (your last name). Leave **Version Control Metadata** set to **Git**.
4. Copy `godot01_lastname.md` into the `gameDev` folder and rename it with your last name.

**Check:** In GitHub Desktop, the list of changed files should be small — things like `project.godot`, `.tscn` files, `.gd` files, and your assets. If you see a `.godot` folder in the list, your `.gitignore` is not working. Stop and ask before you push.

---

## Things that look different in our version of Godot

The video was made in **Godot 4.2**. Our lab has a newer version of Godot 4. Most things are the same, but these will trip you up. Come back to this list when something doesn't match the video.

**1. The `TileMap` node is now `TileMapLayer`.** (Worldbuilding 1.0, at 16:57.)
When you search for "tile map," you'll see `TileMap` with a line through it or a warning. **Pick `TileMapLayer` instead.** Everything else in that section works the same: make a new Tile Set, set the tile size, drag in `world_tileset.png`, add a physics layer, and paint.

**2. Background tiles are a second node, not a "layer" setting.** (Worldbuilding 2.0, at 40:14.)
In the video he adds a background layer inside the TileMap. In our version, each layer is its own node:
- Rename your first `TileMapLayer` to **Mid**.
- Add a second `TileMapLayer` and name it **Background**.
- Drag **Background** above **Mid** in the Scene tree. Nodes higher in the tree are drawn first, so they end up behind.
- Give Background the same tile set: click the **Tile Set** box on **Mid**, choose **Copy**, then click the **Tile Set** box on **Background** and choose **Paste**.
- Select **Background** in the tree before you paint background tiles.

**3. The player movement script looks a little different.** (Player 1.0 and Player 2.0.)
When you make the player script from the template, our version uses `get_gravity()` instead of the `var gravity = ...` line he talks about at 53:00. **That's fine.** It does the same job. Change `SPEED` and `JUMP_VELOCITY` like he does, and in Player 2.0 change `"ui_accept"`, `"ui_left"`, and `"ui_right"` to your own action names.

**4. Function lines have extra words in them.** When Godot makes a function for you, it may look like this:

```gdscript
func _on_body_entered(body: Node2D) -> void:
```

The video may show just `func _on_body_entered(body):`. The extra `: Node2D` and `-> void` describe what kind of data goes in and comes out. Leave them. Your code still works the same.

**5. Indentation matters.** GDScript uses **tabs** to show which lines belong inside a function or an `if`. If you get an error about indentation, check that the line is tabbed in the same amount as the lines around it.

---

## Day 1 — Player, world, and platforms

Watch **0:00 to 28:20**. Pause and do each thing as he does it.

Before you start building, make the **assets**, **scripts**, and **scenes** folders like he does at 3:22. Keeping files organized now saves you later.

### Checkpoint 1: The knight moves and jumps

Stop at **16:57**.

You should have:
- a **game** scene (your main scene) and a **player** scene
- the player built from a **CharacterBody2D**, an **AnimatedSprite2D** playing the idle animation, and a **CollisionShape2D**
- pixel art that looks crisp, not blurry (Project Settings → Rendering → Textures → Default Texture Filter → **Nearest**)
- a **Camera2D** zoomed in
- the knight moves with the arrow keys and jumps with the spacebar

**Push to GitHub.** Commit message: `Checkpoint 1 - knight moves`

### Checkpoint 2: A level made of tiles

Stop at **23:28**.

You should have:
- a **TileMapLayer** with the tile set painted into a level (see [item 1 above](#things-that-look-different-in-our-version-of-godot))
- a physics layer painted on the solid tiles so the knight stands on them
- trees and bushes the knight walks through
- the camera moved **under** the player so it follows her, with position smoothing on

**Push to GitHub.** Commit message: `Checkpoint 2 - tile level`

### Checkpoint 3: Platforms, one still and one moving

Stop at **28:20**.

You should have:
- a **platform** scene built from an **AnimatableBody2D**, a **Sprite2D**, and a **CollisionShape2D**
- **One Way Collision** turned on, so you can jump up through it from below
- the player's **Z Index** set to 5, so she draws in front of platforms
- one platform that stays still and one that moves back and forth with an **AnimationPlayer**

**Push to GitHub.** Commit message: `Checkpoint 3 - platforms`

Answer the Day 1 questions in your `godot01_lastname.md` before you leave.

---

## Day 2 — Coins, dying, and an enemy

Watch **28:20 to 52:56**.

### Checkpoint 4: Coins you can pick up

Stop at **34:20**.

You should have:
- a **coin** scene built from an **Area2D**, an animated sprite, and a collision shape
- several coins placed around the level
- a **signal** (`body_entered`) connected to the coin's script
- the player on collision **layer 2**, and the coin's **mask** set to 2, so only the player picks up coins
- the coin disappears with `queue_free()` when the knight touches it

**Push to GitHub.** Commit message: `Checkpoint 4 - coins`

### Checkpoint 5: Falling restarts the game

Stop at **40:14**.

You should have:
- a **camera limit** on the bottom, so the camera stops when you fall
- a reusable **killzone** scene (Area2D, mask 2, no collision shape inside the scene itself)
- a killzone in your game scene with a **WorldBoundaryShape2D** below the level
- a **Timer** that waits, then reloads the scene with `get_tree().reload_current_scene()`

**Push to GitHub.** Commit message: `Checkpoint 5 - dying restarts`

### Checkpoint 6: A slime that patrols and kills you

Stop at **52:56**.

You should have:
- your nodes organized under group nodes like **Coins** and **Platforms**
- a background tile layer (see [item 2 above](#things-that-look-different-in-our-version-of-godot)) and more level
- a **slime** scene that uses your **killzone** scene, so touching it kills you
- a slime script that moves with `direction * SPEED * delta`
- two **RayCast2D** nodes that make the slime turn around at walls, and flip its sprite
- dying in **slow motion** (`Engine.time_scale`), and the knight falls through the floor when she dies

**Push to GitHub.** Commit message: `Checkpoint 6 - slime enemy`

Finish the Day 2 questions in your `godot01_lastname.md` and push.

---

## Day 3 — Animation, score, sound, and export

Watch **52:56 to the end**.

### Checkpoint 7: The knight runs, jumps, and faces the right way

Stop at **1:00:43**.

You should have:
- **Input Map** actions named `jump`, `move_left`, and `move_right`, with both arrow keys and WASD
- the player script using your action names instead of the `ui_` ones
- the knight flips to face left and right
- **idle**, **run**, and **jump** animations that play at the right times, using `is_on_floor()`

**Push to GitHub.** Commit message: `Checkpoint 7 - player animation`

### Checkpoint 8: Signs and a coin score

Stop at **1:07:46**.

You should have:
- **Label** nodes in the level with hints, using the pixel font at size 8, 16, 24, and so on
- a **GameManager** node with a script that has a `score` variable and an `add_point()` function
- the GameManager set to **Access as Unique Name** (the `%` sign)
- coins that call `game_manager.add_point()`
- a score label that says how many coins you collected, using `str()` to turn the number into text

**Push to GitHub.** Commit message: `Checkpoint 8 - score`

### Checkpoint 9: Music and a coin sound

Stop at **1:14:16**.

You should have:
- background music that loops and doesn't restart when you die (it's an **Autoload**)
- **Music** and **SFX** buses in the Audio tab, with the music turned down
- a coin pickup sound
- an **AnimationPlayer** on the coin with a **pickup** animation that hides the coin, turns off its collision, plays the sound, and then calls `queue_free`

**Push to GitHub.** Commit message: `Checkpoint 9 - audio`

### Checkpoint 10: Export (try it)

Watch **1:14:16 to 1:15:41**.

Exporting turns your project into a `.exe` that runs without Godot. First Godot has to download something called **export templates**, which is a big download.

Give it a try. It might work on the school network and it might not. If the download fails or takes forever, skip this step — you're still done with the tutorial.

**Do not push the `.exe` to GitHub.**

---

## If you finish early

Watch **1:15:41 to the end**. He gives you a list of things to try. Pick **at least one** and add it. Some good ones:

- **Expand the level** using only what you know now.
- **Add spikes or a trap.** It's just another use of your killzone scene.
- **Add an effect** when you pick up a coin, like a short animation.
- **Add a double jump.**
- **Make your own sprite** in Piskel and swap it in for the coin or the slime.

Write down what you added in your `godot01_lastname.md`.

---

## Words to know

| Word | What it means |
|---|---|
| **Node** | The building block of everything in Godot. Each node does one job — shows a picture, plays a sound, adds physics, detects an area. |
| **Scene** | A group of nodes saved together so you can reuse it — the player, a coin, an enemy, a whole level. |
| **Scene tree** | The list of nodes in a scene, shown like a family tree. The top node is the **root**. |
| **Child node** | A node inside another node. It moves with its parent (like the camera under the player). |
| **Inspector** | The panel where you see and change the settings of the selected node. |
| **CharacterBody2D** | A physics node for something you move with code, like the player. |
| **StaticBody2D** | A physics node for things that don't move, like ground. |
| **AnimatableBody2D** | A physics node for things that move by animation, like a moving platform. |
| **Area2D** | A node that detects when something enters it, without blocking it. Used for coins and the killzone. |
| **CollisionShape2D** | The invisible shape Godot uses to check if things touch. Physics nodes need one. |
| **TileMapLayer / Tile set** | The tile set is the collection of tiles. The TileMapLayer is the node you paint them onto. |
| **Sprite sheet** | One image with many frames or pictures packed into a grid. |
| **GDScript** | Godot's programming language. It looks a lot like Python. |
| **`_ready()`** | Runs **once**, when the node first enters the game. |
| **`_process(delta)`** | Runs **every frame**. |
| **`_physics_process(delta)`** | Runs at a steady 60 times a second. Used for anything with physics, like player movement. |
| **delta** | The time since the last frame. Multiply speed by it so the game runs the same on fast and slow computers. |
| **Signal** | A message a node sends when something happens, like `body_entered` or `timeout`. You connect it to a function. |
| **Collision layer / mask** | **Layer** = what layer this thing is on. **Mask** = what layers it looks for. |
| **`queue_free()`** | Removes a node from the game. |
| **RayCast2D** | An invisible line that checks if it hits something, like a wall. |
| **Z Index** | Draw order. Higher numbers are drawn on top. |
| **const** | A value that never changes while the game runs, like `SPEED`. |
| **var** | A variable — a value that can change, like `direction` or `score`. |
| **Unique name (%)** | Lets you find a one-of-a-kind node, like the GameManager, by name instead of by path. |
| **Autoload** | A scene or script that stays loaded the whole game, even when a scene reloads. |
| **`str()`** | Turns a number into text so it can go in a label. |
