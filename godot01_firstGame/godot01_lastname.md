**Before you start:** rename this file to `godot01_lastname.md`, using your own last name. Put it in your `gameDev` repo folder. Read `godot01_Walkthrough.md` first. Push at every checkpoint.

**Name:**

**Team:**

---

# Godot 01 — First Game Tutorial

## Checkpoints

Put an `x` in each box when it works and you've pushed it. Example: `- [x]`

- [ ] **Checkpoint 1** — Knight moves and jumps
- [ ] **Checkpoint 2** — Level made of tiles, camera follows
- [ ] **Checkpoint 3** — One still platform, one moving platform
- [ ] **Checkpoint 4** — Coins you can pick up
- [ ] **Checkpoint 5** — Falling restarts the game
- [ ] **Checkpoint 6** — Slime patrols and kills you
- [ ] **Checkpoint 7** — Knight runs, jumps, and faces the right way
- [ ] **Checkpoint 8** — Signs and a coin score
- [ ] **Checkpoint 9** — Music and a coin sound
- [ ] **Checkpoint 10** — Export (only if your teacher says so)

---

## Day 1 Questions

Answer in your own words. One or two sentences is enough.

**1.** What is the difference between a **node** and a **scene**? Give an example of each from your game.

**Answer:**


**2.** Why did the player need a **CollisionShape2D**? What warning did Godot give you before you added one?

**Answer:**


**3.** Why did he make the collision shape a little **smaller** than the knight's picture?

**Answer:**


**4.** The ground uses a **StaticBody2D** and the moving platform uses an **AnimatableBody2D**. Why not use the same kind of node for both?

**Answer:**


**5.** Write down the `SPEED` and `JUMP_VELOCITY` numbers you ended up with. Why is the jump number **negative**?

**Answer:**


---

## Day 2 Questions

**6.** What is a **signal**? Which signal did you use for the coin, and when does it fire?

**Answer:**


**7.** You put the player on collision **layer 2** and set the coin's **mask** to 2. What problem did that fix?

**Answer:**


**8.** The killzone scene has **no collision shape** inside it. Why did he build it that way? Name two places in your game that use the killzone.

**Answer:**


**9.** Paste the line that moves the slime. Explain why it's multiplied by `delta`.

```gdscript

```

**Answer:**


**10.** How does the slime know when to turn around? What does `direction` do?

**Answer:**


---

## Day 3 Questions

**11.** What is the difference between `_process(delta)` and `_physics_process(delta)`? Which one moves the player, and why?

**Answer:**


**12.** Explain what this code does, line by line:

```gdscript
if is_on_floor():
	if direction == 0:
		animated_sprite.play("idle")
	else:
		animated_sprite.play("run")
else:
	animated_sprite.play("jump")
```

**Answer:**


**13.** Why did the score line need `str(score)` instead of just `score`?

**Answer:**


**14.** Why did the coin use an **AnimationPlayer** to play the sound and then remove itself, instead of calling `queue_free()` right away?

**Answer:**


**15.** What was the hardest bug you hit, and how did you fix it?

**Answer:**


---

## If you already did the Unity tutorial

Skip this section if Godot is your first engine.

**16.** What is Godot's version of each Unity thing?

| Unity | Godot |
|---|---|
| GameObject | |
| Prefab | |
| `Start()` | |
| `Update()` | |
| `Destroy(gameObject)` | |
| `Time.deltaTime` | |

**17.** Which engine did you like better so far, and why?

**Answer:**


---

## If you finished early

What did you add from the "Where to go from here" list? How did you do it?

**Answer:**

