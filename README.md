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

```bash
# Standard stash (ignores new/untracked files)
git stash

# The BETTER stash (includes new files)
git stash -u
