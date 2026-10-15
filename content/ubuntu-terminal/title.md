## Set terminal tab title

### 1. Change the title from the terminal

Try this command:

```bash
printf '\033]0;My Custom Title\007'
```

For example:

```bash
printf '\033]0;Backend Server\007'
```

This sets the terminal window title and, in many terminal emulators, the tab title too.

If you want to set **only the tab title**, use:

```bash
printf '\033]1;Backend Server\007'
```

These commands use terminal escape sequences, which are supported by many popular terminal emulators.

### 2. Set a title using a shell function

If you frequently work with multiple projects, you can create a reusable function.

Add this to your `~/.bashrc`:

```bash
set-title() {
    printf '\033]0;%s\007' "$*"
}
```

Reload your configuration:

```bash
source ~/.bashrc
```

Now you can set a title like this:

```bash
set-title Backend Server
```

Or:

```bash
set-title Frontend Dev
```

## 3. Set a title automatically when opening a terminal

You can also set the title when starting a new shell by adding this to `~/.bashrc`:

```bash
printf '\033]0;Development\007'
```

Every new interactive Bash session will then start with that title, unless another configuration or application overrides it.

**Note:** Some terminal emulators automatically change the title to the current command or directory. If your title keeps changing, your shell configuration or terminal settings may be responsible.
