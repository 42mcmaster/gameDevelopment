# Godot 02 Walkthrough — Team Game

**Read this first. Then one person on your team copies `godot02_gamePlan.md` into your team repo and you fill it in together.**

---

## Table of Contents

- [What you're doing](#what-youre-doing)
- [Your team and roles](#your-team-and-roles)
- [Week by week](#week-by-week)
- [Set up your team repo](#set-up-your-team-repo)
- [Working on the same project without breaking it](#working-on-the-same-project-without-breaking-it)
- [Required changes (every team)](#required-changes-every-team)
- [Pick at least 3 from the menu](#pick-at-least-3-from-the-menu)
- [Making your art in Piskel](#making-your-art-in-piskel)
- [Making your music and sound effects](#making-your-music-and-sound-effects)
- [What you turn in](#what-you-turn-in)
- [How it's graded](#how-its-graded)

---

## What you're doing

You finished the Godot tutorial on your own. Now you and a partner take that platformer and **turn it into your own game**.

You are not starting from a blank project. You start with a game that already works and then:

- replace **all** the art and sound with your own
- give it a new name, a new theme, and new levels
- add **at least 3 new features** from the menu below

When you're done, someone who played the tutorial game should look at yours and say "that's a different game."

You have **7 Thursdays** for this. You can also work on it on Tuesdays if you finish your regular class work.

---

## Your team and roles

Teams are **2 people**. Both of you write code. On top of that, each of you leads one area:

| Role | What you're in charge of |
|---|---|
| **Art lead** | The player, enemies, coins or pickups, and tiles — made in Piskel |
| **Sound lead** | The music track (BeepBox) and sound effects (jsfxr) |

"Lead" means you make sure it gets done. It doesn't mean your partner can't help.

In the Unity round, you **switch roles**.

---

## Week by week

| Week | What to get done | Push by the end of class |
|---|---|---|
| **1** | Make the team repo. Copy one partner's tutorial game into it. Fill out the **Game Plan** section of `godot02_gamePlan.md`. | Repo set up, game plan filled out |
| **2** | Art lead: player sprite. Sound lead: first music track. Both: start redesigning the level. | Player sprite and music in the game |
| **3** | Art lead: enemy, coin or pickup, and tiles. Sound lead: sound effects. Both: finish level 1. | All art and sound replaced |
| **4** | Build menu feature #1. | Feature #1 working |
| **5** | Build menu features #2 and #3. | Features #2 and #3 working |
| **6** | **Playtest.** Another team plays your game and writes notes. Fix what they found. | Fixes pushed |
| **7** | Finish your `README.md`. **Demo** your game to the class. | Final push |

Every week, add an entry to the **Weekly Log** in your `godot02_gamePlan.md` before you leave. That's your weekly grade.

If you fall behind, talk to your teacher. Cut a menu feature before you cut the art or the sound.

---

## Set up your team repo

Only **one** partner does steps 1–5. The other partner does step 6.

1. In GitHub Desktop, create a new repository named **`teamGame_Lastname1_Lastname2`** (both last names, in alphabetical order).
2. Copy the `.gitignore` from your `gameDev` repo into this new repo. It already has the `.godot/` line.
3. Pick whichever partner's tutorial game works best. Copy that whole `FirstGame_Lastname` folder into the team repo and rename the folder to your new game's name, with no spaces (for example `SpaceSlime`).
4. Open the project in Godot. Go to **Project → Project Settings → Application → Config → Name** and change it to your game's name.
5. Copy `godot02_gamePlan.md` into the team repo. **Commit and push.**
6. **Add your partner:** on github.com, open the repo, go to **Settings → Collaborators → Add people**, and add your partner's GitHub username. Your partner accepts the invite in their email or on GitHub, then clones the repo in GitHub Desktop.

**Check:** both of you can see the repo in GitHub Desktop, and both of you can open the game in Godot and press Play.

---

## Working on the same project without breaking it

Two people editing the same Godot scene at the same time causes a **merge conflict**. Godot scene files (`.tscn`) are hard to fix when that happens. Follow these rules and you'll avoid most of it.

1. **Pull before you start.** Every class, the first thing you do in GitHub Desktop is **Fetch origin**, then **Pull**.
2. **Push when you stop.** Every class, the last thing you do is commit and **Push**. Never leave with work that isn't pushed.
3. **Don't edit the same scene at the same time.** Say out loud who is working on what. Example: "I'm in `player.tscn`, you take `slime.tscn`."
4. **`game.tscn` is one person at a time.** That's the level, and everyone wants to touch it. Take turns.
5. **Art files are safe.** Making sprites in Piskel or music in BeepBox doesn't touch any Godot files, so the art and sound leads can work on those any time.

**If GitHub Desktop says there's a conflict:** stop and get your teacher. Don't just pick one side and push.

---

## Required changes (every team)

Every team must do all of these:

- [ ] **New name and theme.** It's not a knight anymore. Pick your own idea: a robot in a factory, a frog in a swamp, an astronaut on a moon, whatever you want.
- [ ] **All new art.** Player, enemy, coins or pickups, and the tiles you use — all made by your team in Piskel. No Brackeys art left in the game.
- [ ] **All new sound.** Your own music track from BeepBox and at least **2** sound effects from jsfxr. No Brackeys sounds left in the game.
- [ ] **A new level.** Redesign the level layout. Don't just repaint the tutorial level with new tiles.
- [ ] **New text.** Change the signs and the score message to fit your game.

---

## Pick at least 3 from the menu

Pick **at least 3** features. **At least 1 must be from the Harder list.** Write your picks in your game plan in Week 1.

### Easier

| Feature | Hint |
|---|---|
| **Spikes or a trap** | It's another use of your killzone scene. |
| **A second enemy type** | Use the purple slime (or your own) with a different speed, or one that patrols a longer path. |
| **A pickup worth more points** | Copy the coin scene and change how many points it adds. |
| **A win message at the end** | A label or area at the end of the level that says you won. |
| **New player animations** | A landing animation, a hurt animation, or a different idle. |

### Harder

| Feature | Hint |
|---|---|
| **Double jump** | Keep a count of jumps. Reset it when `is_on_floor()` is true. |
| **Lives or health** | The player survives a few hits before restarting. Store lives in the GameManager. |
| **A countdown timer** | Show time left on a label. When it hits zero, the player loses. |
| **A second level** | Make a new level scene and switch to it when the player reaches the end. Look up `change_scene_to_file`. |
| **A title screen** | A start scene with your game's name and a **Start** button. |
| **A power-up** | A pickup that makes the player faster or jump higher for a few seconds. Use a Timer. |
| **A falling or disappearing platform** | Use an AnimationPlayer or a Timer to make it drop or vanish after you land on it. |
| **A checkpoint** | When you touch it, you respawn there instead of at the start. |

**Your own idea:** if you want a feature that isn't on the list, write it in your game plan and get your teacher's OK in Week 1.

---

## Making your art in Piskel

Piskel is free and runs in a browser: **piskelapp.com**

**Use these sizes so your art fits the tutorial game:**

| Thing | Size of one frame |
|---|---|
| Player | 32 × 32 |
| Enemy | 24 × 24 or 32 × 32 |
| Coin or pickup | 16 × 16 |
| Tiles | 16 × 16 |

**For animations** (player running, coin spinning):
1. Draw each frame in the frames panel on the left.
2. Click **Export → PNG → Spritesheet**.
3. Set the columns so all frames go in one row, then download.
4. In Godot, load it the same way you loaded `knight.png` — with **Add frames from sprite sheet** — and set the grid to match your number of frames.

**Save your `.piskel` file** too (Export → Save as .piskel) and put it in an `art` folder in your repo. That way you can edit it later.

---

## Making your music and sound effects

**Music — BeepBox:** **beepbox.co**
- Click squares on the grid to place notes. Press space to play.
- Keep it short (8–16 bars) and loop it in Godot like the tutorial music.
- **File → Export Song → .wav**. Put it in your project's `assets/music` folder.

**Sound effects — jsfxr:** **sfxr.me**
- Click a button like **Jump**, **Pickup/Coin**, or **Hit/Hurt** until you get a sound you like.
- Click **Export .wav**. Put it in your project's `assets/sounds` folder.

You need at least **2** sound effects. Good ones to make: jump, coin, hurt, enemy dies, power-up.

If either site is blocked at school, tell your teacher right away.

---

## What you turn in

All of this goes in your **team repo**:

1. **The game project**, pushed every week.
2. **`godot02_gamePlan.md`** with the game plan, your menu picks, and a Weekly Log entry every week.
3. **An `art` folder** with your `.piskel` files.
4. **`README.md`** (Week 7) with:
   - your game's name and a one-sentence description
   - the controls
   - a screenshot of your game
   - **who made what** — art, sound, and which features each person coded
5. **A demo** to the class in Week 7: about 2 minutes. Play your game and show your 3+ features.

---

## How it's graded

**Weekly Log — 5 points each week.** You get the 5 if your log entry is there and your work is pushed.

**Final game — 5 points:**

| Score | What it looks like |
|---|---|
| **5** | All required changes done. At least 3 menu features working. README done. |
| **4** | Missing one or two small things — like one sound effect, or one feature that only half works. |
| **3** | The game runs and has some new art and sound, but several required changes or features are missing. |
| **2** | The game is mostly the tutorial game with a few changes. |

**Demo — 5 points.** Show the game, show your features, and both partners talk.
