<div align="center">
  <h1>Git & GitHub — Part 3 🌳</h1>
  <p><strong>Different branches. Shared progress. One working project.</strong></p>
  <p>A hands-on learning journal about branching, merging, conflicts, and the feature branch workflow.</p>

  <img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" alt="Git">
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
  <img src="https://img.shields.io/badge/Practice-Branch%20%26%20Merge-2563EB?style=for-the-badge" alt="Branch and merge practice">

  <p>
    <a href="#preview">Preview</a> ·
    <a href="#what-you-can-explore">Topics</a> ·
    <a href="#run-it-locally">Practice setup</a> ·
    <a href="#exercise-1-a-feature-and-an-urgent-fix">Branches</a> ·
    <a href="#exercise-2-create-and-resolve-a-conflict">Conflicts</a> ·
    <a href="#exercise-3-the-feature-branch-workflow">Pull requests</a> ·
    <a href="#when-git-says-wait-a-second">Troubleshooting</a>
  </p>
</div>

## Why I built this

Part 1 taught me to save versions. Part 2 taught me to share them with GitHub. Part 3 answers a team-development question: **how do we work on different changes without mixing unfinished work into the main project?**

Imagine improving the MOVE It navbar when someone discovers a broken contact link. I want to fix the link without publishing half a navbar. Branches let those tasks develop separately; merging brings finished work together.

This README follows the supplied final tutorial transcript by **Simon from SuperSimple.dev**. It covers branching, merging, merge conflicts, pull requests, code review, and local synchronization after a GitHub merge.

The exercises below adapt the lesson into copyable PowerShell examples with clear checkpoints. Extra troubleshooting commands are identified separately. This is a foundation for professional workflows, not a claim that one video covers every Git skill.

## Preview

The most important two lines:

```powershell
git checkout main
git merge feature-1 -m "Merge feature-1"
```

**Read it as: “Stand on `main`, then bring `feature-1` into it.”**

| Where I stand | What I merge | Which branch receives the result? |
| --- | --- | --- |
| `main` | `feature-1` | `main` |
| `feature-1` | `main` | `feature-1` |

The source branch is not automatically deleted or moved by the merge.

<!-- OPTIONAL SCREENSHOTS:
1. Add docs/branch-history.png for your completed branch graph.
2. Add docs/pull-request.png for your practice pull request.
3. Uncomment the matching image lines below after the files exist.
Avoid exposing private information in screenshots.
-->
<!-- ![Feature and bug fix histories joined by a merge](docs/branch-history.png) -->
<!-- ![Practice pull request with main as the base branch](docs/pull-request.png) -->

## What you can explore

| Topic | What it solves |
| --- | --- |
| Branches | Keep a feature's commits separate from another line of work |
| `HEAD` | Identify the branch or commit currently checked out |
| Local merging | Bring another branch's changes into the current branch |
| Conflicts | Decide the intended result when Git cannot combine edits automatically |
| Feature branches | Develop and publish a focused change |
| Pull requests | Propose changes for review before integration |
| GitHub merging | Integrate a reviewed branch on the remote |
| Synchronization | Bring that remote merge into local `main` |
| Conflict resolution during review | Update the feature branch with current `main`, resolve, and push |

### A life example: building with LEGO 🧱

Imagine a finished LEGO house. You want to experiment with a garage while your friend repairs its front door.

- **`main`** is the agreed starting house.
- **A feature branch** lets you develop the garage separately.
- **A bug-fix commit on `main`** repairs the door without adding the unfinished garage.
- **A merge** combines compatible work from both histories.
- **A conflict** happens when both people propose incompatible edits to the same part, and someone must choose the final design.

Git does not literally make a second folder for the garage. A branch is a lightweight reference to a commit; branches share existing history until new commits make their paths diverge.

## Built with

