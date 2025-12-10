### 🧩 1. Create a directory for your personal executables (if you don’t already have one)

`mkdir -p ~/.local/bin`

This folder is commonly used for user-specific executables and is usually in your PATH by default on most modern Linux distros.

---

### 🧲 2. Create a symbolic link to your script

`ln -s /home/incursore/incursore.sh ~/.local/bin/incursore`

> 💡 You can name the link whatever you like (e.g., just `incursore` instead of `incursore.sh` for cleaner commands).

---

### ⚙️ 3. Make sure the script is executable

`chmod +x /home/incursore/incursore.sh`

---

### 🧠 4. Add `~/.local/bin` to your PATH (if it isn’t already)

Check your PATH:

`echo $PATH`

If `~/.local/bin` isn’t listed, add it by editing your shell config file (e.g., `~/.bashrc`, `~/.zshrc`, or similar):

`export PATH="$HOME/.local/bin:$PATH"`

Then reload your shell:

`source ~/.bashrc`

(or `source ~/.zshrc`)