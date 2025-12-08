## Python Environments for `ai-engineering-lab` (with `uv` and Jupyter)

This project uses [`uv`](https://github.com/astral-sh/uv) to manage Python environments for all AI engineering notebooks. `uv` gives you **clean, isolated, and reproducible** environments that are fast to create and easy to rebuild.

### 1. Why `uv` is used

- **Clean**: All project dependencies live inside a local `.venv` directory instead of your global Python installation.
- **Isolated**: Each project has its own environment, so packages and versions don’t conflict across projects.
- **Reproducible**: Dependencies are pinned via `pyproject.toml` / `uv.lock`, so any machine can recreate the same environment with a single command.
- **Fast**: `uv` uses a highly optimized resolver and installer, making environment creation and sync significantly faster than traditional tools.

You should do **all Python and Jupyter work for this repo via `uv`** so that notebooks run consistently across machines.

---

### 2. Project setup with `uv`

**Prerequisite**: Install `uv` by following the official instructions (typically via a single install script). Once `uv` is on your `PATH`, you can set up the project.

1. **Clone the repository**

   ```bash
   git clone git@github.com:YOUR_ORG/ai-engineering-lab.git
   cd ai-engineering-lab
   ```

2. **Create and sync the environment**

   This will create a local `.venv` in the project directory and install all dependencies declared in `pyproject.toml` / `uv.lock`:

   ```bash
   uv sync
   ```

   After this, the project’s virtual environment is ready and managed entirely by `uv`.

---

### 3. Install Jupyter inside the `uv` environment

Jupyter must be installed **inside** the project environment (not globally) so notebooks use the same dependencies as your code.

From the project root (`ai-engineering-lab`):

```bash
uv add jupyter
```

This adds Jupyter as a dependency and installs it into the `.venv` created by `uv`.

---

### 4. Register a dedicated IPython kernel for this project

To make the environment available as a named kernel inside Jupyter, register it via `ipykernel` using `uv`:

```bash
uv run python -m ipykernel install --user --name ai-eng-lab
```

- **`uv run`** ensures the `python` used is the one in this project’s `.venv`.
- **`--user`** installs the kernel specification into your user-level Jupyter configuration.
- **`--name ai-eng-lab`** is the kernel name you will later select inside Jupyter.

You only need to do this once per machine (or when you change the environment name).

---

### 5. Launch Jupyter using `uv`

Always start Jupyter from within the project directory and via `uv` so that the correct environment is used:

```bash
cd /path/to/ai-engineering-lab
uv run jupyter lab
```

You can also use `uv run jupyter notebook` if you prefer the classic interface, but `jupyter lab` is recommended.

---

### 6. Select the correct kernel inside Jupyter

Once Jupyter Lab is running:

1. Open or create a notebook in the `agents` (or any) directory.
2. In Jupyter Lab, use the **kernel picker**:
   - Click the kernel name in the top-right of the notebook, or
   - Use the menu: **Kernel → Change Kernel…**
3. Select the kernel named **`ai-eng-lab`** (the name you registered earlier).

All notebook cells will now execute using the `uv`-managed `.venv` for this project.

---

### 7. Validate the environment from a notebook (`sys.executable`)

To verify that the notebook is actually using the `uv` virtual environment, run the following in a notebook cell:

```python
import sys
sys.executable
```

You should see a path pointing into the project’s `.venv`, for example something like:

```text
/Users/your-user/Documents/ai-engineering-lab/.venv/bin/python
```

If the path does **not** point into `.venv`, re-check that you selected the `ai-eng-lab` kernel and that you started Jupyter via `uv run` from the project root.

---

### 8. Why this workflow matters (clean macOS, fewer issues, reproducible notebooks)

By using `uv` + a project-local `.venv` + a dedicated Jupyter kernel:

- **Your Mac stays clean**: No need to rely on or modify a global Python installation; everything lives under the project directory.
- **Fewer environment issues**: Notebooks, scripts, and tools all share the same, explicitly managed dependencies.
- **Reproducibility**: Anyone who clones this repo and runs `uv sync` + `uv run jupyter lab` with the `ai-eng-lab` kernel will get the same environment, making AI engineering notebooks easier to debug, review, and share.

Use this setup for all work in `ai-engineering-lab` to ensure reliable, repeatable experimentation.