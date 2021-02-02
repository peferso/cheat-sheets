# Git cheat sheet

## Passwords and authentication

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
