## Hide `user@host` in Your Bash Prompt (Keep the Path)

**Goal:** Turn this ugly prompt:

```
atari-monk@atari-monk-VivoBook-ASUSLaptop-X512UA:~$
```

into this clean one:

```
~$
```

### The fix

In `~/.bashrc`, find the prompt block and remove the `\u@\h` parts from **both** `PS1` lines (the colored and non-colored branches):

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\w\$ '
fi
unset color_prompt force_color_prompt

case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\w\a\]$PS1"
    ;;
*)
    ;;
esac
```

Then reload:

```bash
source ~/.bashrc
```

### What the escape codes mean

| Code | Meaning                               |
| ---- | ------------------------------------- |
| `\u` | username                              |
| `\h` | hostname                              |
| `\w` | full working directory (`~` for home) |
| `\$` | `$` for normal users, `#` for root    |

Removing `\u@\h` leaves `\w\$` — just the path and the dollar sign. The path stays **blue** because the color codes are untouched.

### Bonus: keep `user@host` for SSH sessions only

If you want the full prompt when connected remotely (a useful visual cue) but a clean one locally:

```bash
if [ -n "$SSH_CONNECTION" ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;34m\]\w\[\033[00m\]\$ '
fi
```

### Gotcha

The `xterm*|rxvt*` block controls the **window/tab title**, not the visible prompt. If you also want the title to drop `user@host`, update it too (done in the example above).
