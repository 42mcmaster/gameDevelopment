# Godot 02 Supplement — Enemy That Attacks and Takes Damage

This is an add-on to `godot02_HowTo.md`. In the tutorial, the enemy is a killzone: touch it once and you die. This guide changes that so:

- the **player** has health and an **attack** button
- the **enemy** has health, **attacks** when the player gets close, and **takes damage** from the player's attack
- both of them get hurt instead of dying right away

**Before you start:** finish How-To parts [#1 Player](godot02_HowTo.md#1-player-with-animations), [#2 Controls](godot02_HowTo.md#2-your-own-controls), [#7 Score](godot02_HowTo.md#7-score-on-screen), and [#9 Enemy](godot02_HowTo.md#9-enemy-that-moves-between-walls). This guide changes the player and the enemy you already built.

---

## Table of Contents

1. [How it works](#1-how-it-works)
2. [Set up the collision layers](#2-set-up-the-collision-layers)
3. [Add an attack button](#3-add-an-attack-button)
4. [Set up the player](#4-set-up-the-player)
5. [Player script](#5-player-script)
6. [Set up the enemy](#6-set-up-the-enemy)
7. [Enemy script](#7-enemy-script)
8. [Show the player's health](#8-show-the-players-health)
9. [Check it](#9-check-it)
10. [If something's wrong](#10-if-somethings-wrong)
11. [RPG changes](#11-rpg-changes)

---

## 1. How it works

Two things have to happen:

| What happens | How |
|---|---|
| **The enemy hurts the player** | The enemy has an Area2D called **Hitbox**. Every frame, the enemy checks if the player is inside it. If so, it plays its attack animation and calls the player's `take_damage()` function. |
| **The player hurts the enemy** | The player has an Area2D called **AttackArea** in front of it. When the player presses attack, it checks which enemies are inside AttackArea and calls each one's `take_damage()` function. |

Both the player and the enemy have their **own** `take_damage()` function. Each one decides what getting hurt looks like for itself.

**Invincibility:** after the player gets hit, it can't be hit again for 1 second. Without this, the enemy would hit the player 60 times a second.

**Knockback:** when the player gets hit, it gets pushed away from the enemy so it isn't stuck inside it.

---

## 2. Set up the collision layers

Layers tell Godot what can detect what. Name them first so they're easier to read.

1. Go to **Project → Project Settings → Layer Names → 2D Physics**.
2. Name the layers:

| Layer | Name |
|---|---|
| 1 | World |
| 2 | Player |
| 3 | Enemy |

Here is how each part should be set. You'll set these in the steps below. Come back to this table if something isn't detecting.

| Node | Layer (what it **is**) | Mask (what it **looks for**) |
|---|---|---|
| Player (CharacterBody2D) | 2 Player | 1 World |
| Player's **AttackArea** | none | 3 Enemy |
| Enemy's **Hitbox** | 3 Enemy | 2 Player |
| Enemy's raycasts | — | 1 World |
| Coins and Killzone | (leave as is) | 2 Player |

---

## 3. Add an attack button

1. Go to **Project → Project Settings → Input Map**.
2. Add an action named **attack**.
3. Give it a key. **J**, **X**, or **Left Mouse Button** all work well.

---

## 4. Set up the player

### Nodes you need

```
Player (CharacterBody2D)
├── AnimatedSprite2D
├── CollisionShape2D
├── Camera2D
└── AttackArea (Area2D)       ← add this
    └── CollisionShape2D
```

### Steps

**Add an attack animation**

1. Open `player.tscn`. Select **AnimatedSprite2D**.
2. In the SpriteFrames panel, click **Add Animation**. Name it **attack**.
3. Add your attack frames. If you don't have attack art yet, use any few frames for now and swap them later.
4. Set FPS to about **12**.
5. Turn **off** looping for this animation (the loop button next to the FPS box). **This matters.** The script waits for the attack animation to finish. If it loops, it never finishes, and the player gets stuck in the attack.

**Add the AttackArea**

6. Select **Player**. Press **Ctrl+A** and add an **Area2D**. Rename it **AttackArea**.
7. Select **AttackArea**. Under **Collision**:
   - **Layer:** turn everything **off**.
   - **Mask:** turn **off** 1 and turn **on** 3.
8. Add a **CollisionShape2D** under AttackArea. Set **Shape → New RectangleShape2D**. Size it to about how far your weapon reaches, like **16 × 16**.
9. Leave the **shape** at position **0, 0**. The script moves the whole AttackArea to the left or right side depending on which way the player faces.

**Check the player's layers**

10. Select **Player**. Under **Collision**, **Layer** should be only **2**, and **Mask** should be only **1**.

---

## 5. Player script

Replace everything in `player.gd` with this. It's the How-To #1 script with health, attacking, getting hurt, and dying added.

1. Drag **AttackArea** into the script and hold **Ctrl** when you let go to get its `@onready` line. (It's already in the script below.)

### Script — `player.gd`

```gdscript
extends CharacterBody2D

# A signal is a message this node sends out.
# The GameManager listens for it to update the health on screen (part 8).
signal health_changed(new_health)

const SPEED = 130.0
const JUMP_VELOCITY = -300.0
const KNOCKBACK_SPEED = 200.0   # how hard the player gets pushed when hit
const ATTACK_REACH = 12         # how far in front of the player the AttackArea sits

var health = 3
var is_invincible = false   # true for a moment after getting hit
var is_attacking = false    # true while the attack animation plays
var is_dead = false
var knockback_timer = 0.0   # while this is above 0, the player can't control movement

@onready var animated_sprite = $AnimatedSprite2D
@onready var attack_area = $AttackArea


func _physics_process(delta: float) -> void:
	# --- GRAVITY ---
	if not is_on_floor():
		velocity += get_gravity() * delta

	# --- DEAD: just fall, no controls ---
	if is_dead:
		move_and_slide()
		return

	# --- KNOCKBACK: being pushed, no controls ---
	if knockback_timer > 0:
		knockback_timer -= delta
		move_and_slide()
		return

	# --- JUMP ---
	if Input.is_action_just_pressed("jump") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# --- ATTACK ---
	if Input.is_action_just_pressed("attack") and not is_attacking:
		attack()

	# --- INPUT DIRECTION ---
	var direction := Input.get_axis("move_left", "move_right")

	# --- FACE THE RIGHT WAY ---
	# Flip the sprite AND move the AttackArea to the side we're facing.
	if direction > 0:
		animated_sprite.flip_h = false
		attack_area.position.x = ATTACK_REACH
	elif direction < 0:
		animated_sprite.flip_h = true
		attack_area.position.x = -ATTACK_REACH

	# --- PICK THE ANIMATION ---
	# Don't change animations while attacking, or the attack gets cut off.
	if not is_attacking:
		if is_on_floor():
			if direction == 0:
				animated_sprite.play("idle")
			else:
				animated_sprite.play("run")
		else:
			animated_sprite.play("jump")

	# --- MOVE ---
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)

	move_and_slide()


# Called when the attack button is pressed.
func attack():
	is_attacking = true
	animated_sprite.play("attack")

	# Look at every Area2D inside our AttackArea right now.
	for area in attack_area.get_overlapping_areas():
		# area is the enemy's Hitbox. Its owner is the enemy scene's root node.
		var enemy = area.owner
		# Only hit things that have a take_damage function.
		if enemy and enemy.has_method("take_damage"):
			enemy.take_damage(1)

	# Wait until the attack animation is done before we can attack again.
	await animated_sprite.animation_finished
	is_attacking = false


# Called by the enemy when it hits us.
# from_position is where the enemy is, so we know which way to get pushed.
func take_damage(amount, from_position):
	# Can't get hurt while invincible or already dead.
	if is_invincible or is_dead:
		return

	health -= amount
	health_changed.emit(health)   # tell the GameManager
	print("Player health: ", health)

	if health <= 0:
		die()
		return

	# --- KNOCKBACK ---
	# sign() gives 1 if the enemy is to our left (push right), -1 if to our right (push left).
	var push = sign(global_position.x - from_position.x)
	if push == 0:
		push = 1
	velocity.x = push * KNOCKBACK_SPEED
	velocity.y = -150             # a little hop up
	knockback_timer = 0.2         # no controls for 0.2 seconds

	# --- INVINCIBLE AND FLASH RED ---
	is_invincible = true
	animated_sprite.modulate = Color(1, 0.4, 0.4)   # tint red
	await get_tree().create_timer(1.0).timeout      # wait 1 second
	animated_sprite.modulate = Color(1, 1, 1)       # back to normal
	is_invincible = false


# Same idea as the killzone: slow down, fall off, restart.
func die():
	is_dead = true
	print("You died!")
	Engine.time_scale = 0.5
	$CollisionShape2D.queue_free()   # fall through the floor
	await get_tree().create_timer(0.6).timeout
	Engine.time_scale = 1.0
	get_tree().reload_current_scene()
```

> **Your killzone still works.** Falling into a pit still kills the player right away. Only the enemy uses health.

---

## 6. Set up the enemy

The tutorial enemy used a **Killzone** inside it. That kills instantly, so you'll replace it with a **Hitbox** that does 1 damage at a time.

### Nodes you need

```
Slime (Node2D)
├── AnimatedSprite2D
├── Hitbox (Area2D)          ← replaces the Killzone
│   └── CollisionShape2D
├── RayCastRight (RayCast2D)
└── RayCastLeft (RayCast2D)
```

### Steps

**Add the animations**

1. Open your enemy scene (like `slime.tscn`). Select **AnimatedSprite2D**.
2. Make sure you have these three animations, spelled exactly like this:

| Animation | When it plays | Loop? |
|---|---|---|
| **idle** | Walking back and forth | On |
| **attack** | The player is close | On |
| **hurt** | The player hit it | Off |

   The Brackeys slime sheet already has a hurt row you can use. For your own enemy, make these in Piskel.

**Swap the Killzone for a Hitbox**

3. Right-click the **Killzone** under the enemy → **Delete**.
4. Select the enemy root. Add an **Area2D**. Rename it **Hitbox**.
5. Select **Hitbox**. Under **Collision**:
   - **Layer:** turn **off** 1 and turn **on** 3.
   - **Mask:** turn **off** 1 and turn **on** 2.
6. Add a **CollisionShape2D** under Hitbox with a **RectangleShape2D**.
7. Make the shape a **little bigger** than the enemy's body. The enemy attacks when the player enters this box, so a bigger box means it attacks from a bit farther away.

**Check the raycasts**

8. Select each raycast. Under **Collision Mask**, only **1** should be on. This way they only see walls, not the player.

---

## 7. Enemy script

Replace everything in your enemy script (like `slime.gd`).

1. Drag **Hitbox** into the script and hold **Ctrl** when you let go. (It's already in the script below.)

### Script — `slime.gd`

```gdscript
extends Node2D

const SPEED = 60

var direction = 1      # 1 = right, -1 = left
var health = 3         # how many hits it takes to defeat
var is_hurt = false    # true for a moment after the player hits it
var is_dead = false

@onready var ray_cast_right = $RayCastRight
@onready var ray_cast_left = $RayCastLeft
@onready var animated_sprite = $AnimatedSprite2D
@onready var hitbox = $Hitbox


func _process(delta):
	# Do nothing while dead or hurt. This gives the player a moment to get away.
	if is_dead or is_hurt:
		return

	# --- ATTACK ---
	# Check every body inside the Hitbox. Because of the mask, that can only be the player.
	var touching_player = false
	for body in hitbox.get_overlapping_bodies():
		if body.has_method("take_damage"):
			touching_player = true
			# Send our position so the player knows which way to get pushed.
			body.take_damage(1, global_position)
			# The player's invincibility stops this from hitting 60 times a second.

	if touching_player:
		animated_sprite.play("attack")
		return   # stand still while attacking

	# --- WALK (same as the tutorial) ---
	animated_sprite.play("idle")

	if ray_cast_right.is_colliding():
		direction = -1
		animated_sprite.flip_h = true

	if ray_cast_left.is_colliding():
		direction = 1
		animated_sprite.flip_h = false

	position.x += direction * SPEED * delta


# Called by the player's attack.
func take_damage(amount):
	if is_dead:
		return

	health -= amount
	print("Enemy health: ", health)

	if health <= 0:
		die()
		return

	# Get hurt: play the hurt animation and stop for a moment.
	is_hurt = true
	animated_sprite.play("hurt")
	await get_tree().create_timer(0.4).timeout

	# If it died while we were waiting, stop here.
	if is_dead:
		return
	is_hurt = false


func die():
	is_dead = true
	print("Enemy defeated!")

	# Turn off the Hitbox so it can't hurt the player or get hit again.
	# set_deferred waits until it's safe to change physics settings.
	hitbox.set_deferred("monitoring", false)
	hitbox.set_deferred("monitorable", false)

	animated_sprite.play("hurt")

	# A tween changes a value smoothly over time.
	# This fades the enemy out over 0.5 seconds. ("modulate:a" means transparency.)
	var tween = create_tween()
	tween.tween_property(animated_sprite, "modulate:a", 0.0, 0.5)
	await tween.finished

	queue_free()   # remove the enemy from the game
```

**Want enemies that take more hits?** Change `var health = 3`. For different enemy types, select the enemy in `game.tscn` and change it there. First add `@export` in front of the line (`@export var health = 3`) so it shows up in the Inspector.

---

## 8. Show the player's health

The player is in its own scene, so it can't use `%GameManager` the way coins do. Instead, the player sends a **signal**, and you connect it to the GameManager in `game.tscn`.

### Nodes you need

```
Game (Node2D)
├── GameManager (Node) %
│   ├── ScoreLabel (Label)
│   └── HUD (CanvasLayer)        ← add this
│       └── HealthLabel (Label)  ← add this
```

A **CanvasLayer** draws on top of the screen and ignores the camera. Use it for anything that should stay in the corner, like health.

### Steps

1. Open `game.tscn`. Select **GameManager** and add a **CanvasLayer**. Rename it **HUD**.
2. Under **HUD**, add a **Label**. Rename it **HealthLabel**.
3. Put it in the top-left corner. Type `Health: 3` in **Text**.
4. Give it your pixel font (How-To [#10](godot02_HowTo.md#10-signs-with-a-pixel-font)). The HUD isn't zoomed in like the level, so use a bigger size like **16** or **32**.

**Connect the signal**

5. In `game.tscn`, select the **Player**.
6. Click the **Node** tab. Under **Signals**, you'll see **health_changed**. It's there because of the `signal` line in `player.gd`.
7. Double-click it. Pick **GameManager**, then click **Connect**.

**Update the GameManager script**

8. Open `game_manager.gd`. Godot added a new function at the bottom. Fill it in like this:

### Script — `game_manager.gd`

```gdscript
extends Node

var score = 0

@onready var score_label = $ScoreLabel
@onready var health_label = $HUD/HealthLabel


func add_point():
	score += 1
	score_label.text = "You collected " + str(score) + " coins."


# Runs every time the player sends the health_changed signal.
func _on_player_health_changed(new_health):
	health_label.text = "Health: " + str(new_health)
```

---

## 9. Check it

Press **F5** and test each of these:

- [ ] Walk into the enemy. It stops and plays **attack**.
- [ ] The player flashes red, gets pushed back, and the health label goes down by **1** (not 2 or 3).
- [ ] For 1 second after getting hit, the enemy can't hurt the player again.
- [ ] Get hit 3 times. The player dies and the level restarts.
- [ ] Face the enemy and press attack. The player plays **attack** and the enemy plays **hurt**.
- [ ] Hit the enemy 3 times. It fades out and disappears.
- [ ] Attacking while facing **away** from the enemy does nothing.
- [ ] Falling in a pit still kills the player right away.

---

## 10. If something's wrong

| Problem | Likely fix |
|---|---|
| Player gets stuck in the attack animation | **attack** is set to loop. Turn looping off (part 4, step 5). |
| Pressing attack doesn't hurt the enemy | AttackArea's **Mask** isn't **3**, or the Hitbox's **Layer** isn't **3**. Check the table in [part 2](#2-set-up-the-collision-layers). |
| Enemy never attacks | Hitbox's **Mask** isn't **2**, or the Player's **Layer** isn't **2**. |
| Health drops by a lot at once | `is_invincible` isn't being set. Make sure your `take_damage` matches the script. |
| Error: *Invalid call. Nonexistent function 'take_damage'* | The function name is spelled differently, or the script isn't attached to the root node. |
| Error: *Animation 'attack' doesn't exist* | An animation name doesn't match. Names are case-sensitive: `attack`, not `Attack`. |
| Enemy turns around when it touches the player | A raycast's mask has layer 2 on. Turn it off. |
| `health_changed` isn't in the Signals list | Save `player.gd`, then click the Player in `game.tscn` again. |

---

## 11. RPG changes

In an RPG there's no gravity and the player can face four ways, so a few things change.

**Player variables**: add this with the others:

```gdscript
var facing = Vector2.DOWN   # which way the player last moved
```

**Movement and facing**: use the RPG movement from How-To #1, and add this right after you get `direction`:

```gdscript
	# Remember the last direction we moved, and put the AttackArea on that side.
	if direction != Vector2.ZERO:
		facing = direction.normalized()
		attack_area.position = facing * ATTACK_REACH
```

**Remove** the gravity lines and the `jump` lines.

**Knockback**: in `take_damage`, replace the knockback section with this. It pushes the player straight away from the enemy in any direction:

```gdscript
	# Push away from the enemy in whatever direction that is.
	velocity = (global_position - from_position).normalized() * KNOCKBACK_SPEED
	knockback_timer = 0.2
```

**Dying**: the player can't fall off the map, so in `die()`, remove the `$CollisionShape2D.queue_free()` line. Fading out works well instead:

```gdscript
	var tween = create_tween()
	tween.tween_property(animated_sprite, "modulate:a", 0.0, 0.5)
```

**The enemy script doesn't need any changes.** For an enemy that walks up and down, see How-To [#9 RPG changes](godot02_HowTo.md#9-enemy-that-moves-between-walls).
