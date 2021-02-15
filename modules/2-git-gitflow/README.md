
# Git and GitFlow: Source Control and Collaboration:

## 1. Git Source Control Management:

Git is a Distributed Version Control System for tracking, versioning, and collaborating on software projects (and more). It's the VCS of choice for InstaDeep and most software teams.

Learn more with the "Pro Git book": https://git-scm.com/book/en/v2

### a. Git Concepts:

- **Repository**: a collection of a project's files (and directories), along with each file's revision history
- **Commit**: a snapshot of the repository, identified by a 40-character SHA-1 hash and described by a message written by the contributor who took the snapshot, having exactly one parent
- **Branch**: a line of development, organizing the chains of commits
- **Remote**: a clone of the repository that exists on a centralized external server (e.g. GitHub, Gitlab, BitBucket), which is synchronized with the local versions of the repository on the different contributors' workstations
- **Merge**: a special type of commit with more than one parent, joining multiple branches into one

***Additional points to explore:***
- Git stores snapshots, not differences
- Git integrity is based on checksums
- Git almost always adds data to perform any type of operation
- Git has two types of files: untracked and tracked (which can be in one of three primary states: modified, staged, and committed)
- Git has three primary sections where files in the three aforementioned states reside: the working directory, the staging area, and the `.git` directory
- Git supports multiple authentication methods such as: SSH key, username/password, and OAuth
- Git uses tags to mark specific commits such as release points (e.g. `v0.1`)
- Git internally works like a UNIX filesystem, storing the repository contents using two object types:
    - `blob`: loosely speaking, corresponds to inodes (used to store file contents)
    - `tree`: corresponds to directories and contains blobs and subtrees (corresponding files and subdirectories), so commits simply point to a tree
    In terms of storage, `blob` and `tree` objects are never duplicated when multiple commits (snapshots) exist unless the contents of the corresponding files and directories since they're identified by hashes of their contents.
- Git uses a special file called `.gitignore` containing a list of files/directories to ignore in the change-tracking process
- Git supports hiding uncommitted changes to a local `stash` while moving between commits

### b. Git Commands:

- `git init`: initializes a new Git repository (creating a `.git` hidden directory) and starts tracking the contents of the directory where the command was executed in
- `git clone`: creates a local clone of the repository from a remote
- `git add`: adds modified files/directories to the staging area (tracked files only)
- `git status`: shows the status changes of files/directories compared to the most recent commit: untracked, modified, or staged
- `git commit`: takes and saves a snapshot of the repository, moving the changes from the staging area to the `.git` directory
- `git push`: uploads local changes (e.g. commits, tags) to the remote
- `git checkout`: moves the repository to an existing commit using its hash, an existing branch using its name, a new branch using the `-b` command option, a tag, etc ...
- `git pull`: downloads remote changes into the local repository
- `git merge`: merge one or more branches (only up to a specific commit, if needed) into another branch

***Additional commands to explore:***
- `git log`
- `git tag`
- `git branch`
- `git stash`
- `git rm`
- `git cherry-pick`
- `git rebase`



## 2. GitFlow Collaboration:

GitFlow is a popular, successful branching model that organizes and optimizes the collaboration process using git, especially for projects where the deliverables are versioned.

Find the original article presenting and discussing the model here: https://nvie.com/posts/a-successful-git-branching-model/

#### a. GitFlow Concepts:

- **Central repo**: the single source of "truth", often called `origin`, from which contributor pull others' changes, and to which they push theirs 
- **Main branches**: two git branches with an infinite lifetime: 
    - **Master branch**: `master` is the git branch that represents the state of the system in live production environments, sometimes also called `main` 
    - **Develop branch**: `develop` is the git branch containing the most recent state of the system to be deployed to production in the future
- **Supporting branches**: git branches with finite lifetime, of which exist three subtypes:
    - **Feature branches**: git branches created for the purpose of creating or updating a feature, usually named like `feature-*`
    - **Release branches**: git branches representing upcoming shipping and/or deployment to a production environment, usually named like `release-{version-number}`
    - **Hotfix branches**: git branches created to fix critical bugs detected in a live production environment, usually named like `hotfix-{version}`


### b. GitFlow Workflow:

The general workflow can be inferred from the following rules:

- Main branches exist during all of the project's lifetime 
- Feature branches are created from `develop` or from other feature branches and are merged back to `develop` once they're confirmed to be a part of an upcoming release
- Release branches are created from `develop` with names reflecting a release version number and are merged to `master` once they're tested and confirmed to be production-ready
- Release branches can only receive bug fix or metadata-change commits, in which case, they are merged back to `develop` eventually, often before being merged to `master`
- Hotfix branches are created from `master` with names reflecting an incremented release version number, are merged back into both it and `develop`, and are often matked using a git tag

Note: Versioning was not addressed in the original GitFlow article. Consider building the version numbers used in release or hotfix branches loosely based on the "Semantic Versioning" scheme: `X.Y.Z`, where:
    - `Z` is incremented when a bug is fixed
    - `Y` is incremented when new features are introduced
    - `X` is incremented when major new features or breaking changes are introduced

***Additional GitFlow ideas to explore and ponder-upon:***
- Subteams
- Feature branches often not being pushed to `origin`
- Merging without fast forward (`git merge --no-ff`) 