- **Git** — branches, commits, merges, and history.
- **GitHub** — remote branches, pull requests, and review.
- **PowerShell** — Windows practice commands.
- **VS Code or another text editor** — file editing and conflict resolution.
- **Markdown** — study notes and examples.

## Run it locally

### Before starting: give the experiments their own playground 🎠

Use a new practice folder, not an important project. Git should already be installed and your commit identity configured. GitHub authentication from Part 2 is needed for the remote exercises.

These examples use **`main`**. The video often uses `master`; substitute your actual branch name when practicing elsewhere. Branch names below contain hyphens, not spaces.

```powershell
New-Item -ItemType Directory -Path "$HOME/git-branching-practice"
cd "$HOME/git-branching-practice"
git init -b main
```

`-b main` is an explicit setup choice from earlier practice. The transcript uses `git init`.

If the folder already exists, enter it rather than recreating it. The `Set-Content` commands below intentionally replace the contents of disposable example files.

### Build three checkpoints: the tree needs a trunk 🌳

```powershell
"Version 1" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Version 1"

"Version 2" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Version 2"

"Version 3" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Version 3"

git log --all --graph --oneline
```

**Checkpoint:** three commits, with `HEAD -> main` on the newest one. `--oneline` is a readability addition to the video's graph command. Press `q` to exit the scrolling viewer if necessary.

## Exercise 1: a feature and an urgent fix

### Step 1 — Create a workspace for the feature 🛠️

```powershell
git branch feature-1
git checkout feature-1
git status
```

`git branch feature-1` creates a branch at the current commit. It **does not switch to it**. `git checkout feature-1` does the switching.

**Checkpoint:** `On branch feature-1`. New commits now advance this branch.

### Step 2 — Save the feature in small pieces 🧩

```powershell
"Feature: first piece" | Set-Content -Encoding utf8 feature.txt
git add feature.txt
git commit -m "Feature commit 1"

"Feature: second piece" | Add-Content -Encoding utf8 feature.txt
git add feature.txt
git commit -m "Feature commit 2"

git log --all --graph --oneline
```

**Checkpoint:** `feature-1` has two extra commits. `main` still points to Version 3. Committing unfinished work on a dedicated branch can be useful; it does not make that work ready to merge.

### Step 3 — An urgent fix arrives: pause the garage, repair the door 🚪

```powershell
git checkout main
"Practice bug fix" | Set-Content -Encoding utf8 bugfix.txt
git add bugfix.txt
git commit -m "Add bug fix"
```

When you return to `main`, tracked files match its version. `feature.txt` disappears from the working tree because `main` does not contain it yet. It is still saved in `feature-1`'s history.

This file is a stand-in for a bug fix, not a functional application repair. In a real project, reproduce and test the fix. The tutorial commits directly to the main branch here for teaching; a team may require a separate fix branch and review.

### Step 4 — Finish the feature 🏁

```powershell
git checkout feature-1
"Feature: complete" | Add-Content -Encoding utf8 feature.txt
git add feature.txt
git commit -m "Finish feature"
```

Your feature comes back when you return to its branch. The bug fix is not part of this branch yet: the two branches developed independently.

### Step 5 — Bring the completed feature into main 🤝

```powershell
git checkout main
git merge feature-1 -m "Merge feature-1"
git status
git log --all --graph --oneline
```

**Checkpoint:** `main` now contains both `bugfix.txt` and `feature.txt`, plus the existing `hello.txt`. Because both branches gained independent commits, this example creates a merge commit connecting their histories.

Different new files make this a straightforward automatic merge. Other histories may fast-forward instead: Git can move the destination branch forward without creating a new merge commit when it already lies on the source branch's history.

**Useful correction to the lesson's simplification:** not every successful merge creates a new commit, and not every change to the same file causes a conflict. Git uses a common ancestor to combine compatible changes.

## Exercise 2: create and resolve a conflict

### Step 1 — Prepare two competing versions of one line 🥊

Start from the completed `main` above:

