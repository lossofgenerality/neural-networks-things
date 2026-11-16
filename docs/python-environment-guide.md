# Running the Python environment: a guide for programmers

*How to run the Python editions on macOS and from GitHub, which tool does what, and when to use each*

**Noor Hoshino** · نور 星野  
Advanced AI System Programming  
Hokkaido Prefecture, Japan  
*noor@compclassnotes.com*  
Written with Claude (Anthropic), 16 November 2026  
Version 1.0.0 · 16 November 2026  

*© Computational Classnotes · CC BY-NC-SA 4.0*

---

## The short version

| You want to | Use | Command or link |
|---|---|---|
| read the notebooks with their outputs | GitHub's notebook viewer | open the `.ipynb` on github.com |
| run them on a Mac | uv + JupyterLab | `uv sync`, then `uv run jupyter lab` (or double-click `Run Notebooks.command`) |
| run them with nothing installed | GitHub Codespaces | the **Open in GitHub Codespaces** button in the README |
| run them without a browser and save the outputs | nbconvert | `uv run jupyter nbconvert --to notebook --execute --inplace NOTEBOOK.ipynb` |
| edit them in an editor | VS Code or VSCodium with the Jupyter extension | open the folder, choose the `.venv` kernel |

## How the pieces fit

```
pyproject.toml   what the project needs: Python >= 3.14 and the libraries
.python-version  which Python uv uses: 3.14
uv.lock          the exact version of every package, for every platform
      │
      ▼  uv sync
.venv/           Python 3.14 and the libraries, inside the repository folder
      │
      ▼  uv run jupyter lab      (or a codespace, or VS Code)
the notebooks    each one runs in a kernel, the Python process from .venv
```

The three files at the top are in the repository. `.venv` is not: each machine builds its own from them, and `uv.lock` makes every build the same.

## The tools

| Tool | What it does | Use it when | Licence |
|---|---|---|---|
| **uv** (Astral) | installs Python versions, creates `.venv`, installs exactly the versions in `uv.lock`, runs commands inside the environment | always: it is the one tool the setup needs | MIT / Apache 2.0 |
| **Python 3.14** (installed by uv) | runs the code | always; uv keeps it apart from the macOS and Homebrew Pythons | PSF |
| **JupyterLab** | notebooks in the browser, closest to a Wolfram notebook | running and reading the notebooks on your own machine | BSD |
| **ipykernel** | the Python process behind a notebook | always; it comes with the environment | BSD |
| **ipywidgets** | sliders and other controls (Figure 2) | only in a running notebook; GitHub and PDFs show a static picture | BSD |
| **nbconvert** | runs a notebook without a browser, converts it to HTML or PDF | checking that a notebook runs from the top, saving outputs before a commit, scripts and CI | BSD |
| **VS Code** / **VSCodium** + Jupyter extension | notebooks inside an editor, with debugging and git | editing code cells, larger changes; VSCodium is the build without Microsoft telemetry (extensions from Open VSX) | MIT |
| **GitHub Codespaces** | a Linux machine in the cloud, built from `.devcontainer/devcontainer.json`, with VS Code in the browser | running the notebooks on any computer, or letting a reader run them, with nothing installed | service; free monthly allowance for personal accounts |
| **Git** | version history, upload to GitHub | when you change the notebooks; the GitHub website's upload works too | GPL |
| **PyTorch** | the recurrent layer `nn.RNN` | Problem 5 only | BSD |

Tools you do **not** need: Homebrew's Python, the macOS system Python, conda or Miniforge, Docker. Miniforge (conda) is the better choice only if a project needs many non-Python scientific libraries; here uv is enough.

## On macOS

### First time

1. Install uv. In Terminal:
   ```
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
   Open a new Terminal window afterwards, so that `~/.local/bin` is on the PATH. Homebrew users can run `brew install uv` instead.
2. Get the repository: `git clone https://github.com/lossofgenerality/neural-networks-things.git`, or Code → Download ZIP on GitHub.
3. In the repository folder:
   ```
   uv sync
   ```
   This installs Python 3.14 (if needed) and the libraries into `.venv`. The first time takes a few minutes, mostly for PyTorch.

### Every time

```
cd neural-networks-things
uv run jupyter lab
```

or double-click `Run Notebooks.command`, which also does the first-time steps when they are needed. Open a notebook and choose **Kernel → Restart Kernel and Run All Cells**. To stop JupyterLab, press Ctrl-C twice in Terminal, or close its window.

`uv run` always uses the project's `.venv`, so there is nothing to activate. If you prefer an activated shell: `source .venv/bin/activate`.

### macOS specifics

