# Python port: setup log

*Everything installed for the Python editions of the S Shape Functions notebooks, from beginning to end, on macOS Tahoe and in the cloud, including the mistakes and how they were fixed*

**Noor Hoshino** · نور 星野  
AI System Programming Group  
Hokkaido Prefecture, Japan  
*noor@compclassnotes.com*  
Setup carried out with AI assistance on 16 November 2026  
Version 1.0.0 · 16 November 2026  

*© Computational Classnotes · CC BY-NC-SA 4.0*

---

## What was installed, and where

The work used three places. Each has its own copy of Python and the libraries, and none of them changes the others.

| Place | Machine | What it holds |
|---|---|---|
| The cloud workspace | Linux, x86_64, in the cloud | where the notebooks were written, run and checked |
| The Mac | macOS 26 Tahoe, Apple silicon (arm64), user `darashayda` | a copy of the repository in `~/Documents/GitHub/neural-networks-things`, with uv, Python and the libraries |
| GitHub | repository `lossofgenerality/neural-networks-things` | the notebooks and setup files; GitHub Codespaces builds its own copy of the environment from them |

## Versions

| Component | Version | Notes |
|---|---|---|
| uv (Astral) | 0.13.8 on the Mac; 0.11.32 in the cloud workspace | Python versions, environments and libraries |
| Python (CPython, managed by uv) | 3.14.10 on the Mac; 3.14.6 in the cloud workspace | `.python-version` asks for 3.14, and uv takes the newest 3.14 it has |
| NumPy | 2.5.5 | |
| SciPy | 1.18.1 | |
| SymPy | 1.14.0 | |
| Matplotlib | 3.11.2 | |
| NetworkX | 3.7 | |
| JupyterLab | 4.6.6 | |
| ipykernel / ipywidgets | 7.4.0 / 8.1.9 | |
| Pillow / requests | 13.0.0 / 2.34.2 | |
| nbformat / nbconvert | 5.11.1 / 7.17.2 | building and running the notebooks without a browser |
| PyTorch | 2.15.0 | only for Problem 5 of the exercises |

All library versions are fixed in `uv.lock`, so every place gets the same ones. The lock has 132 packages for all platforms; the Mac needed 110 of them.

## Step by step

### 1. The plan

The plan (`Python-Port-Plan.md`, kept in the project "Python Ports and Notebooks") chose Python 3.14, uv, JupyterLab and the libraries above. Python 3.15.0 had come out on 9 October 2026, but PyTorch may lag behind a new Python, so 3.14 was chosen; moving up later is a change of one line.

### 2. The cloud workspace: the repository and the environment

```
git clone https://github.com/lossofgenerality/neural-networks-things.git repo
uv python install 3.14
uv init --python 3.14 s-shape-functions
cd s-shape-functions
uv add numpy scipy sympy matplotlib networkx jupyterlab ipywidgets pillow requests nbformat nbconvert ipykernel
uv add torch
```

### 3. Writing and running the notebooks

The Wolfram cells were rewritten in Python, one file of cells per notebook. A small script took the text and equations from the Markdown editions, put each Python cell where its Wolfram cell had been (matched by the index `c.N`), and wrote the `.ipynb`. Each notebook was then run from the top without a browser:

```
uv run jupyter nbconvert --to notebook --execute --inplace S-Shape-Functions-Theory.ipynb
```

Every table and value was compared with the Wolfram results, and every figure was looked at and compared with the Wolfram figure.

### 4. A clean test

The repository was copied to a new folder, the environment was built there from `pyproject.toml` and `uv.lock` alone (`uv sync`), and both notebooks were run again. Every output was identical, including the values that depend on random numbers and the PyTorch results.

### 5. The Mac: files

The assistant's desktop app links the session to the Mac. The assistant asked for access to `~/Documents` (approved), found that `~/Documents/GitHub` had no copy of the repository, and created `~/Documents/GitHub/neural-networks-things` from a zip of the whole repository.

It added a double-clickable script, `Run Notebooks.command`:

```bash
#!/bin/bash
cd "$(dirname "$0")"
export PATH="$HOME/.local/bin:$PATH"
if ! command -v uv >/dev/null 2>&1; then
  echo "Installing uv ..."
  curl -LsSf https://astral.sh/uv/install.sh | sh
  export PATH="$HOME/.local/bin:$PATH"
fi
uv sync
uv run jupyter lab s-shape-functions-theory/S-Shape-Functions-Theory.ipynb
```

### 6. The Mac: running the script

The assistant opened the folder in Finder with computer use and opened `Run Notebooks.command` with Finder's File → Open. Terminal then showed:

```
Installing uv ...
downloading uv 0.13.8 aarch64-apple-darwin
installing to /Users/darashayda/.local/bin
  uv
  uvx
everything's installed!
Using CPython 3.14.10
Creating virtual environment at: .venv
Resolved 132 packages in 1ms
Prepared 110 packages in 3.91s
Installed 110 packages in 387ms
```

and JupyterLab started at `http://localhost:8888/lab` and opened the main notebook in the browser.

On the Mac this put:

- `uv` and `uvx` in `~/.local/bin`;
- Python 3.14.10 in `~/.local/share/uv/python/`;
- the libraries in `.venv` inside the repository folder;
- downloaded packages in uv's cache, `~/.cache/uv`.

