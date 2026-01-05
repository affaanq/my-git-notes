# Git In-Depth: Learning Notes

*Notes and key takeaways from the Frontend Masters "Git In-Depth" course.*

## Core Concepts: Porcelain vs. Plumbing
Git is built in layers. Understanding the difference helps when things break.

- **Porcelain:** The user-friendly interface commands (e.g., `git add`, `git commit`, `git push`).
- **Plumbing:** The low-level commands that do the heavy lifting under the hood (e.g., `git hash-object`, `git cat-file`).

> **Definition:** Git is a **Distributed Version Control System (DVCS)**. Unlike a central server model, every developer has a full copy of the repository and history.

## Git Under the Hood (Plumbing)

### How Git Stores Data
Git is a content-addressable filesystem. It stores **snapshots**, not differences.

- **The `.git` Directory:** All objects are stored in `.git/objects`.
- **Deduplication:** Git is efficient. Identical content is stored **only once**.
- **Empty Directories:** Git **does not** track empty directories. It only tracks files (content).

### The Object Model
There are three main types of objects:

1. **Blob:** Stores file content only. No filenames or timestamps.
   - *Structure:* `blob <size>\0<content>`
2. **Tree:** Represents a directory. Maps filenames to Blobs or other Trees.
3. **Commit:** A snapshot of the project.
   - *Contains:* Metadata (Author, Date, Message).
   - *Pointers:* Points to a Tree (root) and a Parent Commit.

## The Three Areas of Git
1. **Working Area:** The files currently on your disk that you are editing.
2. **Staging Area (Index):** The "holding zone" for files about to be committed.
3. **Repository:** The committed history stored in the `.git` directory.

## Key Commands & Workflows

### Advanced Staging
- **`git add -p`:** Stages commits in chunks (interactive mode).
- *Why:* Allows you to split one file's changes into multiple atomic commits.

### Saving Work
- **`git stash`:** Saves uncommitted work (staged and unstaged) to a stack and reverts the working directory to the last commit.

- # Git Stash: The No-Nonsense Guide

**The Brutal Truth:** Most developers use `git stash` as a crutch because they are afraid to commit incomplete work. While it is an essential tool for context switching, if you have a stack of 20 stashes, you are doing it wrong.

This guide explains how to use it, when to use it, and how to avoid shooting yourself in the foot.

---

## 1. The Analogy: "The Panic Drawer"

Imagine you are a mechanic fixing an engine. Parts are everywhere. Suddenly, your boss yells that you need to fix a flat tire **NOW**.

You cannot fix a tire on a workbench covered in engine parts.
* **The Bad Way:** Work around the mess, lose screws, mix up tools.
* **The Git Stash Way:** Sweep everything currently on the bench into a **temporary drawer**. The bench is clean. You fix the tire. You open the drawer and dump the engine parts back exactly where they were.

**Technical Definition:** `git stash` takes your uncommitted changes (staged and unstaged), saves them on a stack, and reverts your working directory to the last clean commit (`HEAD`).

---

## 2. The Critical Commands

Do not memorize everything. You only need these to survive.

### Stashing (Hiding the mess)
When you need to switch branches but your work is messy:

# Git Internals: References, HEAD, and Tags

**Stop treating Git like magic.**

This document demystifies Git references ("refs"). If you do not understand these concepts, you are not actually using Git; you are just memorizing commands and living in fear of merge conflicts.

## 1. The Core Reality
At its heart, Git is dumb. It is a simple **Key-Value Store**.

* **Keys:** SHA-1 Hashes (e.g., `a1b2c3d...`).
* **Values:** Compressed files (Commits, Trees, Blobs).

A **Reference (ref)** is not magic. It is a simple text file located in `.git/refs` that contains a specific hash. It is a user-friendly pointer to a scary-looking hash.

---

## 2. The Analogy: The Infinite Encyclopedia
To understand Refs, visualize an endless Encyclopedia.

* **The Commit:** A permanently **printed page**. Once printed, you cannot change the ink (Immutable). Every page has a unique ID (Hash) and references the previous page number.
* **The Graph:** The history of all pages linked together.

### The Players

| Concept | The Analogy | Mutable? | Definition |
| :--- | :--- | :--- | :--- |
| **Branch** | **A Sticky Note** | **Yes** | A pointer to the *latest* commit in a line of work. It moves automatically when you write a new page. |
| **HEAD** | **Your Eyes / Headlamp** | **Yes** | A pointer to *what you are currently looking at*. It usually points to a Branch, but can point to a Commit. |
| **Tag** | **Laminated Bookmark** | **No** | A permanent, immovable name for a specific commit. It does not move when new work is added. |

