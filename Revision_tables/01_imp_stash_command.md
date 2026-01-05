# Git Stash
Stashing is a temporary storage area. It is NOT a substitute for proper branching or frequent commits. Use this guide to move between tasks without leaving a mess behind.
---

## 1. The Standard Workflow
Follow these steps to safely move your work-in-progress to the side and bring it back later.

| Step | Action | Command | Purpose |
| :--- | :--- | :--- | :--- |
| **1** | **Save** | `git stash save "your_message"` | Labels your changes so you don't have to guess what's inside. |
| **2** | **Verify** | `git stash list` | Confirms the stash exists in the stack. |
| **3** | **Switch** | `git checkout <other_branch>` | Move to the new task/bug fix. |
| **4** | **Return** | `git checkout <original_branch>` | Move back to the starting point. |
| **5** | **Inspect** | `git stash show -p stash@{0}` | **Critical Step:** Review the diff before you apply it. |
| **6** | **Apply** | `git stash pop` | Re-applies the latest changes and deletes the stash entry. |

---

## 2. Advanced Stash Commands
Use these flags to handle files that Git usually ignores by default.

| Feature | Command | Definition |
| :--- | :--- | :--- |
| **Untracked Files** | `git stash -u` | Includes new files that haven't been staged yet. |
| **Ignored Files** | `git stash -a` | Stashes everything, including files in `.gitignore`. |
| **Specific Hunks** | `git stash -p` | Interactively choose which parts of the code to stash. |
| **Stash to Branch** | `git stash branch <name> stash@{n}` | Creates a new branch and pops the stash into it (prevents conflicts). |
| **Keep Stash** | `git stash apply` | Applies changes but keeps the record in the stash list. |
| **Delete Single** | `git stash drop stash@{n}` | Deletes one specific stash from the history. |
| **Nuclear Option** | `git stash clear` | Deletes your entire stash history. No undo. |

---

## ⚠️Warnings

* **The "Untracked" Trap:** If you run a plain `git stash`, new files stay in your working directory. They will follow you to other branches and cause "ghost" bugs. **Always use `-u` if you added new files.**
* **Conflicts are Guaranteed:** If the branch moves forward while your code is stashed, `git stash pop` will cause merge conflicts. Fix them immediately; don't just re-stash.
* **Stashes are Local:** Stashes are NOT pushed to GitHub. If your laptop dies or you delete the folder, your stashed code is gone forever.
* **The 5-Stash Rule:** If you have more than 5 stashes, you are doing it wrong. Commit your work to a temporary branch instead.