- **Gatekeeper and `.command` files.** A `.command` file that was downloaded from the internet may be blocked ("cannot be opened because it is from an unidentified developer"). Open it once with Control-click → Open, or remove the quarantine flag: `xattr -d com.apple.quarantine "Run Notebooks.command"`. A file created on the Mac itself is not blocked.
- **`uv: command not found`.** The installer put uv in `~/.local/bin`; open a new Terminal window, or run `source $HOME/.local/bin/env`.
- **Apple silicon.** PyPI has native arm64 wheels for every library here, PyTorch included. The notebooks run on the CPU; PyTorch's `mps` device (the Apple GPU) is not used and not needed.
- **Port 8888 is busy** (another JupyterLab is running): JupyterLab takes 8889 and prints the new address in Terminal.

## From GitHub

| Service | Cost | Setup in this repository | Notes |
|---|---|---|---|
| **GitHub's notebook viewer** | free | none | shows saved outputs, no running; equations and figures display, the slider does not |
| **GitHub Codespaces** | free monthly allowance for personal accounts; more is billed only above a spending limit you set | `.devcontainer/devcontainer.json` | uses `uv.lock` exactly; the first start takes a few minutes |
| **Google Colab** | free tier | none | runs on Google's own Python and library versions and ignores `uv.lock`; open `https://colab.research.google.com/github/lossofgenerality/neural-networks-things/blob/main/s-shape-functions-theory/S-Shape-Functions-Theory.ipynb`. Not tested with these notebooks |
| **Binder** (mybinder.org) | free, run by volunteers | not set up | often busy, 1–2 GB of memory; it would need its own environment file |

### GitHub Codespaces

1. On the repository page, click **Open in GitHub Codespaces** in the README, then **Create codespace**.
2. GitHub starts Ubuntu 24.04, installs uv, runs `uv sync` and opens the main notebook in VS Code in the browser. The first start takes a few minutes: on Linux, the PyTorch wheel from PyPI brings its CUDA libraries, a few GB that these notebooks do not use.
3. Click **Run All** at the top of the notebook. If asked for a kernel, choose **Python Environments → .venv**.
4. When done: github.com/codespaces → ··· → **Stop codespace**. A stopped codespace keeps its files and does not use compute time; a deleted one frees its storage too.

Keep the Codespaces spending limit at $0 (Settings → Billing) so that use beyond the free allowance is refused, not charged.

## Common tasks

| Task | Command |
|---|---|
| add a library | `uv add LIBRARY` (updates `pyproject.toml` and `uv.lock`; commit both) |
| update all libraries | `uv lock --upgrade`, then `uv sync` |
| install exactly what is locked, nothing else | `uv sync --frozen` |
| move to Python 3.15 | write `3.15` in `.python-version`, set `requires-python = ">=3.15"` in `pyproject.toml`, then `uv sync`. First check that PyTorch has wheels for 3.15 |
| run every notebook from the top and keep the outputs | `uv run jupyter nbconvert --to notebook --execute --inplace s-shape-functions-theory/*.ipynb` |
| export to HTML | `uv run jupyter nbconvert --to html NOTEBOOK.ipynb` |
| remove all outputs before a commit | `uv run jupyter nbconvert --clear-output --inplace NOTEBOOK.ipynb` (the repository keeps outputs, so that GitHub shows them) |
| see which Python runs | `uv run python -c "import sys; print(sys.version)"` |
| update uv itself | `uv self update` |

## Troubleshooting

| Message | Cause | Fix |
|---|---|---|
| `NameError: name '...' is not defined` | a cell was run before c.0 (and c.0.1 in the exercises), which define the settings and helpers | Kernel → Restart Kernel and Run All Cells |
| `ModuleNotFoundError` | the notebook is running on a Python other than `.venv` | in JupyterLab, start it with `uv run jupyter lab`; in VS Code, choose the `.venv` kernel |
| no kernel, or a kernel without the libraries, in VS Code | VS Code picked another Python | Select Kernel → Python Environments → `.venv`; run `uv sync` first if `.venv` is missing |
| the photo cells show "(photo not available offline)" | no internet and no `photos/` folder next to the notebook | run the notebook from inside `s-shape-functions-theory/` |
| JupyterLab says the file changed on disk | the `.ipynb` was replaced while it was open | File → Reload Notebook from Disk, unless you want to keep your unsaved changes |
| values differ slightly from the Wolfram notebooks | random numbers (NumPy, PyTorch) | expected; the text of the Python editions gives the Python values |

## Removing everything

```
rm -rf .venv                                  # in the repository folder
uv cache clean
rm -rf ~/.local/share/uv                      # uv's Pythons and data
rm ~/.local/bin/uv ~/.local/bin/uvx
```

Then remove the `~/.local/bin` line that the installer may have added to `~/.zshrc` or `~/.zprofile`.

See also [the setup log](python-setup-log.md) for what was installed for this repository and the problems met on the way.

---

<p align="center" style="color:#666666"><i>© Computational Classnotes · CC BY-NC-SA 4.0</i></p>