---

## 3. Deep Dive: HEAD (The "You Are Here" Marker)
`HEAD` is the most critical concept for daily work. It is the pointer that determines where your working directory is based.

### State A: Attached HEAD (Normal)
* **Analogy:** Your eyes are looking at the **Sticky Note** (Branch).
* **Technical:** The `.git/HEAD` file contains `ref: refs/heads/main`.
* **Behavior:** If you commit, the Sticky Note moves to the new page, and your eyes follow the Sticky Note.

### State B: Detached HEAD (The Danger Zone)
* **Analogy:** You took your eyes off the Sticky Note and are staring directly at a specific **Page Number** (Hash).
* **Technical:** The `.git/HEAD` file contains a raw hash `a1b2c3...`.
* **Behavior:**
    * If you commit now, you print a new page.
    * **BUT** there is no Sticky Note to mark it.
    * If you look away (`checkout` elsewhere), that new page is lost in the library and eventually destroyed by the janitor (Garbage Collection).

---

## 4. Deep Dive: Tags (The Anchors)
Branches answer: *"Where is the latest work?"*
Tags answer: *"Exactly what code did we give to the customer?"*

* **Branches are for coding.** They change constantly.
* **Tags are for releasing.** They freeze history.

---

## 5. Practical Application: Why This Matters Daily

You don't need to be a Git architect to code, but you need to understand Refs to fix mistakes and deploy safely.

### A. Using HEAD to "Undo" (The Time Machine)
When you commit a mistake (typo, wrong file), you don't need to make a "fix typo" commit. You manipulate `HEAD`.

* **Command:** `git reset --soft HEAD~1`
* **Logic:** "Move the Pointer (and Branch) back one step (`~1`), but leave my files alone (`--soft`)."
* **Result:** You get a do-over on your last commit.

### B. Using HEAD for Debugging (Isolation)
The app is broken. You need to know when it broke.
* **Command:** `git checkout HEAD~1` (and repeat)
* **Logic:** You detach `HEAD` to travel back in time. You check if the bug exists in the past.
* **Result:** You isolate the exact commit that caused the crash.

### C. Using Tags for Sanity (Production Safety)
Never deploy "main" directly if you can help it. Main changes too fast.
* **Strategy:** When you deploy, `git tag v1.0`.
* **Crisis Management:** If Production breaks on Friday, but `main` has unstable code from Wednesday, you cannot use `main` to fix it.
    * **Fix:** `git checkout v1.0`. You are now exactly where Production is. Create a hotfix branch from here.

---

## 6. Prove It (The Lab)
Don't trust the GUI. Run these commands in your terminal to see the raw plumbing.

1.  **See the Sticky Note:**
    ```bash
    cat .git/refs/heads/main
    # Output: A long hash (e.g., 4a3b2c...)
    ```
2.  **See Your Eyes (HEAD):**
    ```bash
    cat .git/HEAD
    # Output: ref: refs/heads/main
    ```
3.  **Detach HEAD:**
    ```bash
    git checkout [hash-from-step-1]
    cat .git/HEAD
    # Output: The raw hash itself. You are now in Detached HEAD.
    ```

```bash
# Standard stash (ignores new/untracked files)
git stash

# The BETTER stash (includes new files)
git stash -u

## 3. Practical Application: HEAD in Daily Work
You use `HEAD` to traverse time and undo mistakes.
```

| Scenario | Command | Logic (The "Why") |
| :--- | :--- | :--- |
| **The "Undo"** | `git reset --soft HEAD~1` | "Move the Pointer (and Sticky Note) back one step, but leave the files on my desk." (Fixes a bad commit). |
| **The "Time Travel"** | `git checkout HEAD~1` | "Detach HEAD to look at the previous page." (Isolates when a bug appeared). |
| **The "Alt-Tab"** | `git checkout -` | "Move HEAD back to the *previous* reference I was looking at." (Fast context switching). |
| **The "Sanity Check"** | `git diff HEAD` | "Compare the dirty files on my desk against what HEAD is currently pointing to." |

---

## 4. Merging vs. Rebasing (The Integrators)
Both commands combine work, but they change history differently.

### Option A: The Merge (`git merge`)
**The Safe, Honest Approach.**
* **Analogy:** You write a **"Summary Page"** that ties two chapters together.
* **Mechanism:** Git creates a new **Merge Commit** with *two* parents.
* **Pros:** Non-destructive. It preserves the exact history of when commits happened.
* **Cons:** "Pollutes" the history with merge commits. The graph can look messy.

