The Git Architecture Map

To understand these concepts, remember the **4 layers** of data:

1. **Original Upstream** (The official cloud repo)
2. **Your Fork** (Your personal cloud repo)
3. **Local Repo** (Hidden `.git` folder on your PC)
4. **Working Directory** (The project files you edit on your PC) 

When and Who Uses Fetch, Merge, and Conflict?

5. FETCH (Download Only)

- **Who uses it:** **You** (the developer).
- **Which step:** Right before or during **Step 3 (Edit)**.
- **Why:** If the original project owner updates their code while you are working, your local computer doesn't know. You run `git fetch upstream` to download their new changes into your **Local Repo** without touching your active code. It is a safe "preview" step.

5. MERGE (Combine Files)

- **Who uses it:** **Both you and the Original Owner**.
- **Which step:**
    - **You (Step 3 - Edit):** After fetching the original owner's updates, you run `git merge upstream/main` to combine their new updates into your local working files.
    - **Original Owner (Step 5 - PR):** When you submit your Pull Request, the original owner clicks the "Merge Pull Request" button on GitHub to pull your code into the official project.

3. CONFLICT (The Roadblock)

- **Who handles it:** **You** (99% of the time).
- **Which step:** Occurs during a **Merge** (either in **Step 3** or **Step 5**).
- **Why:** A conflict happens when two people edit the exact same line of the same file. Git gets confused and stops.
    - **If it happens to you in Step 3:** You must open the file, delete the conflict markers (`<<<<<<<` and `>>>>>>>`), choose which code to keep, and commit the fix.
    - **If it happens in Step 5:** GitHub will block the PR and say _"This branch has conflicts that must be resolved."_ You must fix them locally on your PC, commit, and push again before the owner can accept your PR. 
