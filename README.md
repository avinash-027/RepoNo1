# RepoNo1
Cloning is for getting a copy of an existing remote repository to your local machine.

## Overview
This repository provides a simplified guide to clone, configure, and push changes to a remote repository using Git.

## Setup

### 1. **Global Git Configuration**

Set up your global Git settings (username, email, editor, and autocrlf):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"  # Set editor to Visual Studio Code
git config --global core.autocrlf true          # Enable autocrlf on Windows
```

### 2. **Clone the Repository**

Clone the remote repository to your local machine:

```bash
git clone https://github.com/user/repository.git  # Replace with the actual repo URL
cd repository                                    # Navigate into the cloned repo
```

### 3. **Local Git Configuration** (Optional)

Override your global settings for the specific repo:

```bash
git config user.name "Your Local Name"
git config user.email "your_local_email@example.com"
git config core.editor "nano"                   # Set editor to Nano
git config core.autocrlf false                   # Disable autocrlf for the repo
git config --list --local                        # List repo-specific settings
```

### 4. **Making Changes and Committing**

To make and commit changes:

```bash
git add .                                        # Stage all changes
git commit -m "Updated README file"              # Commit changes with a message
git push origin main                             # Push to the remote 'main' branch

git pull                                         # Fetch and merge changes from the remote repository
```

## Git Commands

  ```bash
  git log --oneline  # View commit history
  git status -s      # View status

  git branch                                      # List local branches
  git branch -r                                   # List remote branches
  git branch -a                                   # List all branches (local + remote)
  git branch branch_name                          # Create a new branch locally
  git checkout branch_name                        # Switch to a different branch
  git checkout -b branch_name                     # Create and switch to a new branch
  git merge branch_name                           # Merge the specified branch into the current branch
  git branch -d branch_name                       # Delete a local branch (only if it's fully merged)
  git push origin --delete branch_name            # Delete a remote branch

  git remote -v              # Show remote URLs for the repo
  git remote show origin     # Show detailed info about the 'origin' remote

  git checkout <commit-hash>     # To checkout a specific commit (detached HEAD)
  git reset <commit-hash>        # To move the branch pointer to a specific commit (keep changes)
  git reset --hard <commit-hash> # To move the branch pointer to a specific commit (discard changes)

  git config --list --local # List local repository settings
  git config --list --global # List global settings
  ```

- HEAD -> main: HEAD points to the main branch, indicating that this is the current commit on your local machine's main branch. 

- origin/main: This indicates that the commit is also present on the remote main branch (on GitHub). 
- origin/HEAD: This points to the default branch on the remote repository (main in this case).

``` 
git log --graph --oneline --decorate --all 
```
``` 
git log --oneline --decorate --all 
```

### Deleting and Renaming Branches in Git

1. **Delete a Local Branch**  
   - Use the command:  
     ```
     git branch -d <branch-name>
     ```  
     To force delete (if the branch hasn't been merged):  
     ```
     git branch -D <branch-name>
     ```

2. **Delete a Remote Branch**  
   - Use the command:  
     ```
     git push origin --delete <branch-name>
     ```

3. **Rename a Local Branch**  
   - First, switch to a different branch (if you're on the branch you're renaming):  
     ```
     git checkout <another-branch>
     ```  
   - Rename the branch:  
     ```
     git branch -m <old-branch-name> <new-branch-name>
     ```

4. **Rename a Remote Branch**  
   - Rename the local branch first (as shown above).  
   - Delete the old remote branch:  
     ```
     git push origin --delete <old-branch-name>
     ```  
   - Push the renamed branch and set the upstream:  
     ```
     git push origin -u <new-branch-name>
     ```  
### Rename a Commit

1. Renaming the Most Recent Commit
```
git commit --amend --no-edit
git commit --amend
```
2. Renaming an Older Commit (Interactive Rebase)
```
git rebase -i HEAD~3

