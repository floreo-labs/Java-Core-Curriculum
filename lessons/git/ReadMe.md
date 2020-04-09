# Git Q & A

# PUSH, PULL, FETCH

### What is git remote?

A `remote` is git's fancy way of saying "the place where your code is stored." That URL could be your repository on GitHub, or another user's fork, or even on a completely different server.

A `remote` can point to two types of URL addresses:

- An HTTPS URL like `https://github.com/user/repo.git`
- An SSH URL, like `git@github.com:user/repo.git`

Git associates a `remote` with a name, and your default `remote` is usually called `origin`. You can have more than one remote (they'll each have a different name).

You can use the `git remote add` command to match a remote URL with a name. For example, you'd type the following in the command line:

`git remote add origin https://github.com/user/repo.git`

This associates the name `origin` with https://github.com/user/repo.git. Now when you run `git push origin <branch-name>`, git will push to `<branch-name>` at https://github.com/user/repo.git.

You can also use the `command git remote set-url` to change a remote's URL.

We can list remotes for our local repo by running `git remote -v`.

### What is git push?

`git push` will find the commits you have on your local branch that the corresponding branch on the remote repository does not have, and send them to the corresponding branch on the remote repository.

### What is git pull? What is the difference between fetch and pull?

`git pull` incorporates changes from a remote repository into the current branch. In its default mode, `git pull` is shorthand for `git fetch` followed by `git merge FETCH_HEAD`: in other words,  `git pull` runs `git fetch` ***and*** calls `git merge` to merge the retrieved branch heads into the current branch.

### When should I use git fetch vs. git pull? How do you pull updates?

`git fetch` only downloads new data from a remote repository. **It doesn't merge any of this new data into your working files.** `git fetch` is good for getting a fresh view on all the things that happened in a remote repository. Due to it's "harmless" nature, you can rest assured: fetch will never manipulate, destroy, or screw up anything.

`git pull`, in contrast, is used with a different goal in mind: to update your current `HEAD` branch with the latest changes from the remote server. This means that pull not only downloads new data; ***it also merges it into your current working copy files.*** 

This has a couple of consequences:

- Since `git pull` tries to merge remote changes with your local ones, a merge conflict can occur. 
- Start a `git pull` only with a clean working copy. This means that you should not have any uncommitted local changes before you pull. You can use `git stash` to save your local changes temporarily before pulling.

### After cloning a repo, why are some branches hidden?

When you clone a repository, all remote branches are created as "remote tracking branches" in your repository. These aren't shown by default, but you can see these with `git branch -a`.

### How do we switch to another remote branch? Is it the same as switching to local?

If you run `git checkout <your-remote-branch-name>`, git will find the remote tracking branch of the same name, automatically create a new local branch from the same commit, and switch to the new local branch.

### How do I create a remote branch?

First, you create your branch locally:

`git checkout -b <branch-name>`

The remote branch is automatically created when you push it to the remote server. So when you're ready to create a remote branch from your local branch, you can just do:

`git push <remote-name> <branch-name>`

Where `<remote-name>` is typically origin. You can also add the `-u` flag when you push to ensure that git will know about the branch the next time you decide to pull, i.e `git push -u origin <branch-name>`.


# BRANCHING

### What does the branching structure look like?

Try creating some branches and commits on https://onlywei.github.io/explain-git-with-d3/ to visualize git's branching structure.

### What are heads?

`HEAD` always refers to the most recent commit on the current branch (`HEAD` is the "tip" of the current branch). When you change branches, `HEAD` is updated to refer to the new branch’s latest commit.

![git HEAD](https://people.gnome.org/~federico/misc/git-repo-3.png)

We can use `HEAD` to refer to the most recent commit, and use `HEAD~` as the commit before the tip, and `HEAD~~` or `HEAD~2` as the commit even earlier, and so forth.

See also: [What is a detached HEAD?](https://www.git-tower.com/learn/git/faq/detached-head-when-checkout-commit)

### What does the "Your branch is ahead of 'origin/master' by N commit" message mean?

That message means that you have made `N` commits in a branch in your local repo, and have not pushed them to the remote repository (i.e. using `git push`). 

Your local branch can also be *behind* a remote branch. For example, in the image below, the `foo` branch is both 2 commits ahead and 2 commits behind `master`.

![ahead of master](https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcSfJs3ulkL_ieL4DGR7jpkmwGGYMt9xy4ybAGYoEzkA2dYcmCr0Kw)

### How does one checkout a branch from a previous commit?

You can check out a previous commit with `git checkout <commit-hash>`. You can discover the commit hash by running `git log`.

Once you've checked out the previous commit, you're in a **detached head** state. You can create and checkout a new branch with `git checkout -b <new-branch-name>` and you'll no longer be in the detached head state.

### Can you undo a commit from terminal?

`git reset HEAD <commit-hash-or-branch>` will move `HEAD` and the current branch back to wherever you specify, abandoning any commits that may be left behind. This is useful to undo a commit that you no longer need.

The ref `HEAD` is usually used together with this command. Remember we use `HEAD` to refer to the most recent commit, and use `HEAD~` as the commit before the tip, and `HEAD~~` or `HEAD~2` as the commit even earlier, and so forth.

[A recommended resource for learning more about reset.](https://git-scm.com/2011/07/11/reset.html)

**KEEP IN MIND:** To undo commits that have already been pushed to the remote repository, we cannot use `git reset`. Instead, we have to use `git revert <commit-hash>`.

`git revert` will create a new commit that will undo all of the work that was done in the commit you want to revert.

### Do we checkout then pull, or pull then checkout?

`git checkout <branch-name>` will checkout out a specific branch. `git pull` will fetch changes from remote for the current branch (assuming there is an equivalent upstream) and merge them into the current branch.

Therefore, you would want to checkout the branch you'd like to update before running `git pull`.

# MERGING

### What is merging?

### When merging is it the entire project being merged, or just a specific branch?

### What is a merge conflict?

Merge conflicts arise when there are differences within the same lines between the base commit and the branch that you’re trying to merge in. For example, if two people changed the same lines in the same file, or if one person decided to delete a line while the other person decided to modify that same line, Git simply cannot know what is correct.

Finally, after solving all conflicts, a merge conflict situation needs to be concluded by a regular commit to mark it's resolution.

### How do I resolve a merge conflict?

### What tools can I use to resolve a merge conflict?

### How do I fix merge conflicts from Android Studio?

### When we resolve merge conflicts on Android Studio or IntelliJ, how does it work?


# STASHING

### What is git stash?

### How / when should we be using git stash?

`git stash` is used when you are working on a project that is not yet completed and want a clean working directory to work on something else. You stash it and can come back to work on it again later.  Stashing will save both staged and unstaged work.  The command is `git stash` and when you are ready to get back your stashed work the command is `git stash pop`.

### How do I restore a stash?

Stash is a way to store your current changes and restore your project to the last commit. This command is particularly useful in the case where you are actively updating your code with a team. An example of this would be if you were in the midst of writing your code, and someone on your team has committed changes. In this scenario, Stash is a useful command to store your changes away and pull the updated repo without any commit issues or merge conflicts. If you want to go back to pull your stash, there are a number of useful commands:

- `git stash list`: list of stashed modifications
- `git stash show`: used to inspect them
- `git stash apply`: a way to restore (potentially on top of a different commit) 

### How do I make a branch off a stash?


# HOW DOES GIT WORK?

### How does git differ from other VCSs?

### What data structure does git use?

### How does git keep track of so much code in memory?

Git keeps track of the code by keeping snapshots of the commits that you make. When you commit, git basically saved a snapshot of the project and records a manifest of what the files look like at that point and stores a reference to that. It also compresses the data that is saved with the idea of “delta compression".

# WORKFLOW


### Is it good practice to push to a branch or to master?

It's almost never a good practice to push to master.

### Describe a basic git workflow.

### How can I separate different features by branch?

### What kind of git structure is typically used at companies? How many branches?

### How can I make my branch follow the changes of a specific branch on another repo?
