# Screen

A custom .screenrc file (see https://kdrossos.net/blog/15/)

```sh
# File: ~/.screenrc
#

# Deactivate the startup message of screen
startup_message off

# Setup hardstatus
hardstatus off
hardstatus alwayslastline
hardstatus string '%{= .g} %H |%=%{K}%{= w}%?%{K}%-Lw%?%{r}(%{W}%{w}%n%{w}*%f%t%?(%u)%?%{r})%{w}%?%{K}%+Lw%?%= %{g}|%{B} %m-%d  %{W}%c %{g} '

# Fix for residual editor text
altscreen on

# Fix for Name column in windowlist only show "bash"
windowlist string "%4n %h%=%f"

# Indicate 256 color screen
term screen-256color

# Bind Ctrl+A f or F for activating/deactivating hardstatus line
bind f eval "hardstatus ignore"
bind F eval "hardstatus alwayslastline"

# EOF
```
