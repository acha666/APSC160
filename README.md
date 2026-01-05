## C/C++ Lab Workspace — Quick Guide (Linux / WSL / macOS)

### 1) Install system dependencies

#### Linux / WSL (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install -y build-essential gdb clangd clang-format
```

#### macOS

Install Xcode command line tools:

```bash
xcode-select --install
```

Install LLVM (clangd / clang-format):

```bash
brew install llvm
```

If VS Code can’t find `clangd` or `clang-format` on macOS, set **User Settings** (not in repo):

- Apple Silicon:

  - `clangd.path`: `/opt/homebrew/opt/llvm/bin/clangd`
  - `clang-format.executable`: `/opt/homebrew/opt/llvm/bin/clang-format`

- Intel:

  - `clangd.path`: `/usr/local/opt/llvm/bin/clangd`
  - `clang-format.executable`: `/usr/local/opt/llvm/bin/clang-format`

---

### 2) Install VS Code extensions

Open the folder/workspace in VS Code → install recommended extensions:

- `ms-vscode.cpptools` (debugger)
- `llvm-vs-code-extensions.vscode-clangd` (language features)
- `xaver.clang-format` (C/C++ formatting)
- `esbenp.prettier-vscode` (Markdown/JSON/YAML formatting)
- (Windows + WSL only) `ms-vscode-remote.remote-wsl`

---

### 3) Project layout (where to put files)

- **Archived (tracked) code** → `archive/`

  - Example: `archive/c/hello.c`, `archive/cpp/hello.cpp`

- **Experiments (NOT tracked by git)** → `experiments/`

  - Put any scratch code here; changes won’t be committed.

- **Runtime working directory** → `run/`

  - Put `in.txt`, `out.txt`, etc. here.
  - Debug/run always uses `cwd = run/`, so you can do:

    ```c
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
    ```

- **Build outputs (auto-generated)** → `build/`

  - Outputs are separated by source path to avoid name collisions:

    - `archive/c/hello.c` → `build/archive/c/hello`
    - `experiments/hello.c` → `build/experiments/hello`

---

### 4) Build / Run / Debug

1. Open any `.c` or `.cpp` file.
2. Press **F5**:

   - Automatically detects `.c` vs `.cpp`
   - Builds into `build/<relative_path>/<name>`
   - Launches debugger with `cwd = run/`

Optional (Terminal tasks):

- Use VS Code Command Palette → **Tasks: Run Task**

  - `run: active (cwd=run)` (build + run from `run/`)

---

### 5) Formatting

- **C/C++**: `clang-format` (uses repo `.clang-format`)
- **Markdown/JSON/YAML**: Prettier (uses repo `.prettierrc.json`)

Format current file:

- **Format Document** (Shift+Alt+F / Shift+Option+F)

---

### 6) Git behavior (important)

- `archive/` is tracked normally.
- `experiments/` is ignored (except `README.md` / `.gitkeep`).
- `run/` contents are ignored (except `README.md` / `.gitkeep`).
- `build/` is ignored.
