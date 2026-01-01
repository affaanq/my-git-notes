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
