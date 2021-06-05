# Git

## Quick reference

### Add Git Submodule to a subdirectory

```text
git submodule add git@github.com:Azure/azure-policy.git azure_guardrails/shared/azure-policy 
```

### Make a new Git Commit without modifying files

```text
git commit --allow-empty
# No more random whitespace changes to trigger new builds! 
```

### Delete a tag

```text
some_tag=0.1.1 
git tag -d $some_tag
git push --delete origin $some_tag

```

### Duplicating a repository

1. Open Terminal.
2. Create a bare clone of the repository.

   ```text
   $ git clone --bare https://github.com/exampleuser/old-repository.git
   ```

3. Mirror-push to the new repository.

   ```text
   $ cd old-repository.git
   $ git push --mirror https://github.com/exampleuser/new-repository.git
   ```

4. Remove the temporary local repository you created earlier.

   ```text
   $ cd ..
   $ rm -rf old-repository.git
   ```

### **Discarding and Deleting**

**Discard uncommitted changes**

```text
git reset --hard
```

**Delete all untracked files and directories**

```text
git clean -fd
```

### Nuke all previous commits

```text
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

```text
git remote add upstream https://github.com/[Original Owner Username]/[Original Repository].git
git fetch upstream
git checkout master
git merge upstream/master
# If your local branch didn't have any unique commits, Git will instead perform a "fast-forward"
```

### Pro Tips

`git pull --rebase origin master` when pulling/syncing changes from master. It makes for a heaps cleaner log history, and also makes it easier to do rebases for situations like this where you might want to cleanup/remove things like this

## The rest

### **Basic**

**List all local branches**

```text
git branch
```

**Commit \(permanently record\) staged changes**

```text
git commit -m "message"
```

**Upload new branch to origin remote**

```text
git push -u origin <new_branch>
```

**List files to be committed**

```text
git status
```

### **Submodules**

**Git submodule update**

```text
git submodule update --init --recursive
```

### **Diff**

**Show unstaged changes**

```text
git diff
```

**Show changes using file diff tool**

```text
git difftool
```

**Show staged changes**

```text
git diff --staged
```

### **Commit management**

\*\*\*\*

**Create a ‘fixup!’ comit for older commit \(to be autosquashed\)**

```text
git commit --fixup <sha1>
```

**Create a ‘squash!’ commit for older commit \(to be autosquashed\)**

```text
git commit --squash <sha1>
```

### **Merge**

**Merge other branch into current one**

```text
git merge <other_branch>
```

### Rebase

**Perform interactive rebase**

```text
git rebase -i <other_branch>
```

**Perform interactive rebase with autosquash**

```text
git rebase -i --autosquash <other_branch>
```

### **Branch**

**Rename a branch**

```text
git branch -m <old_name> <new_name>
```

**Delete a branch**

```text
git branch -d <old_name>
```

**Create new branch while staying on current one**

```text
git branch <new_branch_name>
```

\*\*\*\*

**Print name of current branch**

```text
git rev-parse --abbrev-ref HEAD
```

**Create new branch and switch to it**

```text
git checkout -b <new_branch_name>
```

### **Cherry-pick**

**Apply an existing commit to current branch \(cherry-pick\)**

```text
git cherry-pick <sha1>
```

### rm

**Delete a file and stage deletion**

```text
git rm <file_name>
```

**Stage file deletion from repo, but keep it locally**

```text
git rm --cached <file_name>
```

### **mv**

**Stage file moving or renaming**

```text
git mv <old_filepath <new_filepath>
```

### **Stash**

**Stash \(temporarily put away\) uncommitted changeset**

```text
git stash
```

**Restore most recent stashed changeset**

```text
git stash pop
```

**List all stashed changesets**

```text
git stash list
```

**Delete last stashed changeset**

```text
git stash drop
```

**Edit last commit with new staged changes**

```text
git commit --amend
```

### **Reset**

**Undo \(discard\) last commit**

```text
git reset HEAD~
```

**Undo \(discard\) last 3 commits**

```text
git reset HEAD~3
```

### **Revert**

**Revert previous commit by creating opposite commit**

```text
git revert <sha1>
```

### **Tag**

**Create new tag**

```text
git tag -a <tag_name>
```

**List tags**

```text
git tag
```

### **History**

**Show history for current branch**

```text
git log
```

**Show history, one line per commit**

```text
git log --oneline
```

**Show history, with merged branch graph**

```text
git log --graph
```

**Show history for specific file**

```text
git log --follow <file_path>
```

**Show specific commit \(contents and metadata\)**

```text
git show <sha1>
```

### \*\*\*\*

