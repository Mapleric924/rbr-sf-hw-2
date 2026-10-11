What is the difference between a working directory, the staging area, a commit, and a remote repository?
    Working dir: The actual files that you are editing, changes here are seen but are not recorded until the file is staged and committed
    Staging area: A temporary holding area where you declare which changes to be included in the next commit, can be multiple files
    Commit: A snapshot of a state of the repository, stored in the history with idetifiers(hash), author, commit message etc..
    Remote repository: A cloud copy of the repository, i.e. Github in our case. Essentially a hub for us to push commits and pull other people's changes from.

What do HEAD, main, and origin refer to in a normal repository?
    HEAD: usually a header that points to the newest commit on the branch that you are focused on
    main: the default branch, usually used for stable versions of codes that are released
    origin: the default name for the repository that the current repository is cloned from

Why should you pull or fetch recent main changes before starting new work?
    So that I can start from the latest code, instead of writing on outdated versions, to avoid overwriting other people's changes, creating troublesome merges, or even writing completely unusable code that was intended for outdated versions.

What is the difference between fetching and pulling?
    Fetch: Downloads new commits from the remote repo to the local repo, but does not modify the working directory yet. You'll need to inspect and merge manually
    Pull: Fetch + automatic merge (if no conflicts)

What is the difference between merging and rebasing? When is rebasing a shared branch dangerous?
    Merge: Adds a new commit to JOIN two branches while preserving both branches' commits & histories.
    Rebase: Adds the current branch TO the target branch, so that the current branch's first commit becomes the target branch's last commit's child.
    Rebase is dangerous because it rewrites the history of Git, including the hashes (identifiers) of commits. If someone else is working on the same repo, their pointers would point to old commits that no longer exists, and when they try to pull, their working dir will get completely messed up with duplicate commits (one with old hash one with new hash), merge conflicts, lost work, etc..

Why are pull requests useful even though they are not part of Git itself?
    Pull requests are basically asking repo admins to accept your proposed code changes to PULL your code into the repo.
    This allows:
        1. Collaboratos to propose changes that can be reviewed before its added to the codebase
        2. Documentation of the reasons for changes.
        3. Accountability/traceability if code is problematic.

In Task 4, Git created a new commit called Merge branch 'can'. Why was a new commit needed?
    main and can diverged, i.e. both had new commits after can was created from main, neither branches are a clear ancestor to the other. So the commit was needed to preserve both histories and combine their changes, instead of just pointing to a "newest" child.

In Task 5, both branches changed src/main.cpp. Why did can_id merge by itself but @version and loop() did not?
    only main changed can_id
    but both @version and items in the loop() were changed in BOTH BRANCHES, so Git couldn't choose which change to adopt.
    i.e. non-overlapping changes merge automatically, overlapping changes that happen on the same lines conflict and require human input in the merge editor

In Task 6, which workflow ran when you pushed to ci, and which did not? Why? What does the CI workflow protect the team from?
    On the push to ci, PlatformIO CI ran but not Doxygen GitHub Pages Deploy Action. This is because PlatformIO CI is triggered everytime we push, while Doxygen GitHub Pages Deploy Action is triggered only on pushes to main (according to the homework2 document), which did not happen until ci was merged into main.
    CI workflow protects us from pulling problematic code into the repo, which prevents bad code from breaking the repo and needing human troubleshooting to identify the error.

What does a .gitignore file do, and why does it not remove a file that was already committed? Why is .pio in .gitignore?
    It tells git to ignore certain files in the directory. However, it only affects untracked files, if a certain file is already being tracked, then adding that file to .gitignore will NOT remove it from git histories or its tracking.
    .pio is ignored because the generated files are large and does not carry significant information for human reading. adding those files to the repo only wastes space and creates conflicts (as the file may not have the same content each commit)