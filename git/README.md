# Git cheat sheet

# Passwords and authentication

Tell git to remember the password in future pushes during 3600 seconds:
```sh
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'
```

Personal shortcuts to configure my github user in EC2 instance:
```sh
git config --global user.name "peferso"
git config --global user.email p.fdez.s.90@gmail.com
git commit --amend --reset-author
```

# Branches

Check in which branch you are working now:
```sh
git status
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

## git fetch

Override existing local repository with GitHub version:
```sh
git fetch origin master
git reset --hard origin/master
```
