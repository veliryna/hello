# Git Ready

GitHub repository: https://github.com/veliryna/hello

## Setting Up Git

- Install Git on your local machine by following the instructions for your operating system on the official [Git website](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
```bash
  sudo apt-get install git
```
- Configure Git with your username and email address.
```bash
  git config --global user.name "name"
  git config --global user.email "email"
```
## Git commits to commit

- Within the `work` directory, establish a subdirectory named `hello`. Inside this directory, generate a file titled `hello.sh` and input the following content:
- Initialize the git repository in the `hello` directory.
- Check the status and act accordingly with the output of the executed command.
```bash
  mkdir work && cd work
  mkdir hello && cd hello
  echo 'echo "Hello, World"' > hello.sh
  git init
  git add hello.sh
  git commit -m "Initial commit: Add hello.sh"
```
- Change the `hello.sh` content to the following:
- Stage the changed file and commit the changes, the working tree should be clean.
- Modify the `hello.sh` file to include comments and stage it.
```bash
# Modify hello.sh
  echo '#!/bin/bash' > hello.sh
  echo 'echo "Hello, $1"' >> hello.sh

  # Stage and commit
  git add hello.sh
  git commit -m "Update hello.sh to accept arguments"

  # Add comments to hello.sh
  echo '# Default is "World"' > hello.sh
  echo 'name=${1:-"World"}' >> hello.sh
  echo 'echo "Hello, $name"' >> hello.sh
```

- Make two separate commits:
  - The first commit should be for the comment in line 3.
  - The second commit should include changes made to lines 4 and 5.
```bash
git add hello.sh
git commit -m "Add comments to hello.sh"
```

## History

- Show the history of the working directory.
- Show One-Line History for a condensed view showing only commit hashes and messages.
- **Controlled Entries**:
  - You need to customize the log output by specifying the number of entries or a time range. Customize it to display the last `2 entries` and to view the `commits made within the last 5 minutes`.
- **Personalized Format**:
  - Show logs in a personalized format, including the commit hash, date, message, branch information, and author name, resembling `* e4e3645 2023-06-10 | Added a comment (HEAD -> main) [John Doe]`
```bash
  git log                    # Full history
  git log --oneline          # Condensed history
  git log -n 2               # Last 2 entries
  git log --since="5 minutes" # Commits from the last 5 minutes
  git log --pretty=format:"* %h %ad | %s (%d) [%an]" --date=short
```
## Check it out

- **Restore First Snapshot**:
  - Revert the working tree to its initial state, as captured in the first snapshot, and then print the content of the `hello.sh` file.
- **Restore Second Recent Snapshot**:
  - Revert the working tree to the second most recent snapshot and print the content of the `hello.sh` file.
- **Return to Latest Version**:
  - Ensure that the working directory reflects the latest version of the `hello.sh` file present in the main branch, without referring to specific commit hashes.

```bash
  git checkout <first-commit-hash> hello.sh  # Restore first snapshot
  git checkout HEAD~1 hello.sh              # Restore second-most-recent snapshot
  git checkout main hello.sh                # Restore the latest version
```
## TAG me

- **Referencing Current Version**:
  - Tag the current version of the repository as `v1`.
- **Tagging Previous Version**:
  - Tag the version immediately prior to the current version as `v1-beta`, without relying on commit hashes to navigate through the history.
- **Navigating Tagged Versions**:
  - Move back and forth between the two tagged versions, `v1` and `v1-beta`.
- **Listing Tags**:
  - Display a list of all tags present in the repository to verify successful tagging.

```bash
  git tag v1                                 # Tag current version as v1
  git tag v1-beta HEAD~1                     # Tag previous version as v1-beta
  git checkout v1                            # Switch to v1
  git checkout v1-beta                       # Switch to v1-beta
  git tag                                    # List all tags

```

## Changed your mind?

- **Reverting Changes**:
  - Modify the latest version of the file with unwanted comments, then revert it back to its original state before staging using a `Git` command.
- **Staging and Cleaning**:
  - Introduce unwanted changes to the file, stage them, then clean the staging area to discard the changes.
- **Committing and Reverting**:
  - Add the following unwanted changes again, stage the file, commit the changes, then revert them back to their original state.
- **Tagging and Removing Commits**:
  - Tag the latest commit with `oops`, then remove commits made after the `v1` version. Ensure that the `HEAD` points to `v1`.
- **Displaying Logs with Deleted Commits**:
  - Show the logs with the deleted commits displayed, particularly focusing on the commit tagged `oops`.
- **Cleaning Unreferenced Commits**:
  - Ensure that unreferenced commits are deleted from the history, meaning there should be no logs for these deleted commits.
- **Author Information**:
  - Add an author comment to the file and commit the changes.
```bash
# Modify the file with unwanted comments
echo -e '#!/bin/bash\n\n# the comment\nname=${1:-"World"}\necho "Hello, $name"' > hello.sh

# Revert the file back to its original state before staging
git restore hello.sh

# Introduce unwanted changes to the file
echo -e '#!/bin/bash\n\n# Another comment\nname=${1:-"World"}\necho "Hello, $name"' > hello.sh

# Stage the unwanted changes
git add hello.sh

# Clean the staging area to discard the changes
git restore --staged hello.sh

# Introduce unwanted changes again
echo -e '#!/bin/bash\n\n# 2nd comment\nname=${1:-"World"}\necho "Hello, $name"' > hello.sh

# Stage and commit the unwanted changes
git add hello.sh
git commit -m "Add unwanted comment to hello.sh"

# Revert the committed changes
git revert HEAD

# Tag the latest commit with 'oops'
git tag oops

# Reset HEAD to the v1 version
git reset --hard v1

# View deleted commits
git reflog

# View details of the commit 'oops'
git show oops

# Delete unreferenced commits
git gc --prune=now

# Add an author comment
echo -e '#!/bin/bash\n\n# Default is World\n# Author: Your Name\nname=${1:-"World"}\necho "Hello, $name"' > hello.sh

# Stage and commit
git add hello.sh
git commit -m "Author comment to hello.sh"
```


## Move it

- **Moving hello.sh**:
  - Using Git commands, move the program `hello.sh` into a `lib/` directory, and then commit the move.
  - Create a `Makefile` in the root directory of the repository with the provided content and commit it to the repository.
```bash
  # Move hello.sh to lib/ directory
  mkdir lib
  git mv hello.sh lib/

  # Add and commit the move
  git commit -m "Move hello.sh to lib directory"

  # Add Makefile and commit
  echo -e 'TARGET="lib/hello.sh"\n\nrun:\n\tbash ${TARGET}' > Makefile
  git add Makefile
  git commit -m "Add Makefile"
```
## blobs, trees and commits

- **Exploring `.git/` Directory**:
  - Navigate to the `.git/` directory in your project and examine its contents.You will have to explain the purpose of each subdirectory, including `objects/`, `config`, `refs`, and `HEAD` in the audit.
- **Latest Object Hash**:
  - Find the latest object hash within the `.git/objects/` directory using Git commands and print the type and content of this object using Git commands.
- **Dumping Directory Tree**:
  - Use Git commands to dump the directory tree referenced by this commit.
  - Dump the contents of the `lib/` directory and the `hello.sh` file using Git commands.
```bash
# Navigate to the .git
cd .git
ls -la
# Find the latest object hash
git log --pretty=format:"%H" -n 1       

# Print the type
git cat-file -t <object-hash>           

# Print the content
git cat-file -p <object-hash>

# Get the hash of the latest commit
git log --pretty=format:"%H" -n 1       

# Dump the directory tree for the latest commit
git ls-tree <commit-hash>               

# Dump the contents of the `lib/` directory
git ls-tree <commit-hash> lib/          

# Dump the contents of the `hello.sh` file
git show <commit-hash>:lib/hello.sh
```


## Branching

It's time to do a major rewrite of the hello world functionality. Since this might take a while, you'll want to put these changes into a separate branch to isolate them from changes in the main branch.

- **Create and Switch to New Branch**:
  - Create a local branch named `greet` and switch to it.
  - In the `lib` directory, create a new file named `greeter.sh` and add the provided code to it. Commit these changes.
- Update the `lib/hello.sh` file by adding the content below, stage and commit the changes.
- Update the `Makefile` with the following comment and commit the changes.
- Switch back to the `main` branch, compare and show the differences between the `main` and `greet` branches for `Makefile`, `hello.sh`, and `greeter.sh` files.
```bash
git branch greet                          
git checkout greet                        

# Add greeter.sh
echo -e '#!/bin/bash\n\nGreeter() {\n    who="$1"\n    echo "Hello, $who"\n}' > lib/greeter.sh
git add lib/greeter.sh
git commit -m "Add greeter.sh"

# Update hello.sh
echo -e '#!/bin/bash\n\nsource lib/greeter.sh\n\nname="$1"\nif [ -z "$name" ]; then\n    name="World"\nfi\n\nGreeter "$name"' > lib/hello.sh
git add lib/hello.sh
git commit -m "Update hello.sh to use greeter.sh"

# Update Makefile
echo -e '# Ensure it runs the updated lib/hello.sh file\nTARGET="lib/hello.sh"\n\nrun:\n\tbash ${TARGET}' > Makefile
git add Makefile
git commit -m "Update Makefile for new structure"

```


## Conflicts, merging and rebasing

- **Merge Main into Greet Branch**:
  - Start by merging the changes from the `main` branch into the `greet` branch.
  - Switch to `main` branch and make the changes below to the `hello.sh` file, save and commit the changes.
```bash
# Switch to the greet branch
git checkout greet

# Merge changes from the main branch into the greet branch
git merge main

# Switch to the main branch
git checkout main

# Modify hello.sh with the new content
echo -e '#!/bin/bash\n\necho "What'\''s your name"\nread my_name\n\necho "Hello, $my_name"' > hello.sh

# Stage and commit the changes
git add hello.sh
git commit -m "Update hello.sh"
```
- **Merging Main into Greet Branch (Conflict)**:
  - Attempt to merge the `main` branch into `greet`. Bingooo! There you have it, a `conflict`.
  - Resolve the conflict (manually or using graphical merge tools), accept changes from `main` branch, then commit the conflict resolution.
- **Rebasing Greet Branch**:
  - Go back to the point before the initial merge between `main` and `greet`.
  - Rebase the `greet` branch on top of the latest changes in the `main` branch.
- **Merging Greet into Main**:
  - Merge the changes from the `greet` branch into the `main` branch.
```bash
git checkout greet
git merge main

# Conflict here

# Stage the resolved file and commit
git add hello.sh
git commit -m "Resolve merge conflict by accepting changes from main"

# Reset the greet branch to the state before the initial merge
git reset --hard HEAD~1

# Rebase the greet branch onto the latest changes from main
git rebase main

# Switch to the main branch
git checkout main

# Merge changes from the greet branch into the main branch
git merge greet
```
- **Understanding Fast-Forwarding and Differences**:
  - Explain fast-forwarding and the difference between merging and rebasing.
A fast-forward merge happens when the branch you're merging into has no new commits since the branch you're merging from diverged. Instead of creating a merge commit, Git simply moves the branch pointer forward.
Difference Between Merging and Rebasing:

- Merging:
      -  Combines the changes from two branches into one.
      -  Retains the history of both branches.
      -  Creates a merge commit to signify the merge.
- Rebasing:
      -  Reapplies commits from one branch on top of another branch.
      -  Creates a linear history without merge commits.
      -  Should be avoided on public/shared branches.
## Local and remote repositories

- In the `work/` directory, make a clone of the repository `hello` as `cloned_hello`. (Do not use `copy` command)

- Show the logs for the cloned repository.

- Display the name of the remote repository and provide more information about it.

- List all remote and local branches in the `cloned_hello` repository.

- Inside the cloned repository (`cloned_hello`), fetch the changes from the remote repository and display the logs. Ensure that commits from the `hello` repository are included in the logs.

- Merge the changes from the remote `main` branch into the local `main` branch.

- Add a local branch named `greet` tracking the remote `origin/greet` branch.

- Add a `remote` to your Git repository and push the `main` and `greet` branches to the `remote`.

```bash
git clone hello cloned_hello              # Clone repository
cd cloned_hello
git log                                   # View logs

# Add a remote
git remote add origin <remote-url>
git fetch origin                          # Fetch changes from remote
git merge origin/main 

Other task points were done manually in Visual Studio Code, not with terminal commands
```

## Bare repositories

- What is a bare repository and why is it needed?
A bare Git repository is typically used as a Remote Repository that is sharing a repository among several different people. You don't do work right inside the remote repository so there's no Working Tree (the files in your project that you edit), just bare repository data.
- Create a bare repository named `hello.git` from the existing `hello` repository.
- Add the bare `hello.git` repository as a remote to the original repository `hello`.
- Commit the changes and push them to the shared repository.
- Switch to the cloned repository `cloned_hello` and pull down the changes just pushed to the shared repository.
```bash
# Create bare repository
git clone --bare hello hello.git

# Add remote and push
cd hello
git remote add shared ../hello.git
git push shared main

# Pull changes in the cloned repository
cd ../cloned_hello
git pull shared main
