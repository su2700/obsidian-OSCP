
## 安装 pipx（如果还没安装）

`python3 -m pip install --user pipx python3 -m pipx ensurepath`

> ⚠️ 运行 `python3 -m pipx ensurepath` 后，通常需要 **重开一个 shell** 或执行：

`export PATH="$HOME/.local/bin:$PATH"`

确保 `pipx` 命令可用。

---

## 2️⃣ 用 pipx 安装 Certipy

`pipx install certipy`

> pipx 会自动创建隔离环境并安装 Certipy，使它可全局运行而不污染系统 Python。