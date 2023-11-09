# Git

## Quick reference

For commands that are easy to forget that I've used heavily before that I might need later.

### Delete Git submodule - modern

Taken from [this gist](https://gist.github.com/myusuf3/7f645819ded92bda6677?permalink\_comment\_id=3936499#gistcomment-3936499).

```
# Remove the submodule entry from .git/config
git submodule deinit -f path/to/submodule

# Remove the submodule directory from the superproject's .git/modules directory
rm -rf .git/modules/path/to/submodule

# Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
git rm -f path/to/submodule

# Remove file
rm -rf path/to/submodule
```

### Delete Git Submodule - old

```bash
# 1. Delete the relevant section from the .gitmodules file.
nano .gitmodules
# 2. Stage the .gitmodules changes
git add .gitmodules
# 3. Delete the relevant section from .git/config
nano .git/config
# 4. Delete path with git cache
git rm --cached path_to_submodule
# 5. Delete path in Git modules
rm -rf .git/modules/path_to_submodule
# 6. Delete path to untracked submodules
rm -rf ./path_to_submodule
git add .
git commit -m "Removed submodule"
```

### For scripting - Display the current branch you're on

```
git rev-parse --abbrev-ref HEAD
```

### For scripting - get the Git SHA hash

```
# Long hash
git rev-parse HEAD

# short hash
git rev-parse HEAD --short
```

### Add Git Submodule to a subdirectory

```
git submodule add git@github.com:Azure/azure-policy.git azure_guardrails/shared/azure-policy 
```

### Make a new Git Commit without modifying files

```
git commit --allow-empty
# No more random whitespace changes to trigger new builds! 
```

### Delete a tag

```
some_tag=0.1.1 
git tag -d $some_tag
git push --delete origin $some_tag

```

### Duplicating a repository

1. Open Terminal.
2.  Create a bare clone of the repository.

    ```
    $ git clone --bare https://github.com/exampleuser/old-repository.git
    ```
3.  Mirror-push to the new repository.

    ```
    $ cd old-repository.git
    $ git push --mirror https://github.com/exampleuser/new-repository.git
    ```
4.  Remove the temporary local repository you created earlier.

    ```
    $ cd ..
    $ rm -rf old-repository.git
    ```

### **Discarding and Deleting**

**Discard uncommitted changes**

```
git reset --hard
```

**Delete all untracked files and directories**

```
git clean -fd
```

### Nuke all previous commits

```
git checkout --orphan newBranch
git add -A                          # Add all files and commit them
git commit -m "Cleanup"
git branch -D master                # Deletes the master branch
git branch -m master                # Rename the current branch to master
git push -f origin master           # Force push master branch to github
git gc --aggressive --prune=all     # remove the old files
```

### Syncing a fork

Taken from [here](https://help.github.com/en/articles/syncing-a-fork).

```
git remote add upstream https://github.com/[Original Owner Username]/[Original Repository].git
git fetch upstream
git checkout master
git merge upstream/master
# If your local branch didn't have any unique commits, Git will instead perform a "fast-forward"
```

### Pro Tips

`git pull --rebase origin master` when pulling/syncing changes from master. It makes for a heaps cleaner log history, and also makes it easier to do rebases for situations like this where you might want to cleanup/remove things like this

## The rest (Borrowed from elsewhere)

Borrowed this for comprehensiveness

### **Basic**

**List all local branches**

```
git branch
```

**Commit (permanently record) staged changes**

```
git commit -m "message"
```

**Upload new branch to origin remote**

```
git push -u origin <new_branch>
```

**List files to be committed**

```
git status
```

### **Submodules**

**Git submodule update**

```
    git submodule update --init --recursive
```

### **Diff**

**Show unstaged changes**

```
git diff
```

**Show changes using file diff tool**

```
git difftool
```

**Show staged changes**

```
git diff --staged
```

### **Commit management**



**Create a ‘fixup!’ comit for older commit (to be autosquashed)**

```
git commit --fixup <sha1>
```

**Create a ‘squash!’ commit for older commit (to be autosquashed)**

```
git commit --squash <sha1>
```

### **Merge**

**Merge other branch into current one**

```
git merge <other_branch>
```

### Rebase

**Perform interactive rebase**

```
git rebase -i <other_branch>
```

**Perform interactive rebase with autosquash**

```
git rebase -i --autosquash <other_branch>
```

### **Branch**

**Rename a branch**

```
git branch -m <old_name> <new_name>
```

**Delete a branch**

```
git branch -d <old_name>
```

**Create new branch while staying on current one**

```
git branch <new_branch_name>
```



**Print name of current branch**

```
git rev-parse --abbrev-ref HEAD
```

**Create new branch and switch to it**

```
git checkout -b <new_branch_name>
```

### **Cherry-pick**

**Apply an existing commit to current branch (cherry-pick)**

```
git cherry-pick <sha1>
```

### rm

**Delete a file and stage deletion**

```
git rm <file_name>
```

**Stage file deletion from repo, but keep it locally**

```
git rm --cached <file_name>
```

### **mv**

**Stage file moving or renaming**

```
git mv <old_filepath <new_filepath>
```

### **Stash**

**Stash (temporarily put away) uncommitted changeset**

```
git stash
```

**Restore most recent stashed changeset**

```
git stash pop
```

**List all stashed changesets**

```
git stash list
```

**Delete last stashed changeset**

```
git stash drop
```

**Edit last commit with new staged changes**

```
git commit --amend
```

### **Reset**

**Undo (discard) last commit**

```
git reset HEAD~
```

**Undo (discard) last 3 commits**

```
git reset HEAD~3
```

### **Revert**

**Revert previous commit by creating opposite commit**

```
git revert <sha1>
```

### **Tag**

**Create new tag**

```
git tag -a <tag_name>
```

**List tags**

```
git tag
```

### **History**

**Show history for current branch**

```
git log
```

**Show history, one line per commit**

```
git log --oneline
```

**Show history, with merged branch graph**

```
git log --graph
```

**Show history for specific file**

```
git log --follow <file_path>
```

**Show specific commit (contents and metadata)**

```
git show <sha1>
```

###