```powershell
git checkout main
git branch conflict
git checkout conflict

"Version from conflict branch" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Change greeting on conflict branch"

git checkout main
"Version from main branch" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Change greeting on main"
```

Both branches changed the same original line differently. This deliberately creates a simple conflict rather than hoping one appears by accident.

### Step 2 — Ask Git to combine them: it asks a question back 🙋

```powershell
git merge conflict -m "Merge conflict branch"
git status
```

Expect a conflict message. The merge is paused, not finished. In the default conflict style, `hello.txt` contains something like:

```text
<<<<<<< HEAD
Version from main branch
=======
Version from conflict branch
>>>>>>> conflict
```

| Marker | Meaning in this merge |
| --- | --- |
| `<<<<<<< HEAD` | Starts the current branch's version: `main` |
| `=======` | Separates the alternatives |
| `>>>>>>> conflict` | Ends the incoming branch's version |

Other configured conflict styles may also show the common ancestor. In a normal merge, “current” means the branch you are on; it does not always mean `main`.

### Step 3 — Be the editor, not the referee with a blindfold ✏️

Open `hello.txt` in your editor. Choose the intended final text. You can keep one side, combine the ideas, or write a better result.

For this exercise, make the entire file:

```text
Hello! Both branches helped improve this project.
```

Remove all conflict markers and unwanted text, then save. VS Code's “Accept Current,” “Accept Incoming,” or merge editor can help, but **read the result** before accepting it.

For real code, preserving valid behavior matters more than simply making the markers disappear. Two pieces of code may merge cleanly and still behave incorrectly together.

### Step 4 — Tell Git the decision is ready ✅

```powershell
git add hello.txt
git status
git commit -m "Resolve greeting merge conflict"
git log --all --graph --oneline
```

**Checkpoint:** no unmerged paths remain and the merge is committed. If several files conflict, resolve and stage every one before committing.

The video's `git add .` works too, but staging the named file here makes the decision easier to review.

## Exercise 3: the feature branch workflow

### Step 1 — Give the finished starting point a GitHub home 🏠

Create an **empty** GitHub repository for this practice. Replace the owner and repository placeholders below. Use either your configured HTTPS or SSH authentication method from Part 2.

```powershell
git checkout main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push origin main
```

If `origin` already exists, inspect `git remote -v` rather than adding it again. Ensure GitHub's default branch is `main` for the comparisons below.

### Step 2 — Start a focused feature branch 🎯

```powershell
git branch new-feature
git checkout new-feature
"A feature ready for review" | Set-Content -Encoding utf8 new-feature.txt
git add new-feature.txt
git commit -m "Add new feature example"
git push origin new-feature
```

GitHub now has the main branch and a separate feature branch. Pushing a feature does not merge it into `main`.

### Step 3 — Open a pull request: invite another pair of eyes 👀

On GitHub, open a new pull request comparing:

| Setting | Select |
| --- | --- |
| **Base** | `main`: the destination |
| **Compare** | `new-feature`: your proposed changes |

Add a title and a short explanation of what changed, why, and how you checked it. Review the diff before creating the request.

**A pull request is a proposal for review and integration. It is not the `git pull` command, and opening it does not merge the branch.**

### Step 4 — Review, improve, repeat 🔍

The video demonstrates review comments in the **Files changed** and conversation views. A reviewer can identify bugs, suggest improvements, or ask questions.

If you need to revise the feature, stay on `new-feature`, edit and save the files, then:

```powershell
git add new-feature.txt
git commit -m "Improve feature after review"
git push origin new-feature
```

The existing pull request updates with the pushed branch. You normally do not create a new request for every review fix.

For a solo exercise, review your own diff and practice a revision. That teaches the mechanics without pretending an independent reviewer approved it.

### Step 5 — Merge after review and checks 🤝

Once required reviews and checks pass, merge the pull request on GitHub if you have permission. The video uses **Merge pull request → Confirm merge**, with the merge-commit method.

