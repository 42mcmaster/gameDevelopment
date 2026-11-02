# Godot 01 — First Game Tutorial

You will build a small 2D platformer by following a video. A knight runs, jumps, collects coins, rides a moving platform, and avoids a slime. Do this on your own computer. Your team game comes after this.

**Time:** 3 Thursdays.

---

## First make sure you have the software 

Download and install the Godot Engine (Not the .NET version) at https://godotengine.org/download/windows/.

You'll need to extract the zipped file into a location that you'll access frequently, but don't save it to your downloads.  I suggest your documents folder. 

---

## The video

**How to make a Video Game – Godot Beginner Tutorial** (Brackeys)
https://www.youtube.com/watch?v=LOhfqjmasi0

| Day | Watch | What you'll have at the end |
|---|---|---|
| **Day 1** | 0:00 to 28:20 | The knight moves and jumps on a tile level with platforms |
| **Day 2** | 28:20 to 52:56 | Coins, falling restarts the game, a slime enemy |
| **Day 3** | 52:56 to the end | Run/jump animations, a score, music and sound |

**Art and sound:** don't use the link in the video description. The files are in the **`brackeysAssets`** folder next to this file. At 3:22, drag those four folders (fonts, music, sounds, sprites) into Godot.

---

## Day 1 setup (do this once)

1. In **GitHub Desktop**: File → New Repository. Name it **`gameDev`**. Click Create Repository.
2. In **Godot**: click **Create**. Name the project **`FirstGame_Lastname`** and put it **inside your `gameDev` folder**. Click Create.

Godot keeps its own extra files out of GitHub for you. You don't need to do anything else.

---

## Pushing to GitHub (end of every class)

At the end of each Thursday, push your whole project:

1. Save in Godot (Ctrl+S).
2. Open GitHub Desktop.
3. In the Summary box, type **Day 1**, **Day 2**, or **Day 3**.
4. Click **Commit to main**, then **Push origin**.

That's it. The whole `FirstGame_Lastname` folder goes up. Your teacher will check that your game gets further each day.

---

## When something doesn't match the video

The video uses an older version of Godot. These are the spots that look different:

- **About 17:00 — tile map.** Search for **`TileMapLayer`**, not `TileMap`. Everything after that works the same.
- **About 40:00 — background tiles.** Instead of adding a layer, add a **second `TileMapLayer` node** and name it Background. Drag it **above** the first one in the Scene list. To give it the same tiles: click the Tile Set box on the first one → **Copy**, then on the new one → **Paste**.
- **The player script looks a little different** from his. That's fine.
- **Blurry knight?** Project → Project Settings → Rendering → Textures → Default Texture Filter → **Nearest**.
- **Indentation errors?** Lines inside a function or an `if` must be tabbed in the same amount, like Python.
- **Export at the end** might not work on the school network. Try it. If it doesn't work, skip it.

---

## If you finish early

Add one thing on your own: more level, spikes (copy the killzone), a double jump, or your own sprite made in Piskel. Push it.
