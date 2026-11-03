# Godot 02 How-To Guide

This guide shows how to build each **tutorial part** on your checklist. The numbers match the checklist in `godot02_gamePlan.md`.

Each part has:
- **What it is**
- **Nodes you need**
- **Steps**
- **Script** (with comments) when there is one
- **Check it** — how to know it works
- **RPG changes** when an RPG is different

You built all of this once in the tutorial. This guide is here so you don't have to scrub through the video to find it again.

---

## Table of Contents

1. [Player with animations](#1-player-with-animations)
2. [Your own controls](#2-your-own-controls)
3. [Camera that follows the player](#3-camera-that-follows-the-player)
4. [Tile level with a background layer](#4-tile-level-with-a-background-layer)
5. [Moving platform](#5-moving-platform)
6. [Pickups](#6-pickups)
7. [Score on screen](#7-score-on-screen)
8. [Killzone](#8-killzone)
9. [Enemy that moves between walls](#9-enemy-that-moves-between-walls)
10. [Signs with a pixel font](#10-signs-with-a-pixel-font)
11. [Music that keeps playing](#11-music-that-keeps-playing)
12. [Pickup sound](#12-pickup-sound)

---

## 1. Player with animations

### What it is

The character the player controls. It runs, jumps, plays the right animation, and faces the way it is moving.

### Nodes you need

```
Player (CharacterBody2D)
├── AnimatedSprite2D
└── CollisionShape2D
```

| Node | What it does |
|---|---|
| **CharacterBody2D** | A physics body moved by a script. It stops when it hits walls and floors. |
| **AnimatedSprite2D** | Shows your sprite sheet and plays animations. |
| **CollisionShape2D** | The shape the physics engine uses. Without it, the player falls through everything. |

### Steps

**Make the scene**

1. Click the **+** tab at the top to make a new scene.
2. Click **Other Node**, search for **CharacterBody2D**, and click **Create**.
3. Rename it **Player**.

**Add the sprite**

4. With Player selected, press **Ctrl+A**, search for **AnimatedSprite2D**, and click **Create**.
5. In the Inspector, click **Sprite Frames → New SpriteFrames**. Click it again to open the SpriteFrames panel at the bottom.
6. In that panel, click the **Add frames from sprite sheet** button (the grid icon).
7. Pick your player sprite sheet from your `assets/sprites` folder.
8. Set **Horizontal** and **Vertical** to match how many frames are across and down in your sheet.
9. Click the frames for your idle animation in order, then click **Add Frames**.
10. Rename the animation from `default` to **idle**. Set FPS to **10**. Turn on **Autoplay on Load** (the play icon with an A).

**Make the art crisp**

11. Go to **Project → Project Settings → Rendering → Textures → Default Texture Filter** and set it to **Nearest**. (You only have to do this once per project.)

**Add the other animations**

12. In the SpriteFrames panel, click **Add Animation**. Name it **run**. Add the run frames the same way.
13. Add another animation named **jump**. One frame is fine.

**Add the collision shape**

14. Select Player. Press **Ctrl+A**, add a **CollisionShape2D**.
15. In the Inspector, set **Shape → New CircleShape2D** (or CapsuleShape2D).
16. Drag it to cover the player's body. Make it a little **smaller** than the sprite. A shape that's too big feels unfair to play.

**Draw the player on top of everything**

17. Select Player. In the Inspector, go to **Ordering → Z Index** and set it to **5**.

**Put the player on its own collision layer**

18. Select Player. In the Inspector, go to **Collision → Layer**. Turn **off** layer 1 and turn **on** layer 2. (Pickups and the killzone will look for layer 2. See #6 and #8.)

**Save it**

19. Press **Ctrl+S** and save it as `player.tscn` in your `scenes` folder.
20. Open `game.tscn` and drag `player.tscn` into the level.

**Add the script**

21. In `player.tscn`, select Player and click the **Attach Script** button (scroll with a +).
22. Template: **CharacterBody2D: Basic Movement**. Path: `res://scripts/player.gd`. Click **Create**.
23. Replace the code with the script below.

### Script — `player.gd`

```gdscript
extends CharacterBody2D

# How fast the player runs, in pixels per second
const SPEED = 130.0
# How hard the player jumps. Negative means up in Godot.
const JUMP_VELOCITY = -300.0

# Gets the AnimatedSprite2D node so we can flip it and play animations.
# Tip: drag the node into the script and hold Ctrl when you let go.
@onready var animated_sprite = $AnimatedSprite2D


# Runs 60 times a second. Use this for anything with physics.
func _physics_process(delta: float) -> void:
	# --- GRAVITY ---
	# If we're in the air, pull the player down.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# --- JUMP ---
	# Only jump if the button was just pressed AND we're on the ground.
	if Input.is_action_just_pressed("jump") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# --- INPUT DIRECTION ---
	# -1 = left, 0 = not moving, 1 = right
	var direction := Input.get_axis("move_left", "move_right")

	# --- FACE THE RIGHT WAY ---
	if direction > 0:
		animated_sprite.flip_h = false   # facing right
	elif direction < 0:
		animated_sprite.flip_h = true    # facing left

	# --- PICK THE ANIMATION ---
	if is_on_floor():
		if direction == 0:
			animated_sprite.play("idle")
		else:
			animated_sprite.play("run")
	else:
		animated_sprite.play("jump")      # in the air

	# --- MOVE ---
	if direction:
		velocity.x = direction * SPEED
	else:
		# Slow to a stop when no key is pressed
		velocity.x = move_toward(velocity.x, 0, SPEED)

	# Actually moves the player and handles hitting walls and floors
	move_and_slide()
```

> **This script uses `jump`, `move_left`, and `move_right`.** Those don't exist until you make them in [#2 Your own controls](#2-your-own-controls). Do #2 next, or the player won't move.

### Check it

Press **F5**. The player stands on the ground, runs left and right, faces the right way, jumps, and plays idle, run, and jump at the right times.

If the player falls forever, you don't have ground yet. Do [#4 Tile level](#4-tile-level-with-a-background-layer).

### RPG changes

An RPG is seen from above, so there's no gravity and no jumping.

- Make **walk_down**, **walk_up**, and **walk_side** animations instead of run and jump. Keep **idle**.
- In #2, also make **move_up** and **move_down** actions.
- Use this script instead:

```gdscript
extends CharacterBody2D

const SPEED = 80.0

@onready var animated_sprite = $AnimatedSprite2D


func _physics_process(delta: float) -> void:
	# get_vector reads all four directions at once.
	# It gives back a direction like (1, 0) for right or (0, -1) for up.
	var direction := Input.get_vector("move_left", "move_right", "move_up", "move_down")

	# Move in that direction. No gravity in an RPG.
	velocity = direction * SPEED

	# --- PICK THE ANIMATION ---
	if direction == Vector2.ZERO:
		animated_sprite.play("idle")
	elif abs(direction.x) > abs(direction.y):
		# Moving more sideways than up or down
		animated_sprite.play("walk_side")
		animated_sprite.flip_h = direction.x < 0   # flip when moving left
	elif direction.y < 0:
		animated_sprite.play("walk_up")
	else:
		animated_sprite.play("walk_down")

	move_and_slide()
```

---

## 2. Your own controls

### What it is

Godot uses **actions** for input. An action is a name like `jump`. You connect keys to it. Your scripts check the action, not the key, so one action can have more than one key (Space **and** W for jump).

### Nodes you need

None. This is a project setting.

### Steps

1. Go to **Project → Project Settings → Input Map**.
2. In the **Add New Action** box, type **jump** and click **Add**.
3. Do the same for **move_left** and **move_right**.
4. Click the **+** next to `jump`. Press **Space**, then click **OK**.
5. Click the **+** next to `move_left`. Press the **Left arrow**, then **OK**. Click **+** again and press **A**, then **OK**.
6. Do the same for `move_right` with the **Right arrow** and **D**.

| Action | Keys |
|---|---|
| `jump` | Space (add W or Up arrow if you want) |
| `move_left` | Left arrow, A |
| `move_right` | Right arrow, D |

**Spelling matters.** The names here must match the names in `player.gd` exactly, including the underscore.

### Check it

Press **F5**. The player moves with the arrow keys **and** with A and D, and jumps with Space.

If you get an error like *"The InputMap action 'jump' doesn't exist"*, the name is spelled differently in the Input Map and the script.

### RPG changes

Also add **move_up** (Up arrow, W) and **move_down** (Down arrow, S). You don't need `jump`.

---

## 3. Camera that follows the player

### What it is

The camera decides what part of the level is on screen. Putting it **inside** the player makes it follow the player.

### Nodes you need

```
Player (CharacterBody2D)
├── AnimatedSprite2D
├── CollisionShape2D
└── Camera2D          ← add this
```

### Steps

1. Open `game.tscn`.
2. Press **Ctrl+A**, add a **Camera2D**.
3. Drag the Camera2D **onto the Player** in the Scene list so it becomes a child of the Player.
4. In the Inspector, set **Zoom** to **4** and **4**.
5. Make sure the camera's **Position** is **0, 0** so it sits right on the player.
6. Turn on **Position Smoothing → Enabled**. Leave the speed at 5.

**Stop the camera from following the player down a pit**

7. Press **R** for the ruler tool. Measure from your ground down to where the camera should stop. Around **120** pixels is a good start.
8. Press **Q** to go back to the select tool.
9. Select the Camera2D. Under **Limit**, set **Bottom** to the number you measured.
10. Turn on **Limit → Smoothed**.

### Check it

Press **F5**. The camera follows the player smoothly. When the player falls off the level, the camera stays put and the player drops out of view.

### RPG changes

Set limits on all four sides (**Left**, **Top**, **Right**, **Bottom**) to the edges of your map, so the camera never shows empty space past the map.

---

## 4. Tile level with a background layer

### What it is

You build the level by painting tiles on a grid. A **TileSet** is the collection of tiles. A **TileMapLayer** is the node you paint on. You use two: one for the solid ground (**Mid**) and one for scenery behind it (**Background**).

### Nodes you need

```
Game (Node2D)
├── Background (TileMapLayer)   ← drawn first, behind everything
├── Mid (TileMapLayer)          ← the ground the player stands on
└── ...
```

### Steps

**Make the tile layer**

1. Open `game.tscn`. Press **Ctrl+A**, search for **TileMapLayer**, and click **Create**. Rename it **Mid**.
2. In the Inspector, click **Tile Set → New TileSet**.
3. Click the TileSet to open it. Check that **Tile Size** is **16 × 16**.

**Load your tiles**

4. At the bottom of the screen, click the **TileSet** tab.
5. Drag your tile sheet from `assets/sprites` into the left side of that panel.
6. When it asks to create tiles automatically, click **Yes**.
7. If a big object (like a tree top) got split into small tiles, use the **eraser** to remove them. Then hold **Shift** and drag to make it one big tile.

**Make the ground solid**

8. Select **Mid**. In the Inspector, open the TileSet, then **Physics Layers → Add Element**.
9. In the **TileSet** tab at the bottom, click **Paint**. Under **Paint Properties**, pick **Physics Layer 0**.
10. Click every tile the player should stand on or bump into. Don't paint trees, grass, or decorations.
11. For a tile that should only be **partly** solid (like a thin bridge), drag the points of the shape on the left to fit just the solid part, then paint it on.

**Paint the level**

12. Click the **TileMap** tab at the bottom.
13. Pick a tile and click in the level to paint. **Right-click** removes a tile.
14. Block out the main path first. Leave gaps for platforms and enemies. Decorate last.
15. Drag **Mid** to the top of the Scene list so it's easy to find.

**Add the background layer**

16. Add a **second TileMapLayer**. Name it **Background**.
17. Select **Mid**, click its **Tile Set** box → **Copy**. Select **Background**, click its **Tile Set** box → **Paste**. Now both layers use the same tiles.
18. Drag **Background** **above** Mid in the Scene list. Things higher in the list are drawn first, so Background ends up behind.
19. With **Background** selected, paint scenery behind the level. The **rectangle tool** fills big areas quickly.

### Check it

Press **F5**. The player stands on the ground, bumps into walls, and walks **through** trees and background tiles.

If the player falls through the ground, you skipped steps 8–10 for that tile.

### RPG changes

Paint physics on **walls, water, and trees** (things you can't walk through), not on the floor. Without gravity, the player walks on top of the floor tiles and stops at the walls.

---

## 5. Moving platform

### What it is

A platform the player can stand on. You make one platform scene, then use it in two ways: some sit still, and some move back and forth.

### Nodes you need

```
Platform (AnimatableBody2D)
├── Sprite2D
└── CollisionShape2D
```

| Node | What it does |
|---|---|
| **AnimatableBody2D** | A physics body you can move with animation, and it still pushes and carries the player. |
| **Sprite2D** | A single image. No animation needed. |

### Steps

**Make the platform scene**

1. New scene. Root node: **AnimatableBody2D**. Rename it **Platform**.
2. Add a **Sprite2D**. Drag your platform image into **Texture**.
3. If your image has several platforms on it, turn on **Region → Enabled**, click **Edit Region**, set **Snap Mode** to **Pixel Snap**, and drag a box around one platform.
4. Add a **CollisionShape2D**. Set **Shape → New RectangleShape2D** and drag it to fit the top of the platform.
5. On the CollisionShape2D, turn on **One Way Collision**. Now the player can jump up through it from below and land on top.
6. Save as `platform.tscn`.

**Make one move**

7. Open `game.tscn` and drag in two platforms. One stays still.
8. Select the one that should move. Press **Ctrl+A** and add an **AnimationPlayer** as its child.
9. In the Animation panel at the bottom, click **Animation → New**. Name it **move**.
10. Select the platform. In the Inspector, find **Transform → Position** and click the **key** icon next to it. Click **Create**.
11. Move the timeline to **1.5** seconds. Set the animation **length** (the box with the clock icon) to **1.5** too.
12. Drag the platform to where it should end up. Hold **Shift** to keep it in a straight line. Click the **key** icon again.
13. Click the **loop** button until it shows **ping-pong** (two arrows). Now it goes back and forth instead of jumping back.
14. Click **Autoplay on Load** (the A icon) so it starts when the game starts.

### Check it

Press **F5**. One platform sits still. The other slides back and forth. You can jump up through both from below, and the player rides along on the moving one.

### RPG changes

Make it a **moving obstacle** instead: something that slides back and forth across a hallway that the player has to time getting past.

- Turn **off** One Way Collision so it blocks the player from every side.
- Or, to make it dangerous, animate a killzone object instead (see [#8](#8-killzone)).

---

## 6. Pickups

### What it is

Coins (or whatever fits your theme) that disappear when the player touches them. An **Area2D** doesn't block anything. It only notices when something enters it.

### Nodes you need

```
Coin (Area2D)
├── AnimatedSprite2D
└── CollisionShape2D
```

### Steps

**Make the coin scene**

1. New scene. Root node: **Area2D**. Rename it **Coin**.
2. Add an **AnimatedSprite2D**. Add your coin frames the same way as the player (see [#1](#1-player-with-animations) steps 5–10). Set FPS to 10 and turn on **Autoplay on Load**.
3. Add a **CollisionShape2D** with a **CircleShape2D**. Make it a little smaller than the coin.

**Only notice the player**

4. Select **Coin**. Under **Collision → Mask**, turn **off** 1 and turn **on** 2.

   The player is on layer 2 (from #1 step 18). The mask is what the coin **looks for**. Now a moving platform passing through won't pick up the coin.

5. Save as `coin.tscn`.

**Add the script**

6. Select Coin. **Attach Script** → template **Object: Empty** → path `res://scripts/coin.gd`.
7. Click the **Node** tab (next to Inspector). Under **Signals**, double-click **body_entered**, then click **Connect**. Godot adds the function for you.
8. Fill it in like this:

### Script — `coin.gd` (first version)

```gdscript
extends Area2D


# Runs when a body enters the coin's area.
# Because of the mask, the only body that can trigger this is the player.
func _on_body_entered(body):
	print("+1 coin")
	queue_free()   # removes the coin from the game
```

You'll update this script in [#7](#7-score-on-screen) and [#12](#12-pickup-sound).

**Put coins in the level**

9. In `game.tscn`, drag in coins. **Ctrl+D** duplicates the selected one.
10. To keep the Scene list tidy, add a plain **Node** named **Coins** and drag all the coins under it.

### Check it

Press **F5**. When the player touches a coin, it disappears and **+1 coin** shows in the Output panel at the bottom.

### RPG changes

None. Pickups work the same.

---

## 7. Score on screen

### What it is

A **GameManager** holds things the whole game needs to know, like the score. When a coin is picked up, it tells the GameManager. The GameManager adds a point and updates a label.

### Nodes you need

```
Game (Node2D)
├── GameManager (Node)     ← marked as a unique name (%)
│   └── ScoreLabel (Label)
└── ...
```

GameManager is a plain **Node**, not a Node2D. It doesn't need a position. It just holds a script.

### Steps

**Make the GameManager**

1. Open `game.tscn`. Add a **Node**. Rename it **GameManager**. Drag it to the top of the list.
2. **Right-click** GameManager → **Access as Unique Name**. A **%** appears next to it. This lets other scripts find it by name, no matter where they are in the scene.

**Make the score label**

3. Add a **Label**. Rename it **ScoreLabel**. Drag it **under** GameManager.
4. Place it somewhere in the level, like near the end. Type placeholder text like `You collected 0 coins.`
5. Set up the font so it isn't blurry. See [#10](#10-signs-with-a-pixel-font).

**GameManager script**

6. Select GameManager. **Attach Script** → **Object: Empty** → path `res://scripts/game_manager.gd`.

### Script — `game_manager.gd`

```gdscript
extends Node

# The player's score. Starts at 0 every time the level loads.
var score = 0

# Gets the label under the GameManager so we can change its text.
@onready var score_label = $ScoreLabel


# Our own function. Coins call this when they're picked up.
func add_point():
	score += 1
	# str() turns the number into text so it can be joined to other text.
	# Note the spaces inside the quotes.
	score_label.text = "You collected " + str(score) + " coins."
```

**Update the coin script**

7. Open `coin.gd`. Drag **GameManager** from the Scene list into the script and hold **Ctrl** when you let go. You get a line that starts with `%GameManager`.

### Script — `coin.gd` (second version)

```gdscript
extends Area2D

# The % finds the node by its unique name.
# This only works because the coins and GameManager are both in game.tscn.
@onready var game_manager = %GameManager


func _on_body_entered(body):
	game_manager.add_point()   # tell the GameManager to add a point
	queue_free()
```

### Check it

Press **F5**. Pick up coins, then walk to the label. The number matches how many you picked up.

If you get *"Node not found: %GameManager"*, you skipped step 2, or the coin is in a different scene than the GameManager.

### RPG changes

None.

---

## 8. Killzone

### What it is

One reusable scene that kills the player. You use it for falling off the map, for the enemy, and for anything else dangerous like spikes. The game slows down, the player drops, and then the level restarts.

The killzone scene has **no shape of its own**. You add the shape each time you use it, so it can be a long line at the bottom of the map, a box around an enemy, or a row of spikes.

### Nodes you need

```
Killzone (Area2D)
└── Timer
```

### Steps

**Make the killzone scene**

1. New scene. Root node: **Area2D**. Rename it **Killzone**.
2. Under **Collision → Mask**, turn **off** 1 and turn **on** 2, so it only reacts to the player.
3. Add a **Timer**. Set **Wait Time** to **0.6** and turn on **One Shot**.
4. Ignore the warning about a missing shape. That's on purpose.
5. Save as `killzone.tscn`.

**Add the script**

6. Select Killzone. **Attach Script** → **Object: Empty** → path `res://scripts/killzone.gd`.
7. Drag the **Timer** into the script and hold **Ctrl** when you let go.
8. Select Killzone. **Node** tab → double-click **body_entered** → **Connect**.
9. Select Timer. **Node** tab → double-click **timeout** → **Connect**.

### Script — `killzone.gd`

```gdscript
extends Area2D

@onready var timer = $Timer


# The player touched the killzone.
func _on_body_entered(body):
	print("You died!")
	Engine.time_scale = 0.5   # slow the whole game to half speed
	# body is the player. Remove its collision shape so it falls off the map.
	body.get_node("CollisionShape2D").queue_free()
	timer.start()             # wait a moment before restarting


# The timer ran out.
func _on_timer_timeout():
	Engine.time_scale = 1.0   # back to normal speed, or the restart stays slow
	get_tree().reload_current_scene()   # restart the level
```

**Use it for falling**

10. Open `game.tscn`. Click the **chain link** button (Instantiate Child Scene) and pick `killzone.tscn`.
11. With that Killzone selected, add a **CollisionShape2D** under it. Set **Shape → New WorldBoundaryShape2D**. This line goes on forever left and right.
12. Select the **Killzone** (not the shape). Press **W** and drag it well below your level.

**Use it for spikes or traps (optional)**

13. Add another Killzone. Give it a **RectangleShape2D** and a **Sprite2D** with your spike art.

### Check it

Press **F5** and jump off the edge. **You died!** prints, the game slows, and the level restarts.

### RPG changes

There's no falling in an RPG, so skip steps 10–12. Use the killzone for hazards (lava, spikes, traps) and for the enemy in [#9](#9-enemy-that-moves-between-walls). The script works the same.

---

## 9. Enemy that moves between walls

### What it is

An enemy that walks until it gets close to a wall, then turns around. Because it uses the killzone, touching it kills the player. You can drop it between any two walls and it works.

### Nodes you need

```
Slime (Node2D)
├── AnimatedSprite2D
├── Killzone (instance of killzone.tscn)
│   └── CollisionShape2D
├── RayCastRight (RayCast2D)
└── RayCastLeft (RayCast2D)
```

| Node | What it does |
|---|---|
| **Node2D** | A plain node with a position. The enemy doesn't need physics because it never bumps into anything. |
| **RayCast2D** | An invisible line that checks if it's touching something, like a wall. |

### Steps

**Make the enemy scene**

1. New scene. Root node: **Node2D**. Rename it after your enemy (the tutorial used **Slime**).
2. Add an **AnimatedSprite2D** with your enemy's idle animation. FPS 10, **Autoplay on Load**.
3. Click the **chain link** button and add `killzone.tscn`.
4. Under the Killzone, add a **CollisionShape2D** with a **RectangleShape2D**. Fit it to the enemy's body. Hold **Alt** while dragging to scale it evenly.

**Add the wall checks**

5. Add a **RayCast2D**. Rename it **RayCastRight**.
6. Set its **Target Position** to **x: 10, y: 0**. The arrow now points right, from the middle of the enemy.
7. Select it and press **Ctrl+D**. Rename the copy **RayCastLeft** and set **Target Position** to **x: -10, y: 0**.

   The raycasts look for layer 1 (your tiles). The player is on layer 2, so the enemy won't turn around when it touches the player.

8. Save as `slime.tscn` (or your enemy's name).

**Add the script**

9. Select the root. **Attach Script** → **Object: Empty** → `res://scripts/slime.gd`.
10. Drag both raycasts and the AnimatedSprite2D into the script, holding **Ctrl** when you let go.

### Script — `slime.gd`

```gdscript
extends Node2D

# Speed in pixels per second
const SPEED = 60

# 1 = moving right, -1 = moving left
var direction = 1

@onready var ray_cast_right = $RayCastRight
@onready var ray_cast_left = $RayCastLeft
@onready var animated_sprite = $AnimatedSprite2D


# Runs every frame. delta is the time since the last frame.
func _process(delta):
	# Wall on the right? Turn around and face left.
	if ray_cast_right.is_colliding():
		direction = -1
		animated_sprite.flip_h = true

	# Wall on the left? Turn around and face right.
	if ray_cast_left.is_colliding():
		direction = 1
		animated_sprite.flip_h = false

	# Move. Multiplying by delta keeps the speed the same on fast and slow computers.
	position.x += direction * SPEED * delta
```

**Put it in the level**

11. In `game.tscn`, drag the enemy in. Place it on the ground between two walls.

### Check it

Press **F5**. The enemy walks to a wall, flips, and walks back. Touching it kills the player and restarts the level.

If the enemy walks through a wall, the raycasts are too short, or that wall tile has no physics (see [#4](#4-tile-level-with-a-background-layer)).

### RPG changes

The same script works for an enemy that patrols left and right. For one that patrols **up and down**:
- Name the raycasts **RayCastUp** (target **x: 0, y: -10**) and **RayCastDown** (target **x: 0, y: 10**).
- In the script, use the up and down raycasts and change `position.x` to `position.y`.

---

## 10. Signs with a pixel font

### What it is

Text in your level that gives hints or tells the story. In Godot, text is a **Label**. Normal fonts look blurry in a zoomed-in pixel game, so you use a pixel font.

### Nodes you need

```
Game (Node2D)
├── Labels (Node)
│   ├── Label
│   ├── Label
│   └── ...
```

### Steps

1. Open `game.tscn`. Add a **Label**. Place it where the player will read it and drag its box a little bigger.
2. Type your text in the **Text** box.
3. Under **Theme Overrides → Fonts**, drag in a pixel font from `assets/fonts` (the tutorial used **PixelOperator8**).
4. Under **Theme Overrides → Font Sizes**, turn it on and set it to **8**.

   **Use multiples of 8** (8, 16, 24, 32). Other sizes come out blurry.

5. Under **Theme Overrides → Colors**, change **Font Color** to fit your theme.
6. For longer text, set **Autowrap Mode** to **Word** and **Horizontal Alignment** to **Center**.
7. Make more signs. **Ctrl+D** copies one with all its settings.
8. Add a plain **Node** named **Labels** and drag all your signs under it.

### Check it

Press **F5**. The text is sharp, not blurry, and the player walks in front of it.

### RPG changes

None.

---

## 11. Music that keeps playing

### What it is

A looping music track. If the music is in `game.tscn`, it starts over every time the player dies. Making it an **Autoload** keeps it running the whole game.

### Nodes you need

```
Music (AudioStreamPlayer)    ← its own scene, loaded as an Autoload
```

### Steps

**Make the music scene**

1. New scene. Root node: **AudioStreamPlayer**. Rename it **Music**.
2. Drag your music file from `assets/music` into **Stream**.
3. Turn on **Autoplay**.
4. Save as `music.tscn`.

**Make it loop**

5. In the **FileSystem** panel, double-click your music file.
6. Turn on **Loop** and click **Reimport**.

**Turn the volume down**

7. Click the **Audio** tab at the bottom of the screen.
8. Click **Add Bus** twice. Name them **Music** and **SFX**.
9. Drag the **Music** bus slider down to about **-12 dB**.
10. In `music.tscn`, select Music and set **Bus** to **Music**.

**Make it an Autoload**

11. Go to **Project → Project Settings → Globals → Autoload**. (In some versions the tab is just called **Autoload**.)
12. Click the **folder** icon, pick `music.tscn`, and click **Add**.
13. If you put music in `game.tscn` before, delete it from there. Otherwise you'll hear it twice.

### Check it

Press **F5**. The music plays. Die on purpose. The level restarts, but the music keeps going without starting over.

### RPG changes

None.

---

## 12. Pickup sound

### What it is

A sound when the player grabs a coin. There's a problem: the coin script calls `queue_free()`, which deletes the coin **before** the sound can play. The fix is an **AnimationPlayer** that hides the coin, plays the sound, and then deletes the coin one second later.

### Nodes you need

```
Coin (Area2D)
├── AnimatedSprite2D
├── CollisionShape2D
├── PickupSound (AudioStreamPlayer2D)   ← add this
└── AnimationPlayer                     ← add this
```

### Steps

**Add the sound**

1. Open `coin.tscn`. Add an **AudioStreamPlayer2D**. Rename it **PickupSound**.
2. Drag your coin sound from `assets/sounds` into **Stream**.
3. Set **Bus** to **SFX**.

**Build the pickup animation**

4. Add an **AnimationPlayer**. In the Animation panel, click **Animation → New** and name it **pickup**.
5. Set the animation **length** to **1** second.

   For each step below, make sure the timeline is at **0** before clicking the key icon.

6. **Hide the coin.** Select **AnimatedSprite2D**. In the Inspector, under **Visibility**, click the **key** next to **Visible**. When it asks to create a **RESET** track, click **Yes**. Then turn **Visible** off and click the key again.
7. **Turn off the collision**, so the player can't grab it twice. Select **CollisionShape2D**. Click the key next to **Disabled**. Then turn **Disabled** on and click the key again.
8. **Play the sound.** Select **PickupSound**. Click the key next to **Playing**. Then turn **Playing** on and click the key again.
9. **Delete the coin after 1 second.** In the Animation panel, click **+ Add Track → Call Method Track** and pick **Coin**. Right-click on that track at **1** second → **Insert Key** → pick **queue_free**.

   The RESET track keeps the coin's normal settings (visible, collision on, not playing). Click **RESET** in the animation list anytime to get back to them.

**Update the coin script**

10. Open `coin.gd`. Drag the **AnimationPlayer** into the script and hold **Ctrl** when you let go.

### Script — `coin.gd` (final version)

```gdscript
extends Area2D

@onready var game_manager = %GameManager
@onready var animation_player = $AnimationPlayer


func _on_body_entered(body):
	game_manager.add_point()
	# Instead of queue_free(), play the animation.
	# It hides the coin, plays the sound, and deletes the coin after 1 second.
	animation_player.play("pickup")
```

### Check it

Press **F5**. Grab a coin. You hear the sound, the coin disappears right away, and the score still goes up by **one** (not two).

### RPG changes

None.

---

## Your game when everything is done

Your `game.tscn` should look something like this:

```
Game (Node2D)
├── GameManager (Node) %
│   └── ScoreLabel (Label)
├── Background (TileMapLayer)
├── Mid (TileMapLayer)
├── Player (instance)
│   └── Camera2D
├── Killzone (instance)              ← falling (platformer only)
│   └── CollisionShape2D
├── Platforms (Node)
│   ├── Platform (instance)
│   └── Platform (instance)
│       └── AnimationPlayer
├── Coins (Node)
│   └── Coin (instance) ...
├── Enemies (Node)
│   └── Slime (instance) ...
└── Labels (Node)
    └── Label ...
```

Plus **Music** running as an Autoload.

Every part above came from the tutorial. Your **features** are the parts you research and build yourselves. Check them off in `godot02_gamePlan.md`.
