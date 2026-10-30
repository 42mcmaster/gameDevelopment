**Before you start:** rename this file to `unity01_lastname.md`, using your own last name. Put it in your `gameDev` repo folder. Read `unity01_Walkthrough.md` first. Push at every checkpoint.

**Name:**

**Team:**

---

# Unity 01 — Flappy Bird Tutorial

## Checkpoints

Put an `x` in each box when it works and you've pushed it. Example: `- [x]`

- [ ] **Checkpoint 1** — Bird is on screen
- [ ] **Checkpoint 2** — Bird falls and flaps with the spacebar
- [ ] **Checkpoint 3** — Pipes spawn at random heights, move left, and delete themselves
- [ ] **Checkpoint 4** — Score goes up when the bird passes through pipes
- [ ] **Checkpoint 5** — Game over when the bird hits a pipe, and restart works

---

## Day 1 Questions

Answer in your own words. One or two sentences is enough.

**1.** What is the difference between a **GameObject** and a **component**?

**Answer:**


**2.** Code in `Start()` runs ________. Code in `Update()` runs ________. Give one example from the bird script of something that belongs in each.

**Answer:**


**3.** Why did the script need a line like `public Rigidbody2D myRigidbody;` before it could make the bird move? What would happen without it?

**Answer:**


**4.** In this line, what does the `if` do? What happens on a frame when the spacebar is **not** pressed?

```csharp
if (Input.GetKeyDown(KeyCode.Space) == true)
{
    myRigidbody.linearVelocity = Vector2.up * flapStrength;
}
```

**Answer:**


**5.** Write down the **flapStrength** and **Gravity Scale** numbers you ended up with. Why did you pick them?

**Answer:**


---

## Day 2 Questions

**6.** What is a **prefab**, and why did the pipes need to be one?

**Answer:**


**7.** The pipe movement code multiplies by `Time.deltaTime`. What problem does that fix?

**Answer:**


**8.** The pipes delete themselves when they go off screen. Why bother? What would happen if you didn't?

**Answer:**


**9.** The video moved the spawn code into its own function, `spawnPipe()`, instead of copying it into both `Start()` and `Update()`. Why is that better?

**Answer:**


**10.** What is the difference between a **collider** and a **trigger**? Which one did you use for scoring, and which one ends the game?

**Answer:**


**11.** Paste the **one line of code** that makes the score go up, and explain what each part does.

```csharp

```

**Answer:**


**12.** What does the `birdIsAlive` variable do? What would happen in the game without it?

**Answer:**


**13.** What was the hardest bug you hit, and how did you fix it?

**Answer:**


---

## If you finished early

What did you add from the Next Steps list? How did you do it?

**Answer:**

