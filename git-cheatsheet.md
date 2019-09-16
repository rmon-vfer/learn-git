# GIT Fundamentals

### Basic Commands

##### Initialize an empty git repository on the local directory

```bash
git init .
```

##### Common workflow

1. Create the repo
2. Create and edit files
3. Add the newly created files to the _staging area_ 
4. Commit the files that are on the staging area (take an _snapshot_)

###### Commands

```bash
# Check what has changed since last commit
git status

# Add <file> to staging area (use --all to add all files)
git add <file>

# Commit changes (-m is mandatory)
git commit -m <message>
```

__NOTE:__ `git commit` adds a _checkpoint_ to an imaginary timeline, where the HEAD is the last commit. This timelines are also called _branches_. The `git log` command lists all the changes made in the current timeline (branch).

The `git diff` command shows the unstaged differences since the last commit (to see the differences between the last commit and the current stage, run `git diff --staged`)

To remove a file from the stage just reset the HEAD (that's the last commit): `git reset HEAD <file>`

Another common operations related to commit and stage modiffication are:

- Discard changes: `git checkout -- <file>` resets the stage

- Stage and commit at the same time: `git commit -a -m <message>`, note that the `-a` option doesn't add new files, just commits the already tracked ones.

- Commit moddification

  - Reset the last commit to staging: `git reset --soft HEAD^`

  - Modiffing the last commit:

    ```bash
    git add <file> # Make the changes
    git commit --ammend -m <message> # Fix the last commit
    ```

  - Blowing the last commit away: ``` git reset --hard HEAD^```, deletes the changes (doesn't save anything)

### Remotes

A remote is a repository located on another machine, usually on a hosted git solution such as _GitHub_, _BitBucket_ or _GitLab_.

To use a remote you have to add it first: `git remote add <alias> <remote_url>`

Once it's been added, you can:

- List all remotes: `git remote -v`
- Push to remote: `git push -u <alias> <local_branch>`
- Update the local repo with the remote one: `git pull`
- Remove a remote: `git remote rm <alias>`



### Cloning and branching

_Cloning_ a repository its the same as downloading and adding it as a remote, to clone a remote repo: `git clone <remote_url>`.

A _branch_ is an additional _timeline_ attached to the _master_ one, its mainly used to develop additional features for a given program or application.

You can perform the following operations with branches (<u>**local ones**</u>):

- Create it: `git branch <branch_name>`

- Change from one to another with `git checkout <branch>`

  - Creating and checking a branch out its quite a common task, thats why its possible to perform it with just one command: `git checkout -b <branch_name>`

- Delete a branch: `git branch -d <branch>` (use `-D` to force)

- Merge two branches: 

  ```bash
  # ALWAYS branch from master
  # First, checkout (switch to) master
  git checkout master
  
  # Now, merge master with another branch, cool-feature
  git merge cool-feature
  ```

  A merge is a _fast forward_ one if the master branch hasn't been updated at the moment the merge is performed.

  If the merge is not _fast forward_, git will open the _vi_ editor and ask for a commit message explaining why the merge was done. 



### Collaboration and remote branches

Common operations:

- Create a remote branch:

  ```bash
  # First, create and checkout the branch
  git checkout -b <branch-name>
  
  # Then, push the local branch to remote
  git push <remote-alias> <branch-name>
  ```

- List remote branches: `git remote show <remote-alias>`

- Delete a remote branch:

  ```bash
  # Delete the remote branch
  git push <remote-name> :<branch-name>
  
  # Now delete the local branch manually
  git branch -d <branch-name>
  ```



### Rebase

To skip merge commits

```bash
# Pulls down the changes but doesn't merge diffs.
git fetch

# Moves the local branch commits to a temporary area 
# and then commits the remote ones one-by-one, finally, 
# it does the same with local ones. 
# Result: no merge commits
git rebase
```

Basically, `git rebase` converts a _non-fast forward_ commit into a _fast forward_ one to skip merge commits.