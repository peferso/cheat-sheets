# Screen

A custom .screenrc file

```sh
# File: ~/.screenrc
#

# Deactivate the startup message of screen
startup_message off

# Vim like key bindings for moving around windows
bind l focus right # C-a l goes right
bind h focus left  # C-a h goes left
bind k focus up    # C-a k goes up
bind j focus down  # C-a j goes down

# Bind keys for resizing
bind L resize -h +10  # C-a L increases horizontally by 10
bind H resize -h -10  # C-a H decreases horizontally by 10
bind K resize -v +10  # C-a K increases vertically by 10
bind J resize -v -10  # C-a K decreases vertically by 10

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