reword <commit_hash> <commit_message>
pick <commit_hash> <commit_message>
```

### To delete commit:

**Option 1: Using `git rebase` (Interactive)**

1. Start interactive rebase:
   ```bash
   git rebase -i b75ea4a^
   ```

2. In the editor, delete(drop) the line with `b75ea4a` (just remove the line).

3. Save and close the editor.

4. If there are conflicts, resolve them and run:
   ```bash
   git rebase --continue
   ```

**Option 2: Using `git reset` (Hard Reset)**

1. Find the commit before `b75ea4a` (let's call it `parent_commit_hash`).

2. Reset to that commit:
   ```bash
   git reset --hard <parent_commit_hash>
   ```

3. If pushed to remote, force push:
   ```bash
   git push origin main --force
   ```

**3. Using git reflog to recover from mistakes (if needed)**

If you made a mistake or want to undo the changes after performing one of the above methods, you can use git reflog to find the commit and reset to it:
```
git reflog
git reset --hard <reflog_commit_hash>
```

## Commit Graph Structure:

- '*' represents commits.

- Branches and merges are represented with lines and arrows:

    | shows the relationship between commits on the same branch.

    / and \ show where branches diverge and merge.

- HEAD points to your current commit .

- origin/main points to the same commit as HEAD, showing the current remote state.

##
This will show the local branches along with their remote counterparts and whether they are up-to-date.
``` 
git branch -vv 
```

This will display a list of recent actions and commits for HEAD
``` 
git reflog 
```
The --root option ensures that the rebase starts from the very first commit (the Initial commit).
``` 
git rebase -i --root 
```
If any conflicts arise during the rebase, you will need to resolve them, then continue the rebase using:
``` 
git rebase --continue 
```
```
git rebase -i <commit-hash>^ 
``` 
The ^ symbol means "the parent of this commit," so it will include <commit-hash> in the rebase.


## 
To delete a particular commit from your Git history without affecting the remaining commits, you can use an interactive `git rebase or a git reset` depending on the context. 

**Option 1: Using Interactive Rebase (recommended for deleting a commit in the middle of history)**
1. Start an interactive rebase 
```
git rebase -i HEAD~4    # This means you want to rebase the last 4 commits (counting back from the most recent).
```
2. Edit the rebase list
```
pick abc123 Commit message
drop def456 Commit message  # This is the commit you want to delete
pick ghi789 Commit message
pick jkl012 Commit message
```
3. Save and close the editor
4. Resolve conflicts if any: During the rebase, if any conflicts occur due to the removal of the commit, you'll need to resolve them manually. After resolving conflicts, run:
```
git rebase --continue
```
5. Force push to update the remote:
``` 
git push origin main --force 
```
**About the merge conflict and divergence.**

"Before pushing, you may encounter merge conflicts because the commit you dropped could cause divergence in the branch history. After resolving the conflicts and completing the rebase, you will need to force-push the changes to sync the local and remote repositories, ensuring they are the same."

**Explanation** of the Fix:
- Merge Conflict: Conflicts happen because the commit you dropped may have been part of the history that other commits depend on.
- Divergence: Dropping a commit essentially rewrites history, causing a mismatch between the local branch and the remote branch.
- Force Push: After resolving conflicts, a force push (git push origin main --force) is required to overwrite the remote history with your rewritten local history, making both local and remote branches identical.

**To clarify:** You resolve conflicts as part of the rebase process, and once all conflicts are resolved, you continue with the rebase and force-push to sync the history.

**Issue :**

The issue you’re encountering — where certain commits (like a2d5cd7 and ef4abd5) are not showing up in your interactive rebase — is likely due to the fact that those commits are either part of a different branch or are part of a merge history. Git’s rebase process works on the history of the current branch, and if the commits you’re trying to remove are on a different branch or part of a merge commit, they may not appear in the rebase list as expected.

The issue is likely due to a2d5cd7 being part of a different branch or merge history, which is why it's not appearing where you expect.

- Find the commit immediately before a2d5cd7 using git log.
- Start the rebase from that commit.
- Remove the unwanted commits in the interactive rebase editor, then force push to the remote.

When you force-push (git push --force), you are essentially overwriting the history of the remote branch with your local branch history. This means the changes that exist in your local main branch will replace the history on the remote main branch.


### What is an Empty Commit?
An empty commit is a commit that doesn't make any actual changes to the code or content compared to the parent commit. This can happen in the following cases:

- Rebasing a commit that doesn't have any changes: 
   If the changes in a commit were already applied in earlier commits or if the changes were completely overwritten in a subsequent commit, the rebase may produce an empty commit.

- A conflict that isn't resolved: 
   If you run git rebase --continue after resolving the conflict (but you didn't actually make any changes), Git may create an empty commit because the rebase doesn't have any new changes to apply after resolving the conflict.

**Conclusion**

The "empty commit" you see during the rebase likely occurs because there was no actual change to apply after the conflict resolution. When you resolve a conflict, if the content ends up the same as it was before (e.g., you deleted `README.md` in the current branch and kept it deleted in the conflicting commit), the commit will be considered "empty."

To avoid this, always ensure that there's a meaningful change in each commit during a rebase, and consider using squash or fixup to combine commits that don't make any changes.

**Option 2: Using git reset (only works if you want to delete the most recent commit)**
1. 
``` 
git reset --hard HEAD~1 
```
2. 
``` 
git push origin main --force 
```


Backup: It’s a good idea to create a backup branch before performing destructive operations like force pushing.
``` 
git checkout -b backup-branch 
```

Always create a backup branch (git checkout -b backup-branch) before performing a rebase or force push, just in case anything goes wrong.

## NOTE :

## Clear Cached Credentials (if necessary)

**On Windows**:
1. Open **Windows Credential Manager** (Control Panel > User Accounts > Credential Manager).
2. Find stored GitHub credentials (they'll look like `git:https://github.com`).
3. Remove the stored credentials to resolve authentication issues.

