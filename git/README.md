# Git cheat sheet

# Passwords and authentication

Tell git to remember the password in future pushes during 3600 seconds:
```sh
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'
```

Modify user and email globally:
```sh
git config --system user.name "username"
git config --system user.email "mailexample@mail"
```

Modify user and email for linux user:
```sh
git config --global user.name "globalname"
git config --global user.email "mailexampl@mail"
```

Modify user and email on current git repository:
```sh
git config --local user.name "nameproject"
git config --local user.email "mailexample@mail"
```

Check the user name:
```sh
git config --get user.name
git config --get user.email
```

## Private repositories

An ssh key should locally exists, protected with a password (not sure if the same as the GH account), which public key should be added to the list of known keys on the GitHub account. To create a new ssh key pair locally:

```sh
ssh-keygen -t ed25519 -C "anemail@email.com"
```

Everytime we want to interact with the private repo, there should be an ssh session running and using this key, this can be done with:

```sh
eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_rsa_gh_key
```

It might be useful to setup an auxiliary script such as:

```sh
#!/bin/bash

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_rsa_github_1
```

in some directory (e.g. `~/.utilities/setup_github_ssh_key_1.sh, permissions 744). 

Further, we could add an alias to invoke the script

```sh
cat ~/.bash_aliases

alias setup_gihub_key_1='source ~/.utilities/setup_github_ssh_key_1.sh'
```

# Branches

Check in which branch you are working now:
```sh
git status
git branch
```

Create a new branch and switch to it
```sh
git checkout -b new_branch_name
```

# tags

Checkout to a specific tagged version of the code.

```sh
git checkout tags/v2.1.3
```

## List all branches
```sh
git branch -a
```

## Pull all remote branches
```sh
git pull --all
```

## Create a new branch called "develop"
```sh
git branch develop
```

## Switch to the branch "develop"
```sh
git checkout develop
```

## Create a new branch and switch to it 
```sh
git checkout -b newbranch
```

## Delete a branch locally
```sh
git branch -d branchname
```

## Save workflow

Once you are in the desired branch, change the code and commit the changes 
```sh
git add filechanged.py
git commit -m "description of change"
```

If the changes are correct, then merge with master
```sh
git checkout master
git merge newbranch
```

Delete a branch if not needed
```sh
git branch -d newbranch
```

## Remove commit

Remove the last commit:

```sh
git reset --hard HEAD^
```

Remove the last 2 commits:

```sh
git reset --hard HEAD~2
```

Without `--hard` flag the commits are removed but not the changes in the code.

## git fetch

Download all branches and commits from origin:
```sh
git fetch origin 
```

Download commits and files from specific branch:
```sh
git fetch origin branch_name
```

Download all registered repositories and branches:
```sh
git fetch --all
```

To check the result of the command without actually applying changes:
```sh
git fetch ... --dry-run
```

## Git pull

Checkout changes from master to current branch
```sh
git pull origin master(main)
```

Checkout changes from specific branch to current branch
```sh
git pull origin branch_name
```

## Git add

Add changes to specific files:
```sh
git add pathToFile
```
Add changes to current folder and subfolders
```sh
git add .
```
Add changes to all repository
```sh
git add -A
```

## Git rm

Remove files added with `git add`:
```sh
git rm --cache pathtofile
```
or remove a complete folder
```sh
git rm -rf --cache pathtofolder
```

## Merge a branch to incorporate changes to master

```sh
# Update progress in destination branch master
git checkout master
git pull
# Merge feature branch 
git merge FEATURE_BRANCH_NAME
```

Then, on github create a pull request of the two branches.

## .gitignore

Ignore certain files systematically when adding changes creating a `.gitignore` file and adding the files
```sh
file1
file2
*.log
folder1
folder2/*
!folder2/subfolder_or_file_included
```

## Miscellanea

### Adapt PS1 to show current branch in terminal 

Add this to `.bashrc` or `.profile`...

```sh
if type __git_ps1 &>/dev/null; then
    PS1="$(echo "$PS1" | sed 's/\\\$ $//')"'$( \
        git rev-parse --is-inside-work-tree &>/dev/null && \
        __git_ps1 "\[\033[38;5;250m\][\[\033[38;5;208m\]%s\[\033[38;5;250m\]]\[\033[0m\]" \
    )\$ '
fi
```

***

Return to **[main page](../README.md)**