The uv installer may also add `~/.local/bin` to the PATH in the shell profile. Nothing else was changed: not the macOS system Python, Homebrew or Mathematica.

### 7. GitHub

With the user's approval, two commits went to `main`:

1. the two notebooks, `pyproject.toml`, `uv.lock`, `.python-version`, `.devcontainer/devcontainer.json` (the Codespaces setup) and the updated README;
2. the copyright, licence and front matter made the same as in the Wolfram notebooks.

The Codespaces setup starts from Ubuntu 24.04, installs uv, runs `uv sync`, adds the Python and Jupyter extensions, and opens the main notebook.

## Mistakes and problems, and what fixed them

| # | What went wrong | Why | Fix |
|---|---|---|---|
| 1 | The CPU-only PyTorch could not be downloaded in the cloud workspace | the workspace's network allows only some hosts, and `download.pytorch.org` is not one of them | PyTorch was installed from PyPI. On Linux this version includes the CUDA libraries, so Linux installs, Codespaces included, download a few GB more than needed. The Mac gets the normal Apple-silicon wheel and is not affected |
| 2 | `uv init` gave the project the folder's name, a placeholder description and a `readme` entry | `uv init` defaults | renamed to `neural-networks-things`, description written, `readme` removed, `uv lock` rerun |
| 3 | A rough draft of the Cantor-function code was left in cell c.21 | editing slip | removed before the first run |
| 4 | Cell c.0.4 failed: `legend() got multiple values for keyword argument 'bbox_to_anchor'` | the helper `legend_below` always set the legend position, and c.0.4 set it again | the helper sets it only when it is not given |
| 5 | The build script failed after a text edit | HTML attributes in double quotes inside a Python string in double quotes | single quotes in the HTML |
| 6 | Some figures came out wrong: Figure 0.4 (legend over the axis label), Figure 3 (weight labels at the wrong end of the arrows), Figure 8 (nodes out of order), Figure 7 (title over the panel titles), Figures 1.1, 4.1, 4.3 and 5.3 (crowded or missing labels, ragged surfaces) | found by looking at every figure. Figure 3: NetworkX measures `label_pos` from the other end of the arrow than Wolfram's `Placed` | fixed one by one and compared again with the Wolfram figures |
| 7 | Table 3 of the main notebook disagreed in two places with the Wolfram Markdown edition (N = 80, C and D) | the Markdown edition and the PDF have two old values; a fresh Wolfram run gave the Python values digit for digit | the Python edition uses the correct values (0.061 and 0.046) |
| 8 | Values from random weights differ from the Wolfram run (section 5; Problems 3, 5 and 6) | NumPy and PyTorch have their own random-number generators and seeds | the text of the Python editions gives the values of the Python run; the conclusions are unchanged |
| 9 | The revision lines first read "revision 7 · Python edition 1" | a change that should not have been made | put back to exactly "revision 7" and "revision 1", as in the Wolfram notebooks |
| 10 | The assistant's shell on the Mac could not install uv for macOS | that shell runs in a Linux sandbox on the Mac and can only reach the connected folders; it is not a macOS shell | the double-clickable script, run by macOS itself |
| 11 | Computer use first refused to start | the assistant's desktop app did not yet have macOS Accessibility and Screen Recording permission | the user granted it in System Settings |
| 12 | The assistant could not type into Terminal | for safety, Terminal is granted to the assistant at "click" level only: clicks, no typing | the script does the typing; the assistant only opens it |
| 13 | In Finder, the Return key could not be sent to Go → Go to Folder, and double-clicking the script's name selected the name for renaming | keys and double-clicks act differently when Finder is not the frontmost app | double-clicked the folder in the suggestion list, then selected the script's icon and used File → Open |
| 14 | Updating the Mac copy with `unzip` failed: `cannot delete old README.md` | deleting files in the connected folder is not allowed unless the user permits it, and `unzip` replaces a file by deleting it | the files were overwritten in place instead. The zips used for copying are in `_to_delete/` for the user to delete |
| 15 | The link `http://localhost:8888/lab/tree/s-shape-functions-theory/...` was the wrong guess | when JupyterLab is started with a notebook file, its top folder is that file's folder | `http://localhost:8888/lab/tree/S-Shape-Functions-Theory.ipynb` |
| 16 | `NameError: name 'commonsImage' is not defined` | a cell was run before c.0, which defines the helpers | Kernel → Restart Kernel and Run All Cells |
| 17 | The Codespaces setup was not started from the cloud workspace | a codespace cannot be started from there | checked instead that the configuration is valid and that the same `uv sync` works on a clean Linux machine; the first real start is the final test |

## How to undo

On the Mac:

```
rm -rf ~/Documents/GitHub/neural-networks-things/.venv   # the libraries
uv cache clean                                            # downloaded packages
rm -rf ~/.local/share/uv                                  # Python 3.14 and uv's data
rm ~/.local/bin/uv ~/.local/bin/uvx                       # uv itself
```

and remove the `~/.local/bin` line from the shell profile (`~/.zshrc` or `~/.zprofile`) if the installer added one. Deleting the folder `~/Documents/GitHub/neural-networks-things` removes the copy of the repository.

On GitHub, either commit can be reverted from its page.

---

<p align="center" style="color:#666666"><i>© Computational Classnotes · CC BY-NC-SA 4.0</i></p>
