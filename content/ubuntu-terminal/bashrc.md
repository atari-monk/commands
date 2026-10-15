## Editing `.bashrc`

**TL;DR:** Open `~/.bashrc` with a text editor, make changes, save, then `source ~/.bashrc` to apply.

### Steps

1. **Back it up first** (cheap insurance):

   ```bash
   cp ~/.bashrc ~/.bashrc_backup
   ```

2. **Open it** with a simple editor:

   ```bash
   nano ~/.bashrc
   ```

3. **Make your changes** — common ones are aliases and `PATH` additions:

   ```bash
   alias ll='ls -alF'
   export PATH=$PATH:/your/custom/path
   ```

4. **Save & exit:**
   - `nano`: `Ctrl+O` → `Enter` → `Ctrl+X`
   - `vim`: `Esc` → `:wq` → `Enter`

5. **Apply changes to the current terminal:**
   ```bash
   source ~/.bashrc
   ```
   (New terminals pick it up automatically.)

### Notes

- `~/.bashrc` is **per-user**. For all users, edit `/etc/bash.bashrc` with `sudo` — rarely needed, proceed with care.
- This file only applies to the **Bash** shell (Ubuntu's default). Using Zsh? Edit `~/.zshrc` instead.

---