Repositories may offer squash or rebase methods instead, or enforce a particular policy. This exercise follows the video's merge-commit approach so its history is easy to compare.

Merging does not automatically deploy a website. Deployment depends on a separate hosting or automation setup.

### Step 6 — Bring the remote result back to your computer 📥

```powershell
git fetch origin
git log --all --graph --oneline
git checkout main
git pull origin main
git status
```

**Checkpoint:** your local `main` now contains the GitHub merge. Before pulling, the remote-tracking `origin/main` could be ahead while local `main` stayed unchanged.

Start with a clean working tree and no extra local main-branch commits for this exercise. Pull integrates into the current branch, so check out the intended branch first.

## Exercise 4: two pull requests, one shared line

### Step 1 — Simulate two developers from the same starting point 👥

After updating `main`, create two separate branches. Each changes the same line of `hello.txt`.

```powershell
git checkout main
git branch feature-3
git checkout feature-3
"Greeting from feature 3" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Update greeting in feature 3"
git push origin feature-3

git checkout main
git branch feature-4
git checkout feature-4
"Greeting from feature 4" | Set-Content -Encoding utf8 hello.txt
git add hello.txt
git commit -m "Update greeting in feature 4"
git push origin feature-4
```

**Why return to `main` before creating `feature-4`?** Otherwise, it would start from the current feature branch and inherit its commits. Branches start where you create them, not automatically at `main`.

The transcript uses `feature-1` and `feature-2` for its web-editor demonstration, then `feature-3` and `feature-4` for the local one. We use fresh `feature-3` and `feature-4` names here to avoid colliding with Exercise 1.

### Step 2 — Merge one request; watch the other need attention 🚦

Create a pull request for each branch with base `main`. Review and merge `feature-3` first. Leave `feature-4` open.

Now `main` contains Feature 3's greeting, while Feature 4 has a different edit to the same original line. The second request should report a conflict.

This can happen even when neither developer made a mistake. They simply worked on overlapping content.

### Step 3 — Pick a resolution route 🧭

These are **alternative** methods. Use one for the current conflict.

| Route | When it helps |
| --- | --- |
| GitHub web editor | A simple supported text conflict you can understand in the browser |
| Local editor | You want to run the app, test behavior, or use local editing tools |

**Web route shown in the video:** open the request's conflict editor, choose the intended final content, remove markers, mark each file resolved, and commit the resolution. Where this action is available, it updates the feature branch by merging the base branch into it. Not all conflict types are resolvable through GitHub's editor.

Resolving the conflict makes the request mergeable again; it does not mean the request is already merged or approved.

### Step 4 — Local route: bring main into the feature branch 🛠️

If you chose the local route, keep the web conflict unresolved and run:

```powershell
git checkout main
git pull origin main
git checkout feature-4
git merge main -m "Merge main into feature-4"
git status
```

**Direction matters:** we are updating `feature-4` with the latest `main`, so the merge belongs on `feature-4`. We are not bypassing review by merging the feature into local `main`.

The markers now show:

```text
<<<<<<< HEAD
Greeting from feature 4
=======
Greeting from feature 3
>>>>>>> main
```

`HEAD` means `feature-4` here. The incoming `main` contains the greeting added through Feature 3's merged request.

### Step 5 — Resolve, check, commit, and push the feature 📤

Choose the intended result in your editor. For this text exercise, use:

```text
Hello from our combined features!
```

Save, then:

```powershell
git add hello.txt
git commit -m "Resolve greeting conflict with main"
git push origin feature-4
```

For real software, run appropriate checks before committing and pushing. A successfully staged file does not prove the code works.

**Checkpoint:** GitHub's open request includes the resolution and should no longer show this conflict. Further pushes to `main` could introduce new changes, so mergeability is always assessed against its current state.

### Step 6 — Finish the review loop 🏁

Complete any required review and checks, merge Feature 4's request, then synchronize:

