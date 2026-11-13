# Sharing a GitHub Repository with a Partner

This guide shows how two students set up one GitHub repository and work on the same project without losing each other's work. It uses a Godot game project as the example, but the same steps work for any partner project.

## Table of Contents

- [Sharing a GitHub Repository with a Partner](#sharing-a-github-repository-with-a-partner)
  - [Table of Contents](#table-of-contents)
  - [1. Key Words](#1-key-words)
  - [2. Decide Who Is the Owner](#2-decide-who-is-the-owner)
  - [3. Owner: Create the Repository](#3-owner-create-the-repository)
  - [4. Owner: Add Your Partner as a Collaborator](#4-owner-add-your-partner-as-a-collaborator)
  - [5. Partner: Accept the Invitation](#5-partner-accept-the-invitation)
  - [6. Both: Clone the Repository to Your Computer](#6-both-clone-the-repository-to-your-computer)
  - [7. Owner: Put the Godot Project in the Repository](#7-owner-put-the-godot-project-in-the-repository)
  - [8. Partner: Open the Project](#8-partner-open-the-project)
  - [9. The Daily Routine: Pull, Work, Commit, Push](#9-the-daily-routine-pull-work-commit-push)
  - [10. How to Split the Work So You Do Not Overwrite Each Other](#10-how-to-split-the-work-so-you-do-not-overwrite-each-other)
  - [11. Saving Your Exported Game (.exe) with Git LFS](#11-saving-your-exported-game-exe-with-git-lfs)
  - [12. Using Branches (Optional)](#12-using-branches-optional)
  - [13. Fixing a Merge Conflict](#13-fixing-a-merge-conflict)
  - [14. Common Problems and Fixes](#14-common-problems-and-fixes)
  - [15. Quick Reference Checklist](#15-quick-reference-checklist)

---

## 1. Key Words

| Word | What it means |
|---|---|
| **Repository (repo)** | The project folder that GitHub keeps track of. |
| **Owner** | The person whose GitHub account the repo lives under. |
| **Collaborator** | A person the owner has given permission to change the repo. |
| **Clone** | Download a copy of the repo to your computer, connected to GitHub. |
| **Commit** | Save a snapshot of your changes on your computer, with a short message. |
| **Push** | Send your commits from your computer up to GitHub. |
| **Pull** | Bring your partner's commits down from GitHub to your computer. |
| **Merge conflict** | Both partners changed the same lines of the same file. Git needs a person to pick which version to keep. |
| **Git LFS (Large File Storage)** | An add-on that lets GitHub store big files, like your exported `.exe`, that are too large for a normal push. |

**The most important idea:** GitHub is the shared copy. Your computer has your own copy. You stay in sync by **pulling before you work** and **pushing when you finish**.

---

## 2. Decide Who Is the Owner

Only **one** person creates the repo. Talk with your partner and pick:

- **Owner** – creates the repo, adds the partner, and sets up the Godot project.
- **Partner** – accepts the invite and clones the repo.

After setup, both of you have the same permissions to pull, commit, and push. The only difference is whose account the repo is under.

> Do **not** both create a repo. Do **not** copy the project folder on a flash drive or by email. That creates two separate projects that cannot be combined easily.

---

## 3. Owner: Create the Repository

1. Go to [github.com](https://github.com) and sign in.
2. Click the **+** in the top-right corner, then **New repository**.
3. Fill in the form:
   - **Repository name:** something clear, for example `team-platformer-smith-jones`.
   - **Description:** one sentence about your game.
   - **Public** (unless your instructor says otherwise).
   - Check **Add a README file**.
   - **Add .gitignore:** pick **Godot** from the list.
4. Click **Create repository**.

**Why the Godot .gitignore matters:** Godot makes a hidden folder called `.godot/` full of cache files that are rebuilt on each computer. If those files get pushed, they cause constant conflicts between partners. The `.gitignore` file tells Git to skip them.

---

## 4. Owner: Add Your Partner as a Collaborator

1. On your repo page, click **Settings** (the gear tab at the top of the repo, not your account settings).
2. In the left menu, click **Collaborators** (it may be under the **Access** heading).
3. GitHub may ask you to confirm your password or use your authenticator. Do that.
4. Click **Add people**.
5. Type your partner's **GitHub username** (ask them for it; spelling matters).
6. Pick their account from the list and click **Add [name] to this repository**.

Your partner now shows as **Pending invite**. They are not a collaborator until they accept.

> The invitation expires after **7 days**. If it expires, remove it and send a new one.

---

## 5. Partner: Accept the Invitation

1. Check the email address tied to your GitHub account, **or** sign in to GitHub and look for the notification bell in the top right.
2. Open the invitation and click **Accept invitation**.

If you cannot find it, go to this address, replacing the parts in capitals:

```
https://github.com/OWNER-USERNAME/REPO-NAME/invitations
```

After you accept, the owner's page will show you as a collaborator instead of "Pending."

---

## 6. Both: Clone the Repository to Your Computer

Both partners do this step. Use **one** of the two methods below.

### Method A: GitHub Desktop

1. Open **GitHub Desktop** and sign in with your GitHub account.
2. Click **File > Clone repository**.
3. Pick the **GitHub.com** tab and find the repo in the list. (Partners: it shows up under the owner's name.)
4. Under **Local path**, pick where to save it. Choose a folder you can find again, such as `Documents/GitHub`.
5. Click **Clone**.

### Method B: Command Line (Terminal or Git Bash)

1. On the repo page on GitHub, click the green **Code** button and copy the HTTPS address.
2. In your terminal, go to the folder where you keep projects and run:

```bash
# Move into the folder where you keep projects
cd Documents/GitHub

# Download the repo (paste the address you copied)
git clone https://github.com/OWNER-USERNAME/REPO-NAME.git

# Move into the new project folder
cd REPO-NAME
```

You now have a folder on your computer with the README and `.gitignore` inside.

---

## 7. Owner: Put the Godot Project in the Repository

Only the **owner** does this, and only **once**. The goal is to have `project.godot` sit directly inside the repo folder.

1. Open Godot. In the Project Manager, click **Create** (or **New Project**).
2. For **Project Path**, browse to the repo folder you just cloned and select it.
   - Godot may warn that the folder is not empty because of the README and `.gitignore`. That is fine. Continue.
3. Create the project. Make your first scene and **save** it.
4. Close Godot or keep it open, then **commit and push**:

**GitHub Desktop:**
1. You will see the new files in the **Changes** list on the left.
2. Check that the `.godot/` folder is **not** in the list. (If it is, the `.gitignore` is missing. See [Common Problems](#14-common-problems-and-fixes).)
3. In the **Summary** box at the bottom left, type `Set up Godot project`.
4. Click **Commit to main**.
5. Click **Push origin** at the top.

**Command line:**

```bash
# Stage every new and changed file
git add .

# Save a snapshot with a message
git commit -m "Set up Godot project"

# Send it to GitHub
git push
```

5. Refresh the repo page on GitHub. You should see `project.godot` and your scene file.
6. Tell your partner the project is ready.

**If you already started the project in another folder:** close Godot, copy everything **inside** that project folder (including `project.godot`) into the repo folder, then commit and push. Do not copy the `.godot/` folder; Godot will rebuild it.

---

## 8. Partner: Open the Project

1. Pull the owner's work:
   - **GitHub Desktop:** click **Fetch origin**, then **Pull origin**.
   - **Command line:** `git pull`
2. Open Godot. In the Project Manager, click **Import**.
3. Browse to your repo folder and select the `project.godot` file.
4. Click **Import & Edit**.

Godot will take a moment to rebuild its `.godot/` cache folder. This is normal and happens the first time on each computer.

---

## 9. The Daily Routine: Pull, Work, Commit, Push

Follow this **every lab period**. Most partner problems come from skipping step 1 or step 4.

### Step 1: Pull first (start of class)

Get your partner's latest work before you change anything.

- **GitHub Desktop:** **Fetch origin**, then **Pull origin** if it appears.
- **Command line:** `git pull`

If Godot is already open and says files changed on disk, choose **Reload**.

### Step 2: Work on your part

Build your scenes, scripts, and sprites. **Save often in Godot** (Ctrl+S / Cmd+S, or **Scene > Save All Scenes**).

### Step 3: Commit small, commit often

Commit each time you finish a small piece that works, for example "Added player jump" or "Added coin pickup sound."

- Write a message that says **what** you did, not "stuff" or "update."
- Commit at least 2–3 times per class.

**Command line:**

```bash
git add .
git commit -m "Added enemy patrol script"
```

### Step 4: Pull again, then push (end of class, or any time you finish something)

Your partner may have pushed while you worked. Pull first, then push.

- **GitHub Desktop:** **Fetch origin** → **Pull origin** (if shown) → **Push origin**.
- **Command line:**

```bash
# Get partner's newest work and combine it with yours
git pull

# Send your commits to GitHub
git push
```

### Step 5: Tell your partner

Say it out loud or in a message: "I just pushed the enemy. Pull before you start."

> **Rule of thumb:** Never leave class with work that is not pushed. If you are absent tomorrow, your partner cannot get your work from your computer.

---

## 10. How to Split the Work So You Do Not Overwrite Each Other

Git is good at combining changes to **different files**. It has trouble when both people change the **same file** at the same time. Godot scene files (`.tscn`) are especially messy to fix by hand, so plan your work around them.

### Give each person their own files

| Partner A owns | Partner B owns |
|---|---|
| `player.tscn`, `player.gd` | `enemy.tscn`, `enemy.gd` |
| `coin.tscn`, `coin.gd` | `hud.tscn`, `game_manager.gd` |
| Player sprites | Enemy and item sprites |

Each person builds their pieces as **separate scenes**. Then the pieces are **instanced** (dragged in) to the level scene.

### Shared files: take turns

Some files both people need, such as the main level scene and `project.godot` (which changes when you edit **Project Settings** or **Input Map**). For these:

1. Agree who is editing it right now. Say "I'm in the level scene."
2. That person edits, saves, commits, and pushes.
3. Then the other person pulls before touching it.

### Sprites and other art files

Image and sound files cannot be merged. If both people change the same `.png`, one version will be lost. Each sprite should have **one** person in charge of it.

---

## 11. Saving Your Exported Game (.exe) with Git LFS

When your game is ready to play, you export it from Godot as a Windows `.exe`. You can keep that `.exe` in your repo so anyone can download and play it. Everything in this section is done in **GitHub Desktop**, Godot, and a text editor. No command line.

### Why you need Git LFS

A Godot `.exe` is usually **60–100 MB or more**, because it includes the whole Godot engine. GitHub **blocks any file over 100 MB** and warns about files over 50 MB. Git LFS fixes this: GitHub stores the big file in a separate place, and your repo just keeps a small pointer to it. GitHub Desktop already includes Git LFS, so there is nothing extra to install.

**Storage limits:** A free GitHub account gets **10 GB of LFS storage and 10 GB of LFS downloads per month**. All of it counts against the **owner's** account, even when the partner pushes. Every version of the `.exe` you push stays in storage, so a 90 MB game exported 20 times uses about 1.8 GB. That is why the [rules for exports](#rules-for-exports) say to export only at milestones.

> **Set up LFS before you commit the `.exe`.** If you commit the `.exe` first, the push will fail, and you will have to undo the commit (see [Common Problems](#14-common-problems-and-fixes)).

### Step 1: Owner: Tell Git to use LFS for exported files (once)

You do this by making a small text file called `.gitattributes` in the main repo folder (the same folder as `project.godot` and `.gitignore`).

1. Open **VS Code** or **Notepad**.
2. Paste these two lines exactly:

   ```text
   *.exe filter=lfs diff=lfs merge=lfs -text
   *.pck filter=lfs diff=lfs merge=lfs -text
   ```

   - Line 1 means "store every `.exe` file with LFS."
   - Line 2 does the same for `.pck` files, which hold your game's data if it is not built into the `.exe`.

3. Save the file in the main repo folder with the name **`.gitattributes`** (it starts with a dot and has no other extension).
   - **In Notepad:** choose **File > Save As**, set **Save as type** to **All Files (\*.\*)**, then type `.gitattributes` as the file name. If you skip this, Notepad saves it as `.gitattributes.txt` and it will not work.
4. Open **GitHub Desktop**. `.gitattributes` should appear in the **Changes** list.
5. Summary: `Set up Git LFS for exports` → **Commit to main** → **Push origin**.
6. If GitHub Desktop shows a message asking to **Initialize Git LFS**, click **Initialize Git LFS**.
7. Tell your partner to **pull**.

### Step 2: Export the game into a builds folder

1. In your repo folder, make a new folder called **`builds`**.
2. In Godot, go to **Project > Export**.
3. Click **Add...** and choose **Windows Desktop**.
   - If Godot says export templates are missing, click **Manage Export Templates**, then **Download and Install**. Wait for it to finish, then come back to **Project > Export**.
4. In the options on the right, find **Binary Format** and check **Embed PCK**. This puts the whole game into one `.exe` file instead of an `.exe` plus a `.pck`.
5. Set **Export Path** to the `builds` folder, for example `builds/MyGame.exe`.
6. Click **Export Project**. For a finished version, uncheck **Export With Debug**. Then click **Save**.

Godot may also make a file ending in `.console.exe`. It is used for finding bugs. You can delete it, or leave it and LFS will store it too.

### Step 3: Commit and push the export

1. Open **GitHub Desktop**. The `.exe` in `builds` will be in the **Changes** list.
2. Summary: something like `Exported playable version 1`.
3. **Commit to main** → **Push origin**.

The push takes longer than usual because the file is large. Wait for it to finish before closing GitHub Desktop.

### Step 4: Check that it worked

1. On GitHub, open the repo, click the `builds` folder, then click your `.exe`.
2. The page should say **Stored with Git LFS**.

If it does not say that, the `.gitattributes` file is missing, misnamed, or not in the main repo folder.

### Step 5: Partner: Get the exported game

1. In GitHub Desktop: **Fetch origin** → **Pull origin**.
2. If GitHub Desktop asks to **Initialize Git LFS**, click **Initialize Git LFS**.
3. Open the `builds` folder. Double-click the `.exe` to play.

### Rules for exports

- **Only export at milestones** (a working level, the final version), not every class. Every export you push uses up the repo's LFS storage.
- **One person exports at a time.** An `.exe` cannot be merged. If both people export and push, whoever pushes last replaces the other's file.
- **Use the same file name** each time (for example `builds/MyGame.exe`), so the new export replaces the old one instead of piling up.
- **Pull and test first.** Before you export, pull so the export includes both partners' newest work.

---

## 12. Using Branches (Optional)

A branch is a separate line of work. You can work on a branch without changing `main` until you are ready. Branches are helpful when you are trying something that might break the game.

### Make a branch

- **GitHub Desktop:** **Current branch** (top bar) → **New branch** → name it, for example `enemy-ai` → **Create branch** → **Publish branch**.
- **Command line:**

```bash
# Create a new branch and switch to it
git checkout -b enemy-ai

# Push it to GitHub the first time
git push -u origin enemy-ai
```

Commit and push on your branch as normal.

### Bring the branch into main when it works

1. On GitHub, go to the repo. You will see a yellow bar offering **Compare & pull request**. Click it.
2. Write a short description of what you added and click **Create pull request**.
3. Your partner looks at the changes, then clicks **Merge pull request**.
4. Both partners switch back to `main` and pull.

If you are new to Git, it is fine to both work on `main`, as long as you follow the daily routine and split the files.

---

## 13. Fixing a Merge Conflict

A conflict happens when you and your partner changed the same lines in the same file. Git stops and asks you to choose. **Nothing is lost**; both versions are still there.

### What a conflict looks like inside a file

```gdscript
<<<<<<< HEAD
var speed = 300   # your version
=======
var speed = 250   # your partner's version
>>>>>>> main
```

- Between `<<<<<<< HEAD` and `=======` is **your** version.
- Between `=======` and `>>>>>>>` is the **other** version.

### Steps to fix it

1. **Talk to your partner.** Decide which version to keep, or combine them.
2. Open the file in a text editor (VS Code, or the Godot script editor for `.gd` files).
3. Delete the version you do not want **and** delete the three marker lines (`<<<<<<<`, `=======`, `>>>>>>>`).

   ```gdscript
   var speed = 275   # agreed value
   ```

4. Save the file.
5. Finish the merge:
   - **GitHub Desktop:** the file will show as resolved. Click **Continue merge** (or **Commit merge**), then **Push origin**.
   - **Command line:**

   ```bash
   # Mark the file as fixed
   git add player.gd

   # Finish the merge
   git commit -m "Fixed conflict in player speed"

   # Send it up
   git push
   ```

### If the conflict is in a `.tscn` scene file

Scene files are hard to read by hand. The easier fix is usually to keep **one** person's whole version of the file, then redo the other person's small change in Godot.

- **GitHub Desktop:** right-click the conflicted file and choose **Use the modified file from main** (theirs) or **Use my modified file** (yours).
- **Command line:**

```bash
# Keep your partner's version of the file
git checkout --theirs level.tscn

# OR keep your version of the file
git checkout --ours level.tscn

# Then mark it fixed and finish
git add level.tscn
git commit -m "Kept partner's level scene"
git push
```

Then open Godot, reload, and redo the missing change.

---

## 14. Common Problems and Fixes

| Problem | What it means | Fix |
|---|---|---|
| **Push is rejected** ("Updates were rejected because the remote contains work...") | Your partner pushed and you have not pulled yet. | Pull first, then push. |
| **Partner can't push** ("Permission denied" or 403 error) | They never accepted the invitation, or it expired. | Owner checks **Settings > Collaborators**. Re-send the invite if needed. Partner accepts it. |
| **Repo doesn't show in partner's GitHub Desktop** | Invitation not accepted yet, or not signed in to the right account. | Accept the invite; check **File > Options > Accounts** in GitHub Desktop. |
| **`.godot/` folder shows up in your changes** | The `.gitignore` is missing or in the wrong place. | Make sure `.gitignore` is in the same folder as `project.godot`, and it has the line `.godot/` in it. |
| **Godot can't find the project after pulling** | `project.godot` is inside an extra subfolder. | Use **Import** in Godot and browse into the subfolder to find `project.godot`. |
| **Partner's new scene shows errors or missing resources** | They forgot to commit a file the scene uses (a sprite or script). | Ask them to check their Changes list, commit the missing file, and push. |
| **"You have uncommitted changes" when pulling** | You changed files and did not commit. | Commit your work first, then pull. |
| **Godot shows old content after pulling** | Godot did not reload the files. | Choose **Reload** when asked, or close and reopen the project. |
| **Push fails with "file is larger than 100 MB" or "exceeds GitHub's file size limit"** | The `.exe` was committed before Git LFS was set up. | Do not push again. In GitHub Desktop, open the **History** tab, right-click your newest commit, and choose **Undo commit** (repeat if the `.exe` is in more than one unpushed commit). Set up `.gitattributes` ([Section 11, Step 1](#step-1-owner-tell-git-to-use-lfs-for-exported-files-once)), commit and push that, then commit the `.exe` again. If the commit was already pushed or Undo is not offered, ask your instructor. |
| **The `.exe` on your computer is tiny (about 1 KB) and won't run** | You downloaded the LFS pointer, not the real file. Git LFS is not turned on for this repo on your computer. | In GitHub Desktop, choose **Repository > Remove** (this does **not** delete your files). Then choose **File > Add local repository**, pick the repo folder, and click **Initialize Git LFS** when asked. Pull again. |
| **`.gitattributes` doesn't show up in GitHub Desktop, or LFS isn't working** | The file was saved as `.gitattributes.txt`, or saved in the wrong folder. | Save it again with **Save as type: All Files**, in the same folder as `project.godot`. |
| **Something is badly broken** | A bad commit got pushed. | Stop. Do not keep committing. Ask your instructor; Git keeps every past version and it can be restored. |

---

## 15. Quick Reference Checklist

### Setup (once)

- [ ] Pick an owner and a partner.
- [ ] Owner creates the repo with a README and the **Godot** `.gitignore`.
- [ ] Owner adds the partner in **Settings > Collaborators**.
- [ ] Partner accepts the invitation within 7 days.
- [ ] Both clone the repo.
- [ ] Owner creates the Godot project inside the repo folder, commits, and pushes.
- [ ] Partner pulls and imports `project.godot` in Godot.
- [ ] Agree on who owns which scenes, scripts, and sprites.
- [ ] Owner adds `.gitattributes` for Git LFS, then commits and pushes it **before** anyone commits an `.exe`.

### Exporting the game

- [ ] Pull so the export has both partners' work.
- [ ] Export from Godot to `builds/` with **Embed PCK** checked.
- [ ] Commit and push from GitHub Desktop.
- [ ] Check on GitHub that the `.exe` says **Stored with Git LFS**.

### Every class

- [ ] **Pull** before you start.
- [ ] Work only on **your** files, or say out loud when you need a shared file.
- [ ] **Save** in Godot often.
- [ ] **Commit** small pieces with clear messages.
- [ ] **Pull** again, then **push** before you leave.
- [ ] Tell your partner what you pushed.
