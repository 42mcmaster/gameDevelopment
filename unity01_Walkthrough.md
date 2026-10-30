# Unity 01 Walkthrough — Flappy Bird Tutorial

**Read this first. Then open `unity01_lastname.md` and do the work as you go.**

---

## Table of Contents

- [What you're doing](#what-youre-doing)
- [The video](#the-video)
- [Before you start: set up your repo](#before-you-start-set-up-your-repo)
- [Things that look different in our version of Unity](#things-that-look-different-in-our-version-of-unity)
- [Day 1 — Steps 1 and 2](#day-1--steps-1-and-2)
  - [Checkpoint 1: The bird is on screen](#checkpoint-1-the-bird-is-on-screen)
  - [Checkpoint 2: The bird flaps](#checkpoint-2-the-bird-flaps)
- [Day 2 — Steps 3, 4, and 5](#day-2--steps-3-4-and-5)
  - [Checkpoint 3: Pipes spawn and move](#checkpoint-3-pipes-spawn-and-move)
  - [Checkpoint 4: Score goes up](#checkpoint-4-score-goes-up)
  - [Checkpoint 5: Game over and restart](#checkpoint-5-game-over-and-restart)
- [If you finish early](#if-you-finish-early)
- [Words to know](#words-to-know)

---

## What you're doing

You are going to build a copy of **Flappy Bird** in Unity by following a video. The point is not Flappy Bird. The point is that this one small game uses almost everything you need for any 2D game:

- putting a character on screen
- moving it with the keyboard
- spawning objects and deleting them
- collisions
- keeping score
- game over and restart

This is **individual** work. Everyone on a team does this tutorial on their own computer. Your team's own game starts after this.

You have **two Thursdays** for this. Push to GitHub at every checkpoint, not just at the end.

---

## The video

**The Unity Tutorial For Complete Beginners** — Game Maker's Toolkit
https://www.youtube.com/watch?v=XtQMytORBmM

The video is about 45 minutes. Plan on it taking you two class periods, because you will pause, type, fix mistakes, and try things.

| Time | Section |
|---|---|
| 00:00 | Intro |
| 02:26 | Installing Unity — **skip this, Unity is already installed** |
| 03:42 | Step 1 — Unity UI |
| 07:11 | Step 2 — Physics and Programming |
| 16:09 | Step 3 — Spawning Objects |
| 28:32 | Step 4 — Logic and UI |
| 38:00 | Step 5 — Game Over |
| 43:31 | Next Steps |

The video description has a link to the bird and pipe pictures he uses. **Make your own bird and pipe if you can** — even a quick one in Piskel. If you use his, that's fine for this tutorial.

**Do not use the "Scripts" link in the description.** That copy of the code has mistakes in it (a missing line and a backwards `<`). Type the code yourself as you watch. That's how you learn it.

---

## Before you start: set up your repo

Unity projects make a lot of junk files that should **not** go to GitHub. The `Library` folder alone can be over a gigabyte. You keep it out with a `.gitignore` file.

1. In GitHub Desktop, create a new repository named **`gameDev`**.
2. In Unity Hub, create your new project **inside** that `gameDev` folder. Name the project **`FlappyBird_Lastname`** (your last name).
3. In the `gameDev` folder (not inside the Unity project), create a file named `.gitignore` and paste this in:

```
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

4. Copy `unity01_lastname.md` into the `gameDev` folder and rename it with your last name.

**Check:** In GitHub Desktop, the list of changed files should be small — things in `Assets`, `Packages`, and `ProjectSettings`. If you see hundreds or thousands of files, or anything in `Library`, your `.gitignore` is not working. Stop and ask before you push.

---

## Things that look different in our version of Unity

The video was made in **Unity 2021.3**. Our lab has a newer version. Most things are the same, but these will trip you up. Come back to this list when something doesn't match the video.

**1. Project template name.** The video picks **2D (Core)**. In newer Unity it may be called **2D (Built-In Render Pipeline)** or **Universal 2D**. Either one works for this tutorial.

**2. Spacebar does nothing, or you get a red error about `Input`.** Newer Unity uses a new input system by default, and the video's `Input.GetKeyDown` code uses the old one. Fix it once:
**Edit → Project Settings → Player → Other Settings → Active Input Handling → Both.** Unity will ask to restart. Say yes.

**3. `velocity` is underlined or gives a warning.** In newer Unity, `Rigidbody2D.velocity` was renamed to `linearVelocity`. Where the video types:

```csharp
myRigidbody.velocity = Vector2.up * flapStrength;
```

type this instead:

```csharp
myRigidbody.linearVelocity = Vector2.up * flapStrength;
```

**4. The code editor.** The video uses Visual Studio. Our lab computers may open scripts in a different editor. That's fine — the code is the same. If double-clicking a script opens nothing, ask.

**5. UI Text.** In Step 4 the video says text "may be under Legacy." In our version it is: **right-click in Hierarchy → UI → Legacy → Text**. Use the Legacy one so your code matches the video.

**6. UI Button.** Same thing in Step 5. Use **UI → Legacy → Button**. If Unity pops up a window asking to import "TMP Essentials," you picked the newer button by mistake. Close the window, delete that button, and make a Legacy one.

---

## Day 1 — Steps 1 and 2

Watch **03:42 to 15:30**. Pause and do each thing as he does it.

### Checkpoint 1: The bird is on screen

Stop at **06:49** (the recap).

You should have:
- a GameObject named **Bird** with a **Sprite Renderer** showing your bird
- the camera zoomed out and a background color you picked
- the game running when you press Play (nothing moves yet — that's right)

**Push to GitHub.** Commit message: `Checkpoint 1 - bird on screen`

### Checkpoint 2: The bird flaps

Stop at **15:30** (the recap).

You should have:
- a **Rigidbody 2D** and **Circle Collider 2D** on the bird
- a script named **BirdScript**
- the bird falls, and goes up when you press the spacebar
- a public **flapStrength** variable you can change in the Inspector

If the spacebar does nothing, go back to [item 2 above](#things-that-look-different-in-our-version-of-unity).

**Push to GitHub.** Commit message: `Checkpoint 2 - bird flaps`

Answer the Day 1 questions in your `unity01_lastname.md` before you leave.

---

## Day 2 — Steps 3, 4, and 5

### Checkpoint 3: Pipes spawn and move

Watch **16:09 to 27:52**.

You should have:
- a **Pipe** prefab (a parent object with a top pipe and bottom pipe inside it)
- a **PipeMoveScript** that moves pipes left using `Time.deltaTime`
- a **Pipe Spawner** that makes new pipes on a timer, at random heights
- pipes that delete themselves after they leave the screen
- a `Debug.Log` message in the **Console** each time a pipe is deleted

**Push to GitHub.** Commit message: `Checkpoint 3 - pipes spawn`

### Checkpoint 4: Score goes up

Watch **28:32 to 37:12**.

You should have:
- a score number in the corner of the screen (Legacy Text)
- a **Logic Manager** object with a **LogicScript** that has an `addScore` function
- the Logic Manager **tagged** `Logic` (he warns you that you'll forget this — check it)
- an invisible **trigger** between the pipes that adds 1 to the score when the bird flies through
- the bird on its own **Layer** (he uses layer 3), so that only the bird can score — not a pipe bumping into the trigger

**Watch for:** near the end of Step 4 he changes `addScore()` so it takes a number, like `addScore(1)`. If your score code stops working after that, check that you changed it in **both** scripts.

**Push to GitHub.** Commit message: `Checkpoint 4 - score works`

### Checkpoint 5: Game over and restart

Watch **38:00 to 43:31**.

You should have:
- a **Game Over** screen (text and a button) that is turned **off** when the game starts
- `OnCollisionEnter2D` in **BirdScript** that runs `gameOver()` when the bird hits a pipe
- a `gameOver()` function in **LogicScript** that turns the Game Over screen on with `SetActive(true)`
- a `restartGame()` function that reloads the scene, hooked up to the button's **On Click** box
- a `birdIsAlive` true/false variable, so the bird can't keep flapping after game over

**Watch for:** `SceneManager` won't work until you add `using UnityEngine.SceneManagement;` at the top of LogicScript — the same idea as `using UnityEngine.UI;` in Step 4.

**Push to GitHub.** Commit message: `Checkpoint 5 - game over`

Finish the Day 2 questions in your `unity01_lastname.md` and push one last time.

---

## If you finish early

Watch **43:31 to the end**. He gives you a list of things to try on your own. Pick **at least one** and add it. Some good ones:

- **Sound effects** — play a sound when the bird scores or crashes. Look up `AudioSource` in the Unity docs. You can make a sound in one click at **sfxr.me**.
- **High score** — save the best score even after the game closes. Look up `PlayerPrefs`.
- **Game over when the bird goes off the top or bottom of the screen.**
- **Your own art** — replace the bird and pipes with your own Piskel sprites.

Write down what you added in your `unity01_lastname.md`.

---

## Words to know

| Word | What it means |
|---|---|
| **GameObject** | An invisible container in your scene. It has a position, rotation, and scale. Everything in the game is one — the bird, the pipes, the camera, the score text. |
| **Component** | Something you add to a GameObject to give it a feature, like a Sprite Renderer (shows a picture) or a Rigidbody 2D (gravity and physics). |
| **Hierarchy** | The panel that lists every GameObject in the current scene. |
| **Inspector** | The panel where you see and change the components on the GameObject you have selected. |
| **Project panel** | Every file in your game — pictures, sounds, scripts, prefabs. |
| **Script** | A component you write yourself in C#. |
| **Start()** | Code inside it runs **once**, when the script first turns on. |
| **Update()** | Code inside it runs **every frame**, over and over. |
| **Reference** | A slot in your script that points at another component or object so the script can talk to it. You make the slot in code, then fill it by dragging in Unity. |
| **public** | Makes a variable show up in the Inspector so you can change it without editing code. |
| **float** | A number that can have a decimal, like `10.5`. |
| **int** | A whole number, like `3`. |
| **Vector2 / Vector3** | Two or three numbers grouped together, used for positions and directions (x, y) or (x, y, z). |
| **Time.deltaTime** | The time since the last frame. Multiply movement by it so the game runs the same speed on fast and slow computers. |
| **Prefab** | A saved blueprint of a GameObject that you can make copies of while the game runs. |
| **Instantiate** | The command that makes a new copy of a prefab. |
| **Collider** | The invisible shape Unity uses to check if two objects touch. |
| **Trigger** | A collider that doesn't block anything. It only tells your code "something touched me." |
| **Tag** | A label you put on a GameObject so code can find it, like `Logic`. |
| **Debug.Log** | Prints a message to the Console so you can see what your code is doing. |
| `=` vs `==` | One equals sign **sets** a value. Two equals signs **checks** if two things are the same. |
