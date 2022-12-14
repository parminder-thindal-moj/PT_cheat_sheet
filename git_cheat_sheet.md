### git Commands

* `git remote show origin` - check which repo you are pointing at
* `git add <filename>` - add a file to your commit - https://git-scm.com/docs/git-add
* `git reset` - Undo staging changes to commit 
* `git log --name-status` - see last commit items
* `git commit -m <commit_message>` - stage commit
* `git commit <filename> -m <commit_message>` - add and commit a change in 1 line
* `git push` - push commit
* `git checkout <filename>` - revert to upstream
* `git checkout -b <branch_name>` - checkout a branch 
* `git switch <branch_name>` switch to a local OR remote a branch
* `git checkout -b <branch_name>` - create and checkout on branch

#### git branches:
* `git branch` check which branch you are on
* `git branch -v -a` - view all branchs (including remote)
* `git branch <branch_name>` - create a branch
* `git branch -m <new_branch_name>` - rename local git branch
* `git branch --all` - show all branches - local and remote
* `git checkout -b <new_branch_name> <old_branch>` - create a branch, by branching off another branch


* `git checkout -D <branch_name>` - delete a branch locally.
* `git remote prune origin` - deletes all stale remote tracking branches. These branches have already been removed from the remote repo, but are still available locally. Add `--dry-run` to report what branches will be prune, without actually pruning them.


#### git merges:
First:
  - `git checkout -b <your_branch>` - move to the branch you want to MERGE changes INTO to.
  - `git pull origin` - Update your local branch with the remote branch.
  
Then:
  - `git merge <other_branch>` - merge changes from a different branch into the current branch.

For git error: "_fatal: refusing to merge unrelated histories_"

- `git pull origin <branchname> --allow-unrelated-histories` 

OR:
- `git merge <other_branch> --allow-unrelated-histories`


#### Merging individual files :O

* `git checkout --patch <branch_to_merge_from> <PATH_OF_FILE_1> <PATH_OF_FILE_2>` - Merge a single or multiple files from one branch into another.
  Options available after running this command:
    y : Yes, implement all the changes shown
    n : No, I want to keep the file as it is
    e : Edit, I wish to pick which changes to keep

NUCLEAR OPTION - [docs](https://git-scm.com/docs/git-merge-file)
* `git merge-file <our_current_file> <their_file> <base_file>` - Run a 3 way file merge


#### git push/pulls:
* `git push -u origin my_branch_name` - tell github that you have made a new branch - otherwise it will fail when attempting to push
* `git push origin --delete remoteBranchName` - delete branch remotely (and locally)
* `git reset --hard origin/branch_to_overwrite` - hard reset if something breaks

* `git reset --hard && git pull` - reset and pull
* `git pull --set-upstream` - pull the live version down


For Error: - "_fatal: refusing to merge unrelated histories_"
* `git pull origin master --allow-unrelated-histories` - 

* `git checkout <new_branch_name> <hash_key>(commit number)` - restore a previously deleted branch that has been pushed and committed to GitHub
* `git checkout <hash_key^ <folder_path>/>file>` - restore a previously deleted file that has been commited and pushed to Github 

----

* `git log` - check the commit history of a repo in the terminal.
* `git clone <github ssh url>` clone a copy of a repo in current directory


##### git pull whilst ignoring local changes
* `git reset --hard`
* `git pull`

### Git Refs 

- [git refs stackoverflow](https://stackoverflow.com/questions/26046698/git-refname-origin-master-is-ambiguous/26047558#26047558)
* `git show-ref origin/main` -- displayed references in a local repository along with the associated commit ID's.
* `git update-ref -d refs/heads/origin/master` - delete the refs of a local repository


### Git Stats

* `git shortlog -s -n` -- see total commits broken down by users
* `brew install git-extras` -> `git summary --line` -- see total line changes in git history

### Git config and SSH

[More on git config here](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

[SSH key platform notes all here](https://user-guidance.services.alpha.mojanalytics.xyz/github.html#jupyterlab)


<br>
<hr>


## Working with github

### Cloning

`git clone <YOUR-GIT-REPO-URL>` - git clone a specific repo

`git clone -b <branch-to-clone> <YOUR-GIT-REPO-URL> clone-name` - clone repo with a specific branch

Or in a working example:
`git clone -b write_df_to_s3_fix git@github.com:moj-analytical-services/s3tools.git alt-ringfence`


#### Clone - with submodules

`git clone --recursive <YOUR-GIT-REPO-URL>` - clone a repo with the submodules

DON'T FORGET TO INITIALISE AND UPDATE THE SUBMODULE REPO - REMEMBER SUBMODULES ACT LIKE NORMAL GIT REPOS!

PATH TO CURRENT REPO then:

`git submodule init` - initialise the git submodule
`git submodule update` - update the submodule

#### git submodules:

TODO - write submodules stuff

#### updating a git remote URL

A useful command if you decide to change your repository name (_for whatever reason_) and you have already worked on the project before. You will then need to update the remote with the following command:

`git remote set-url origin new.git.url/here` - [Stackoverflow](https://stackoverflow.com/questions/2432764/how-do-i-change-the-uri-url-for-a-remote-git-repository)

<br><br>
#### Branches
`git push origin --delete {the_remote_branch} # Delete branch locally + remotely`
git checkout
<br><br>
#### Syncing up repos (specifically to allow creation of a test repo for an app)
https://www.opentechguides.com/how-to/article/git/177/git-sync-repos.html

Working example of the above:

To get everything setup, initially ensure you cd into your home directory. You can do this by using `cd ..`. If you go too far up the folder branches, you can path back down using `cd my_folder_i_want_to_access`.

If done correctly, your cmd session should end up looking like this: <br>
![Screenshot 2021-08-09 at 14 49 34](https://user-images.githubusercontent.com/45356472/128717070-4cba4063-1e12-488e-aaf0-bde7629220e9.png)

From there, enter the following code into terminal, line by line:
```
git clone --mirror git@github.com:moj-analytical-services/criminal-scenario-tool.git
cd criminal-scenario-tool.git
git remote add --mirror=fetch secondary git@github.com:moj-analytical-services/criminal-scenario-tool-testing.git
git fetch origin
git push secondary --all
```
Once done, ensure your directory stays within `criminal-scenario-tool-testing.git` (or equivalent).

Any updates from the master repo can subsequently be captured running the following lines:
```
git fetch origin
git push secondary --all
```

Please note, any changes to your secondary repo will lead to errors when attempting to run the above lines.
<br><br>
#### Resetting to a previous commit
More here - https://stackoverflow.com/questions/4114095/how-do-i-revert-a-git-repository-to-a-previous-commit

**Start by resetting to your previous commit**
`git reset --hard <commitId> && git clean -f
**Then update the github version**
```
git push origin/master --force
# or
# git push --force
```

<br>
<hr>
