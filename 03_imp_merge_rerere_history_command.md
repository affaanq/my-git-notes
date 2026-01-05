## ✍️ History Correction (The "Oops" Fixes)
Use these when you make a mistake. **Warning:** If you have already pushed, these require `--force`.

| Issue | Local Fix | Remote Fix (Danger) |
| :--- | :--- | :--- |
| **Typo in last commit** | `git commit --amend -m "New"` | `git push origin <branch> --force` |
| **Forgot a file** | `git add <file>` then `git commit --amend` | `git push origin <branch> --force` |
| **Reword old commit** | `git rebase -i HEAD~n` | `git push origin <branch> --force` |

---

## 🧠 Merging & Rerere (Reuse Recorded Resolution)
Automate your merge conflict resolutions.

| Feature | Command | Definition |
| :--- | :--- | :--- |
| **Enable Rerere** | `git config --global rerere.enabled true` | Tells Git to remember your fixes. |
| **Auto-Stage Fixes** | `git config --global rerere.autoupdate true` | Git automatically `adds` the fix. |
| **Check Status** | `git rerere status` | Sees what Git is currently recording. |
| **Forget Fix** | `git rerere forget <file>` | Deletes a bad recorded resolution. |
| **Abort Merge** | `git merge --abort` | The "Emergency Exit" for bad merges. |
| **Squash Merge** | `git merge --squash <branch>` | Keeps history clean by merging as one. |

---

## ⚠️ Brutal Best Practices
1. **Never Force Push to Main:** If you're on a team, this is a fireable offense. Use Pull Requests.
2. **Stashes are not Backups:** They are local. If your laptop dies, the stash dies.
3. **The 5-Stash Rule:** If you have more than 5 stashes, you are hoarding. Commit to a temporary branch instead.
4. **Clean your Rerere:** Run `git rerere gc` occasionally to clear out ancient resolutions.