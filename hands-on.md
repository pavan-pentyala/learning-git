
## Hands-On Lab — The Git Battleground Simulation

This lab serves as a comprehensive, offline flight simulator to replicate advanced team dynamics, branch divergence, merge conflicts, code auditing, and emergency production rollbacks—all on a single local machine.

---

### Phase 1: Local Setup & Amending History

1. **Initialize the Repository:**
   ```bash
   git init
   ```
2.  **Establish the Base Code:** Created `app.js` with a single baseline console log.
    
3.  **The Intentional Typo & Local Fix:** Committed the file with a typo (`"Inital commit"`), then used the golden rule of local fixing to overwrite the mistake seamlessly:
    
    ```bash
    git add app.js
    git commit -m "Inital commit"
    git commit --amend -m "Initial Commit"
    ```
    

### Phase 2: The Multiverse (The Split Timeline)

Simulated a scenario where a developer builds a feature locally while a teammate pushes a conflicting update directly to `main`.

1.  **Branching Off:** Created a local feature branch and added a dark-mode theme variable.
    
    ```bash
    git checkout -b feature-theme
    # Added: const theme = "dark-mode";
    git add app.js
    git commit -m "Implemented dark mode theme"
    ```
    
2.  **The Teammate's Play (The Main Shift):** Jumped back to `main` (where the dark-mode line vanishes instantly) and added a conflicting light-mode variable on the *exact same line*.
    
    ```bash
    git switch main
    # Added: const theme = "light-mode";
    git add app.js
    git commit -m "Simulating teammate update file edited app.js"
    ```
    

### Phase 3: The Divergence Crisis & The Collision

1.  **The Divergence Warning:** Upon switching branches, Git flagged a divergence error:
    
    > *Your branch and 'origin/main' have diverged, and have 1 and 1 different commits each.*
    
    -   **The Lesson:** This happens because amending a commit *after* it has been pushed completely rewrites the local commit IDs. The laptop and cloud become parallel, conflicting universes.
        
    -   **The Rule:** Never fix divergence by forcing a push (`git push --force`) on shared branches as it erases teammate data. Always **Pull &rarr; Resolve Locally &rarr; Push**.
        
2.  **The Vim Portal (Three-Way Cloud Merge):** Running `git pull origin main` triggered a mandatory Three-Way Merge to align the local/cloud tracking pointers. This forced open the terminal text editor (**Vim**).
    
        
3.  **Triggering the Feature Conflict:** With `main` aligned, the feature branch was pulled in, causing an immediate structural collision on line 2:
    
    
    ```bash
    git merge feature-theme
    # CONFLICT (content): Merge conflict in app.js
    ```
    
4.  **Surgical Conflict Resolution:** \* Opened `app.js` inside VS Code to view the native, color-coded conflict block UI.
    
    -   Selected **"Accept Both Changes"**.
        
    -   Adjusted the variables to prevent a JavaScript error (`themeLight` and `themeDark`).
        
    -   Cleaned out the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) and sealed the merge:
        
        
        ```bash
        git add app.js
        git commit -m "Resolved merge conflict by keeping both and renaming variables"
        ```
        

### Phase 4: Diagnostic Post-Mortem & Emergency Rollback

1.  **The Reachability Law of `git log`:** Running `git log --oneline` showed commits from *both* branches. Once a feature branch is merged into `main`, Git adopts its entire past timeline into `main`'s history list.
    
2.  **Line Auditing with `git blame`:** Running `git blame app.js` exposed the exact commit fingerprint behind every single line of code.
    
    -   **Exporting Large Audits:** To audit massive files without flooding the terminal console, redirect the output directly into a document:
        
        
        ```bash
        git blame app.js > blame-report.txt
        ```
        
3.  **The Advanced Emergency Rollback (`git revert -m 1`):** Simulated a production emergency requiring us to undo our entire merge resolution.
    
    -   Running a standard revert on a merge commit throws an error because Git does not know which side of the family tree to follow.
        
    -   The **`-m 1`** flag instructs Git to cleanly strip out the incoming feature code changes while maintaining the historical integrity of the primary timeline (Parent 1 / `main`).
        
    
    Bash
    
    ```
    git revert 0691fb5 -m 1
    ```
    

### 🔴 The Immutable Law of the Git Ledger

After running the revert, `app.js` successfully snapped back to its safe, pre-merged state (`light-mode` only). However, checking `git log` revealed that **no past commits were deleted**.

Git behaves like a high-security banking ledger. It never erases past events or utilizes "white-out."