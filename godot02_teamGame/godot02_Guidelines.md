# Godot 02 Guidelines — Team Game

**Read this first. Then one person on your team copies `godot02_gamePlan.md` into your team repo and you fill it in together.**

---

## Table of Contents

- [What you're doing](#what-youre-doing)
- [The rules](#the-rules)
- [Your team and roles](#your-team-and-roles)
- [Planning day](#planning-day)
- [Suggested plan for each day](#suggested-plan-for-each-day)
- [Set up your team repo](#set-up-your-team-repo)
- [Working on the same project](#working-on-the-same-project)
- [Required changes](#required-changes)
- [Pick at least 3 features](#pick-at-least-3-features)
- [If you pick an RPG](#if-you-pick-an-rpg)
- [Making your art in Piskel](#making-your-art-in-piskel)
- [Sound](#sound)
- [What you turn in](#what-you-turn-in)

---

## What you're doing

You finished the Godot tutorial on your own. Now you and a partner build your own 2D game, using what you learned in the tutorial.

When you're done, someone who played the tutorial game should look at yours and say "that's a different game."

---

## The rules

- **2D game in Godot.** It can be a **platformer** (like the tutorial) or a top-down **RPG**.
- **At least 3 sprites made by your team in Piskel.** You can make more, but watch your time. For everything else, you can use free sprites you find online.
- **Sound is not required to be made by you.** Reuse the Brackeys sounds or find free ones online.
- **5 lab days** to build it. They will be spread out, not all in one week.
- **Push at the end of every lab day.**

---

## Your team and roles

Teams are **2 people**. Both of you write code. On top of that, each of you leads one area:

| Role | What you're in charge of |
|---|---|
| **Art lead** | Makes sure the Piskel sprites get made and finds any other art you use |
| **Sound lead** | Collects the music and sound effects (Brackeys sounds or ones you find) and adds them to the game |

"Lead" means you make sure it gets done. It doesn't mean your partner can't help.

In the Unity round, you **switch roles**.

---

## Planning day

You get about **1 hour and 15 minutes** to plan before you start building. Fill out the **Game Plan** section of `godot02_gamePlan.md`.

Be detailed. Your plan should answer:

- Platformer or RPG?
- Who is the player, and where are they?
- What does the player do, and how do they win and lose?
- What the player, enemy, pickups, and tiles will look like, and which ones you will make in Piskel
- Which 3 or more features you will build, and who codes each one

Keep it doable. You have 5 lab days. A small game that works is better than a big game that doesn't.

---

## Suggested plan for each day

This is a suggestion. Your team can adjust it.

| Day | What to get done |
|---|---|
| **Day 1** | Set up the team repo. Art lead: start the Piskel sprites. Sound lead: collect sounds and music. Both: start the level or map. |
| **Day 2** | Get your sprites into the game. Finish level 1 (or the first map). |
| **Day 3** | Build your first feature from the feature list. |
| **Day 4** | Build your second and third features. |
| **Day 5** | Have another team play your game. Fix what they found. Finish your `README.md`. |

At the end of every day:
1. Add an entry to the **Daily Log** in `godot02_gamePlan.md`.
2. Commit and **push**.

If you fall behind, talk to your teacher. Cut a feature before you cut the required changes.

---

## Set up your team repo

Only **one** partner does steps 1–5. The other partner does step 6.

1. In GitHub Desktop, create a new repository named **`teamGame_Lastname1_Lastname2`** (both last names, in alphabetical order).
2. Copy the `.gitignore` from your `gameDev` repo into this new repo. It already has the `.godot/` line.
3. Pick whichever partner's tutorial game works best. Copy that whole `FirstGame_Lastname` folder into the team repo and rename the folder to your new game's name, with no spaces (for example `SpaceSlime`).
4. Open the project in Godot. Go to **Project → Project Settings → Application → Config → Name** and change it to your game's name.
5. Copy `godot02_gamePlan.md` into the team repo. **Commit and push.**
6. **Add your partner:** on github.com, open the repo, go to **Settings → Collaborators → Add people**, and add your partner's GitHub username. Your partner accepts the invite, then clones the repo in GitHub Desktop.

**Check:** both of you can open the game in Godot and press Play.

---

## Working on the same project

If you both change the same scene file at the same time, GitHub can't combine them. To avoid that:

1. **Pull when you start.** In GitHub Desktop: **Fetch origin**, then **Pull**.
2. **Push when you stop.**
3. **Don't work in the same scene at the same time.** Say who is working on what, for example "I'm in `player.tscn`, you take `slime.tscn`."
4. Making art in Piskel or collecting sounds doesn't touch Godot files, so that's safe any time.

**If GitHub Desktop says there's a conflict:** stop and get your teacher.

---

## Required changes

Every team must do all of these:

- [ ] **New name and theme.** Pick your own idea: a robot in a factory, a frog in a swamp, an astronaut on a moon.
- [ ] **At least 3 sprites made in Piskel** by your team.
- [ ] **Sound in the game.** Brackeys sounds or ones you found.
- [ ] **Your own level or map.** Don't just repaint the tutorial level with new tiles.
- [ ] **New text.** Change the signs and score message to fit your game.

---

## Pick at least 3 features

Pick **at least 3** features. **At least 1 must be from the Harder list.** Write your picks in your game plan on planning day.

### Easier

| Feature | Hint |
|---|---|
| **Spikes or a trap** | Another use of your killzone scene. |
| **A second enemy type** | A different speed, or one that moves a longer path. |
| **A pickup worth more points** | Copy the coin scene and change how many points it adds. |
| **A win message at the end** | A label or area at the end that says you won. |
| **New player animations** | A landing, hurt, or different idle animation. |

### Harder

| Feature | Hint |
|---|---|
| **Double jump** (platformer) | Keep a count of jumps. Reset it when `is_on_floor()` is true. |
| **Lives or health** | The player survives a few hits. Store lives in the GameManager. |
| **A countdown timer** | Show time left on a label. At zero, the player loses. |
| **A second level or area** | Make a new scene and switch to it. Look up `change_scene_to_file`. |
| **A title screen** | A start scene with your game's name and a **Start** button. |
| **A power-up** | Makes the player faster or jump higher for a few seconds. Use a Timer. |
| **A falling or disappearing platform** (platformer) | Use an AnimationPlayer or Timer to make it drop or vanish. |
| **A checkpoint** | Touch it, and you respawn there instead of at the start. |
| **A character you talk to** (RPG) | Walk near them and a label shows what they say. Use an Area2D. |
| **A locked door and key** (RPG) | Pick up the key, then the door opens. Store "has key" in the GameManager. |

**Your own idea:** write it in your game plan and get your teacher's OK on planning day.

---

## If you pick an RPG

The tutorial was a platformer, so an RPG takes a few changes:

- The camera looks down from above. There's no gravity and no jumping.
- The player moves **up, down, left, and right**. Add `move_up` and `move_down` in **Project → Project Settings → Input Map**.
- In the player script, remove the gravity and jump code. Set both `velocity.x` and `velocity.y` from the input.
- Tiles, coins, the killzone, the GameManager, and animations all work the same way as the tutorial.

Ask your teacher if you get stuck on movement. That's the first thing to get working.

---

## Making your art in Piskel

Piskel is free and runs in a browser: **piskelapp.com**

**Sizes that fit the tutorial game:**

| Thing | Size of one frame |
|---|---|
| Player | 32 × 32 (or 16 × 16 for an RPG) |
| Enemy | 24 × 24 or 32 × 32 |
| Coin or pickup | 16 × 16 |
| Tiles | 16 × 16 |

**For animations** (player running, coin spinning):
1. Draw each frame in the frames panel on the left.
2. Click **Export → PNG → Spritesheet**. Put all frames in one row and download.
3. In Godot, load it like `knight.png` — **Add frames from sprite sheet** — and set the grid to your number of frames.

**Save your `.piskel` file** too (Export → Save as .piskel) and put it in an `art` folder in your repo.

**Found art:** only use art that's free to use. Write down where you got it for your README.

---

## Sound

You don't have to make your own sound. You can:

- reuse the music and sounds from the `brackeysAssets` folder
- use free sounds you find online (write down where you got them)
- make your own if you want, with **beepbox.co** (music) or **sfxr.me** (sound effects)

Put music in `assets/music` and sound effects in `assets/sounds`.

---

## What you turn in

All of this goes in your **team repo**:

1. **The game project**, pushed at the end of every lab day.
2. **`godot02_gamePlan.md`** with the game plan, your feature picks, and a Daily Log entry for each day.
3. **An `art` folder** with your `.piskel` files.
4. **`README.md`** with:
   - your game's name and a one-sentence description
   - the controls
   - a screenshot of your game
   - **who made what**: which sprites, and which features each person coded
   - where any found art or sound came from
