[Link to commands](#commands)

# Background

Frankly, I'm not a fan of Git. It is not just that it's solving a hard problem; I believe it is *needlessly* complicated. It has caused me more grief than arguably any other technology. However, Git is not going away, probably in my lifetime. I've got to to learn to stop worrying and love the git. The below is the minimum viable knowledge you have to memorize to say you "know git" to a medium level.

Most Git reference materials consist of one of:
- Very watered-down obvious fluff
- Super-dense documentation that is impenetrable to all except command line prodigies 
- Conventions (in effect, advice)
- Obscure details or actions that you don't actually need to worry about

And another problem is the git aficionados are really exacting about their use of terminology, rejecting "technically wrong" but much more understandable descriptions in favor of machinespeak explanations that only make sense if you already know enough about the issue that you probably don't have the problem in question in the first place.

The below reference materials are mostly for my own use. But if you find a mistake, please let me know.

**Tip**: Don't want to learn the below commands? Download Github desktop. Seriously. Don't worry, it doesn't make you less of a programmer.

When I put something in angle brackets, like `<this>`, change the value, and do not put the brackets. 

# Model

There are local repos, and there's the remote repo, usually on Github. Each **repo** is made up of branches. A **branch** is a stack (CS data structure) of commits. Some branches share a common-ancestor commit, so the repo is a **tree** (CS data structure). 

A **commit** is composed of:
- Your code (for new or changed code) 
- Pointers to the previous commit (for places in code with no changes between the two)
- Metadata

Finally, there is the **workspace**: the place where you code in, and by doing so change directly. It's what you see when you look at your code editor/IDE. The workspace is not really part of your repo. When you switch branches, the workspace contents update to reflect the branch you are on. However, you only have one workspace per repo copy, regardless of how many branches there are. You make changes part of your repo by committing, as depicted:

![image](https://user-images.githubusercontent.com/37461272/206489424-84f1d260-8947-462b-b3e5-6813bc26265f.png)

[This](https://user-images.githubusercontent.com/37461272/206490658-206b5b06-15f2-40a1-93d3-ab950fa04f39.png) image is similar. The main difference with these images is whether they depict git *checkout* or *reset*. Why the confusion? Well, the right-going arrows are straightforward. But the left-going arrows could be doing a few different things:
- Bringing changes
- Bringing everything from the other place (which might remove your changes)
- Leaving your current commit and going to a different commit/branch/whatever
- Other misc. stuff

# Terminology

- **commit** (noun) - a snapshot of the state of your code (see above for better explanation)
- **commit** (verb) - create a commit
- **branch** - can refer to: a pointer to the last commit in the branch (stack of commits) structure
- **tree** - a tree-structure of branching commits
- **working directory** / **working tree** / **project folder** - the place on your computer with the workspace and the local repo
- **tree-ish** - the name of a commit or reference to a commit (used in git's documentation)
- **local** - stuff on your computer
- **remote** - non-local (ie, github)
- **master** - the primary branch of a repo 
- **main** - what github renamed master to
- **HEAD** - the last commit in the current branch, describing where you're currently "at"
- **heads** - another term for branches
- **detached head** - a "HEAD" set to something that's *not* the last commit in the current branch
- **fork** - a copy of a branch created with clone or fork commands
- **upstream** - the thing that "git push" pushes to, and "git pull" pulls from, by default
- **origin** - the remote version of your repo fork
- **staging area** / **index** / **cache** - all refer to: what "gid add" adds files to (to indicate what will be part of next commit)
- **stage** (verb) - to add to staging area
- **stash** - a place you can temporarily throw changes to avoid the “you have unstashed changes!” errors, among other things
- **pull request** - a request for someone's branch to pull the changes in your branch
- **squash** - combine multiple commits from the same branch into one
- **reference** / **symbolic reference** - something like HEAD~2 which can be used in place of the actual commit name
- **track** - to have an upstream, so you can push and pull from a branch to what it's tracking
- **untracked file** - file not in staging area
- **remote-tracking branch** - not a branch, and does not track remote. It is a reference to a remote branch, eg, "origin/dev"
- **hash** / **SHA1** / **object identifier** - a random-looking number that works like an id of a commit

# Commands

## Well-known commands

```Git
git add .
git commit -m '<message>'
git push
git status
git diff
```

[explanation](./basics#git-basics)

## Pulling

```Git
git pull = git fetch + git merge 
git fetch = the inverse of push
git merge = the inverse of add+commit
git rebase = like merge, but brings the changes commit-by-commit rather than all at once, and with a different result (it's complicated)
git cherry-pick = like rebase, but with a different syntax and work flow
```

## Starting commands

**For: Remote (Github) repo -> Your computer (Local)**

``git clone <https://repo-link.git>``

**For: Your computer (Local) code -> Remote (Github)**

```Git
git init
// Then add+commit your changes you’ve made so far. Also got to github.com and create an empty repo there

// Linking the github repo to your code:
// (Note: when adding an additional upstream, rather than starting a new repository, switch the order of the following 2 commands)
git branch -M main
git remote add origin <https://repo-link.git>

// On your first push, make sure it is written as
git push -u origin main
```

## Show commits and their hashes

``git log --oneline``

Commands like this display information. Sometimes in the terminal you have to press ``q`` to escape out of that view.

See all the branches laid out in a tree-like structure (quite verbose!):

``git log --graph --oneline --decorate --all``

## Show all local branches

``git branch``

## Change branches

``git checkout <branch name>``

## Create a branch 

First make sure you go to the branch that you want to branch "off of". Then

``git branch <branch name>``

Then you have to move to the branch seperately. To create a branch and switch to it, taking any uncommitted changes with you, all in one action:

``git checkout -b <branch name>``

## Undo the changes of commits

Remove the effects of a certain commit, and put that revert action in a new commit:

``git revert <commit-hash>``

or a range of commits

``git revert <oldest_commit_hash>..<latest_commit_hash>``

or the last X commits

``git revert HEAD~<num>``, eg ``git revert HEAD~3``

Make sure as always you do a push after this if you want it to be reflected

## Undo commits themselves, so it’s as if those commits never happened (!)

Stash uncommitted changes first

To go back to how you were at a certain commit hash:

``git reset --hard <commit-hash>``

If it does not let you do a `git push`, then do `git push -f`. The f stands for force. Sometimes git forces you to do that. 

You can also do without --hard option or with --soft option for different but related effects. For example, to undo the last commit but keep its changes:

``git reset HEAD~1``

**Key note**: In some ways this is a "git undo", but the commit hash you enter is not the one you are undoing. Instead you reset to the thing that came right before what you want to undo. If you have commits w -> x -> y -> z, and you want to undo y and z, you *reset* to x, bringing the code from x to you, and that action will undo y+z changes.

## Undo the git add of a given file

``git reset <file name>``

To do it on all index files, do just ``git reset``.

## Make a given file or directory match how it is elsewhere
  
``git checkout <branch name or commit hash> <path/to/file-name.txt>``

Make a folder in working directory to be like your last commit

``git checkout -- <path/to/file-name.txt>`` 

## Switch and restore
  
Checkout does like 13 different things, so to fix this, the maintainers of git introduced switch and restore. Checkout remains for backwards compatiblity.

``git switch <branch name or commit hash>`` = ``git checkout <branch name or commit hash>``

``git restore <path/to/file-name.txt>`` = ``git checkout -- <path/to/file-name.txt>``

## Comparing checkout, reset, and revert

``git checkout <branch>`` = moves you to that branch

``git checkout <commit hash>`` = moves you to that commit (but keeps any later commits in the repo)

``git reset <commit bash>`` = moves you to that commit and also deletes all later commits

``git revert <commit hash>`` = removes that commit's changes, puts that update in new commit

``git checkout -- <file path>`` = to workspace, removes changes to that file so it looks like in HEAD

``git checkout <commit hash> <file path>`` = to workspace, removes changes to that file so it looks like in that commit

``git reset <file path>`` = removes file from staging 

[Source](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting#:~:text=The%20git%20checkout%20command%20can,which%20inverses%20the%20specified%20commit)

## Show commits

Show unpushed commits

``git log origin/master..HEAD``

Show unpushed commits diffs

``git diff origin/master..HEAD``

Show changes of a specific commit

``git show <commit hash>``

Show just names in files change in last commits, 2 in this case

``git diff --name-only HEAD~2``

## Fix merge conflict / Bring changes from master branch into your branch

I used to do this:

``git pull origin master``

But this is cleaner, albeit more steps:

```Git
git checkout <master>
git pull
git checkout <my branch>
git merge <master>
```
Resolve merge conflicts, git add those, then push

If you are working off of master, then <master> it's simply master. If not, put that instead. For example: ``git merge develop``

If you rebased the `master` branch, do `git pull -r <master>` so github doesn't get confused. More on that [here](https://stackoverflow.com/questions/65790387/steps-to-take-when-rebasing-a-branch-thats-branched-off-a-branch-which-is-branch/65791324#65791324)
  
## Delete accidentally committed files from git repository (eg, from github) without effecting workspace
  
``git rm <path/to/file1.txt> --cached``
  
For directories:
  
``git rm -r <path/to/folder> --cached``, r for recursive
  
Then commit and push to see the removal reflected
  
If you want your workspace to still have the file, then git will consider that a change, and want you to add those files. Fix this by adding the files or folder path to the ``.gitignore``
  
"git add <x>" means "stage <x> change, which might be a file deletion, so my index looks like my workspace". By contrast, "git rm <x>" means "regardless of what is in my workspace, tell the index that the file is deleted, so a deletion will be part of next commit. Do not affect my workspace." 

## Setting upstream

Method #1: set upstream during a push (recommended)

``git push -u origin <branch>``

Method #2: set upstream if you have nothing to push (I'm not completely sure this works in all cases):
  
``git branch --set-upstream-to origin/<branch>``
  
For example, if your branch name is feature/sh/xyz, then the command is
  
``git branch --set-upstream-to origin/feature/sh/xyz``. 

## Remove workspace changes that are not part of index or commit

Removes all untracked files and directories 
  
``git clean -f -d``
  
Removes all untracked file changes, including deletions
  
``git checkout -- .``

## See changes to what HEAD is

The head is continually updating to point to the most recent commit. You can view the history of this. 

``git reflog``

- `HEAD@{0}` = commit hash of what the head is now
- `HEAD@{1}`, `HEAD@{2}`, etc = commit hash of what the head was before last change, 2 changes, etc.
- `ORIG_HEAD` = commit hash of what the head was before last major change
- `my-branch@{'3 days ago'}` = what the head of my-branch was 3 days ago
- [more](https://git-scm.com/docs/git-rev-parse#_specifying_revisions) tricks like this

## Stash

The stash [can get pretty complicated](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning) if you want to use it powerfully. But these are the two most important commands:

``git stash`` - takes uncommitted changes and throws them in the stash. If you don't do this, uncommitted changes in the workspace will come with you when you switch branches

``git stash apply`` - takes changes in the stash and puts them back in your workspace

## Merge multiple commits into one

``git rebase -i HEAD~<number of commits to rebase>``

Then follow instructions in comments in the terminal. It is multi-step. More detailed instructions [here](https://www.internalpointers.com/post/squash-commits-into-one-git).

The editor uses Vim. Basics are: [i] to start typing. [esc] to escape edit mode. In non-edit mode: [j, k, h, l] to move cursor. Type [:wq] to save and finish. Type [:!q] to quite without saving. 

## Rename a branch

Excellent answer found [here](https://stackoverflow.com/questions/6591213/how-do-i-rename-a-local-git-branch). Basics: navigate to the branch to delete. Then

``git branch -m <newname>``

There are a few options for dealing with the upstream branch, which still has the old name. I recommend renaming it on eg github. Then in next push

``git push origin -u <newname>``

# Some Topics Not Covered
  
- [Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
- Showing file history with [file logs](https://alvinalexander.com/git/show-commit-history-detailed-for-single-file/) and [git blame](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-blame)
- [git cherry-pick](https://git-scm.com/docs/git-cherry-pick)
- [Type-insensitivity in file names](https://stackoverflow.com/questions/17683458/how-do-i-commit-case-sensitive-only-filename-changes-in-git)