```powershell
git checkout main
git pull origin main
git log --all --graph --oneline
```

You have now practiced separate development, review, overlapping changes, conflict resolution, and final integration.

## Commands from the video

Names below are normalized with hyphens. The video's `master` examples correspond to `main` here.

| Command | Job |
| --- | --- |
| `git init` | Initialize a repository |
| `git status` | Inspect branch and change state |
| `git add .` | Stage eligible changes in the current folder and below |
| `git commit -m "Message"` | Save staged content with a message |
| `git log --all --graph` | Visualize history reachable from refs and HEAD |
| `git branch feature-1` | Create a branch at the current commit |
| `git checkout feature-1` | Check out that branch |
| `git checkout main` | Return to the main branch |
| `git merge feature-1` | Merge that branch into the current branch |
| `git merge feature-1 -m "Merge feature-1"` | Supply a message if a merge commit is created |
| `git merge conflict -m "Merge conflict"` | Attempt the lesson's deliberate conflicting merge |
| `git branch -D feature-1` | Force-delete a local branch; demonstrated to reuse an exercise name |
| `git remote add origin URL` | Configure the remote address |
| `git push origin main` | Publish the selected local branch |
| `git push origin new-feature` | Publish the review branch |
| `git push origin feature-1` / `feature-2` / `feature-3` / `feature-4` | Publish the respective practice branch |
| `git fetch` | Refresh history and references from the selected/default remote |
| `git pull origin main` | Fetch and integrate remote main into the current branch |
| `git merge main` | Bring local main into the current feature branch |

`URL` is a placeholder. In the combined push row, use one branch name per command, such as `git push origin feature-4`; do not type the slashes between alternatives.

Creating branches, committing, and merging can be entirely local. Pushing is the step that publishes the selected branch to the configured remote.

## When Git says “wait a second”

These practical additions help diagnose the lesson's workflow. They are not all commands demonstrated in the transcript.

### “I created a branch, but my commit went to main.”

Creating is not switching. Check `git status` before committing and use `git checkout BRANCH_NAME` to enter the intended branch. Do not try to repair already-published mistakes by blindly deleting branches or resetting history.

### “My new branch includes the other feature's commits.”

It started at your current commit. For independent work, return to updated `main` before creating the next branch. If the unwanted branch already contains useful work, inspect its graph before changing it.

### “My file disappeared when I changed branches!”

For committed tracked files, Git adjusts the working tree to the checked-out branch. Return to the branch containing the file to see it again. Uncommitted edits may carry across if compatible, or block checkout if they would be overwritten; they are not reliably isolated just by changing branches.

### “fatal: a branch named … already exists”

Use a new descriptive name or reuse the existing branch intentionally. Do not force-delete valuable work just to clear an exercise error.

Extra cleanup commands:

```powershell
# After returning to main and confirming integration
git branch -d finished-feature
```

Lowercase `-d` applies a merge-safety check. Capital `-D`, shown in the video, overrides that check and can leave unique commits without that branch reference. Neither deletes a GitHub branch.

For squash/rebase integration, Git may not consider the original commits merged even though their changes were integrated. Inspect the history before overriding deletion checks.

### “Already up to date.”

The source history is already included in the current branch. Confirm both branch names and the graph. If you expected new code, it might be uncommitted or in another branch or repository.

### “The merge opened an editor.”

Git needs a merge-commit message. The exercises use `-m` to provide it directly. If an editor already opened, save the message and close it according to that editor's controls; `q` only exits the history pager, not every editor.

### “Automatic merge failed; fix conflicts and then commit the result.”

This is a paused merge. Run `git status`, resolve every conflicted file, remove markers, check the final content, stage the resolved files, and commit.

To cancel the current merge instead, this additional command is useful:

```powershell
git merge --abort
```

It attempts to restore the pre-merge state. Starting with a clean working tree makes this safer; abort may not reconstruct unrelated edits perfectly if you began with uncommitted changes.

