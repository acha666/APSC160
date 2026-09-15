## C/C++ Lab Workspace — Quick Guide (Linux / WSL / macOS)

This workspace is designed for **standalone C/C++ exercises**. Each source
file is expected to compile independently into its own executable.

Multi-file programs that require linking several `.c`/`.cpp` files together
should use a project-level build system such as Make or CMake instead.

### 1) Install system dependencies

#### Linux / WSL (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install -y build-essential gdb clangd
```

#### macOS

Install Xcode command line tools:

```bash
xcode-select --install
```

Install LLVM (clangd):

```bash
brew install llvm
```

If VS Code can’t find `clangd` on macOS, set **User Settings** (not in repo):

- Apple Silicon:
  - `clangd.path`: `/opt/homebrew/opt/llvm/bin/clangd`

- Intel:
  - `clangd.path`: `/usr/local/opt/llvm/bin/clangd`

---

### 2) Install VS Code extensions

Open the folder/workspace in VS Code → install recommended extensions:

- `ms-vscode.cpptools` (GDB debugger on Linux / WSL)
- `llvm-vs-code-extensions.vscode-clangd` (language features)
- `vadimcn.vscode-lldb` (LLDB debugger on macOS)
- `esbenp.prettier-vscode` (Markdown/JSON/YAML formatting)
- (Windows + WSL only) `ms-vscode-remote.remote-wsl`

---

### 3) Project layout (where to put files)

- **Archived code** → `archive/`
  - Example: `archive/helloworld.c`

- **Current workspace (NOT tracked)** → `workspace/`
  - Put your scratch/working code here; changes won't be committed.
  - Example: `workspace/main.c`

- **Runtime working directory** → `run/`
  - Put `in.txt`, `out.txt`, etc. here.
  - Debug/run always uses `cwd = run/`, so you can do:

    ```c
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
    ```

- **Build outputs (auto-generated)** → `build/`
  - Outputs are separated by source path to avoid name collisions:
    - `archive/helloworld.c` → `build/archive/helloworld`
    - `workspace/main.c` → `build/workspace/main`

---

### 4) Build / Run / Debug

#### Debugging (with breakpoints)

1. Open any `.c` or `.cpp` file.
2. Press **F5** (or Run → Start Debugging):
   - Automatically runs the `build` task with debug symbols (`-g -O0`)
   - Linux / WSL uses GDB; macOS uses CodeLLDB
   - Launches the debugger with `cwd = run/`
   - Detects `.c` vs `.cpp` automatically
   - Output: `build/<relative_path>/<name>`

Use this when you want to:

- Set breakpoints and step through code
- Inspect variables at runtime
- Debug crashes or logic errors

#### Running (without debugger)

**Option 1: Quick run (Terminal task)**

- Command Palette → **Tasks: Run Task** → `run`
  - Builds with debug symbols and runs from `run/` directory
  - Faster startup than debugger
  - Shows program output directly in terminal

**Option 2: Build manually**

```bash
# From workspace root
./build/workspace/main
# or with specific working directory
cd run && ../build/workspace/main
```

Use running (not debugging) when you want to:

- Test program behavior without breakpoints
- See clean console output
- Quickly verify functionality

---

### 5) Formatting

- **C/C++**: clangd's built-in formatter (uses repo `.clang-format`)
- **Markdown/JSON/YAML**: Prettier (uses repo `.prettierrc.json`)

Format current file:

- **Format Document** (Shift+Alt+F / Shift+Option+F)

---

### 6) Git behavior (important)

- `archive/` is tracked normally.
- `workspace/` contents are ignored (except `.gitkeep`) — for scratch work.
- `run/` contents are ignored (except `.gitkeep`).
- `build/` is ignored.
