# Git cheat sheet

## Passwords and authentication

Tell git to remember the password in future pushes during 3600 seconds:
```sh
$ git config --global credential.helper cache
$ git config --global credential.helper 'cache --timeout=3600'
```