Open the Windows Credential Manager:

1. Press Windows + R to open the Run dialog.
2. Type control keymgr.dll and press Enter to open Credential Manager.


## Use Personal Access Token (PAT) for Authentication
GitHub no longer supports password authentication for HTTPS. Instead, use a Personal Access Token (PAT).

**Generate a PAT**:
1. Go to [GitHub's PAT page](https://github.com/settings/tokens).
2. Generate a token with **repo** and **workflow** permissions (these are generally sufficient for most uses).

Once generated, use this PAT in place of your password when prompted by Git.

## 
- When pushing to the remote for the first time, you can use -u to set the upstream branch:
  ```
  git push -u origin main                         # Push and set 'main' as the upstream branch
  ```
- If you want to keep your email private when pushing commits, use GitHub's "noreply" email:
  ```
  git config --global user.email "yourusername@users.noreply.github.com"
  ```
- Ensure your local Git settings match your intended use to avoid pushing to the wrong account or repo.
- If you want to permanently remove a commit from both the remote and local repositories, you will need to delete the commit locally and then force-push the changes to the remote repository. This process rewrites history, so it must be done carefully, especially if other collaborators are working on the same repository.
  ```
  git log --oneline
  git reset --soft <commit-hash>                # To keep the changes in your working directory (but uncommit them)
  git reset --hard <commit-hash>                # To remove the commit and discard the changes (this is permanent)
  git reset --hard HEAD~1                       # To remove the most recent commit

  git push origin <branch-name> --force         #  Force Push to the Remote Repository
  ```
## Git Credential Manager (GCM)

The **GCM** securely manages credentials for Git operations (like push and pull) and integrates with Git for seamless authentication. Here's a quick guide:

1. **Check if GCM is Installed**: Use `git config --global credential.helper` to verify installation. If it's not installed, configure it using `git config --global credential.helper manager-core`.
    
2. **Using GCM**: Once configured, GCM automatically stores and uses credentials when performing Git operations like `git push` or `git pull`.

3. **Manually Adding Credentials**: You can store credentials manually in Windows Credential Manager by adding a new entry for `git:https://github.com` with your GitHub username and token.

4. **Changing/Removing Credentials**: Update or delete credentials in Credential Manager, or use `git credential-manager erase` to remove them.

GCM automates secure credential storage and authentication, reducing the need for repeated logins during Git operations.