### Option B: The Rebase (`git rebase`)
**The Clean, Destructive Approach.**
* **Analogy:** You rip your pages out of the book, burn them, and **re-write** them starting from the latest page of the main branch.
* **Mechanism:** Git **copies** your commits and applies them one by one on top of the target branch.
    * **CRITICAL:** The new commits have **NEW HASHES**. They are effectively new pages. The old pages are abandoned.
* **Pros:** Linear history. No merge commits. It looks clean, like a straight line.
* **Cons:** **Dangerous.** You are rewriting history.

> **⚠️ The Golden Rule:** NEVER rebase a public branch (a branch others are working on). You will burn pages that your teammates are currently holding, causing massive conflicts.

---

## 5. Prove It (The Lab)
Don't trust the GUI. Run these commands in your terminal to see the raw plumbing used in the *Advanced Git* slides.

```bash
# 1. See the Sticky Note (Branch Ref)
cat .git/refs/heads/main
# Output: A long hash (e.g., 4a3b2c...) -> This is the commit ID.

# 2. See Your Eyes (HEAD)
cat .git/HEAD
# Output: ref: refs/heads/main -> HEAD points to the branch name.

# 3. Detach HEAD (Look at a commit directly)
git checkout [hash-from-step-1]
cat .git/HEAD
# Output: [hash] -> HEAD now points directly to a commit. You are "detached".
```

## The Kitchen Analogy: Git Flow

- **Idea**: To master Git, stop thinking about files and start thinking like a chef.

## Core Places in Git (Kitchen Map)

- **Working Directory (The Cutting Board)**: Where you actively edit files; messy, unfinished, “raw ingredients.”
- **Staging Area / Index (The Prep Tray)**: Where you choose exactly what will go into the next commit; `git add` loads the tray.
- **Commit (The Finished Dish)**: A saved snapshot with a unique identity (the hash); permanent in history.
- **Git Log (The Chef’s Journal)**: The chronological record of commits; if it’s not recorded, it didn’t happen.
- **Stash (The Refrigerator)**: Temporary storage for unfinished work so you can switch tasks without committing.

## The Starting Point: Git Log

- **Truth**: `git log` is the source of truth for what actually happened in the repository.
- **The Hash**: Every commit is identified by a unique SHA-1 hash (a fingerprint of the repo state).
- **Author vs. Committer**:
  - **Author**: Who wrote the code/changes.
  - **Committer**: Who actually committed/applied it to the repository (can differ).

## Staging & Stashing

- **Staging**: Use `git add` intentionally; avoid blindly running `git add .` and mixing unrelated changes in one commit.
- **Stashing**: Use `git stash` when interrupted; it clears your board without forcing a “garbage commit” just to save progress.

## References: Bookmarks in Time

- **HEAD**: The “you are here” pointer; usually points to the latest commit on your current branch.
- **Branches**: Not folders—just lightweight, movable pointers to a commit.
- **Tags**: Permanent labels for specific commits (e.g., `v1.0`) that don’t move.

## Merging & ReReRe

- **Merging**: Bringing two different timelines together.
- **ReReRe (Reuse Recorded Resolution)**: If enabled, Git records how you resolved a conflict and can reuse that resolution when the same conflict appears again.

## History, Diffs & Fixing Mistakes

- **Diffs**: The “before and after” view of your code.
- **Reset**: Moving the pointer backward.
  - **`--soft`**: Moves the pointer but leaves your work staged (on the prep tray).
  - **`--hard`**: Discards work in your working directory; the nuclear option.
- **Revert**: Creates a new commit that does the opposite of a previous one; safer for undoing changes on shared branches.

## Rebasing vs. Amending

- **Amending**: `git commit --amend` fixes the very last commit (message and/or contents)—perfect when you forget the “salt.”
- **Rebasing**: Rewrites history by replaying commits onto a new base; creates a clean, linear history but is risky on public/shared branches.

## Advanced Hunting: Bisect & Grep

- **Grep**: Searching the repo (and optionally history) for a string, symbol, or function.
- **Bisect**: The “Who broke the oven?” tool; uses binary search to find the commit that introduced a bug by walking from “good” to “bad.”

## Hooks: The Kitchen Guards

- **Hooks**: Scripts that run on specific events.
  - **Pre-commit**: Runs before a commit finalizes; block dirty code or failed tests.
  - **Post-receive**: Runs after code is pushed to a server; trigger deployments/automation.

## Pro-Tip: The Reflog

- **Reflog**: If you think you deleted a branch or used `reset --hard` by mistake, run `git reflog`.
- **Why it helps**: Git keeps a local record of where `HEAD` and refs pointed recently; work is rarely truly gone until garbage collection prunes it.