### “Your local changes would be overwritten.”

Git is protecting unstaged or staged work. Review it and commit work you want to keep before switching or merging. Stashing is another skill to learn later. Discard only edits you intentionally want to lose.

### “I resolved it locally, but GitHub still shows a conflict.”

Did you save every file, stage the resolution, finish the merge commit, and **push the feature branch**? Also verify the request's base/compare branches. A local commit cannot update GitHub until it is pushed.

### “The pull request merged, but my local files look old.”

The merge happened remotely. Return to local `main` and pull from `origin main`. Fetch alone updates remote knowledge, not your currently checked-out files.

### “I selected Accept Both. Is the conflict solved?”

Only if the result is correct. Combining both alternatives can duplicate logic, break HTML structure, or create invalid code. Read and test the result; Git cannot decide the intended product behavior.

### “Why can't I merge the pull request?”

Conflicts are one possible cause. Required reviews, checks, permissions, or branch protection can also block it. Inspect the reported reason and follow the repository's rules rather than forcing past them.

## What changed during the exercises

| Stage | Result |
| --- | --- |
| Three baseline commits | Created a simple starting history |
| Feature branch | Saved three feature checkpoints separately |
| Main bug fix | Added a fix without the unfinished feature |
| Local merge | Brought feature and fix histories together |
| Deliberate conflict | Created incompatible edits to the same starting line |
| Resolution commit | Recorded the chosen final content and completed the merge |
| First pull request | Practiced publishing a feature, review, and GitHub integration |
| Remote synchronization | Updated local main after the online merge |
| Competing feature requests | Reproduced a conflict caused by another merged feature |
| Main merged into feature | Resolved the conflict on the feature branch before final review |

These examples are an adapted practice path, not a claim that the repository already contains every checkpoint. Its actual `git log` is the record of what was committed.

## What I learned

- A **branch is a reference to a commit**, not a second physical project folder.
- Creating a branch and checking it out are separate actions.
- Commits belong to the line of development I am currently advancing.
- **Merge direction matters:** the current branch receives the result.
- Some merges create a merge commit; others can fast-forward.
- Conflicts require a decision about final content, not just deleting warning text.
- A clean automatic merge still needs appropriate testing.
- A pull request supports review; it is different from `git pull`.
- Resolving a pull-request conflict does not automatically complete review or merge it.
- Remote integration must be synchronized back to the local repository.

### My feature workflow card 🎴

1. Update the starting branch.
2. Create and check out a focused feature branch.
3. Edit, review, test, stage, and commit.
4. Push the feature branch.
5. Open a pull request into the intended base branch.
6. Respond to review feedback with more feature commits.
7. Resolve conflicts against updated main when necessary.
8. Merge after required reviews and checks.
9. Update local main and clean up finished branches deliberately.

## Next improvements

- [ ] Repeat the workflow on a small web-development feature.
- [ ] Practice more conflict types, such as rename/delete conflicts.
- [ ] Learn stashing, rebasing, and recovery tools separately.
- [ ] Explore required reviews and automated checks.

The transcript explicitly leaves rebase for later learning. These notes keep that boundary rather than adding an unrelated advanced tutorial.

## Feedback and contributions

Corrections and clearer examples are welcome. Include the command, current branch, terminal type, and error message when asking for help. Never share private keys, tokens, or passphrases.

## Credits and license

Learning journal by **[Elmahdi Benabbou](https://github.com/Elmahdi25)**, based on the supplied Part 3 transcript by **Simon from [SuperSimple.dev](https://supersimple.dev)**.

The explanations and practice files are adapted into personal study notes.

## Support the project

If the exercises helped the branch graph feel less like spaghetti, a star or useful correction is appreciated. 🍝 Explore more of my learning projects on **[my GitHub profile](https://github.com/Elmahdi25)**.

---

<div align="center">
  <strong>Branch with a purpose. Merge with understanding. Keep learning. 🌱</strong>
</div>
