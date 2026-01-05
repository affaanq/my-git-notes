# Git Pointers: HEAD vs. MAIN

### 📍 HEAD (The "You Are Here" Pointer)
`HEAD` is a reference to the specific commit you are currently working on. 
- **View current HEAD:** `git rev-parse HEAD`
- **Move HEAD (Temporary):** `git checkout <commit-hash>` (Note: This detaches HEAD)
- **Point HEAD back to safety:** `git checkout main`

### 🌿 main (The Branch Pointer)
`main` is a pointer to the tip of your default branch. It is a "moving target" that advances with every commit.
- **View main's location:** `git show-ref --heads main`
- **Force main to a specific commit:** `git reset --hard <hash>`

### ⚠️ Warning: Detached HEAD State
If you move `HEAD` away from a branch pointer, any new commits you make will be **orphaned**. They won't belong to any branch and will eventually be deleted by Git's garbage collection. 

**If you find yourself detached, fix it immediately:**
`git checkout -b fix-branch-name`