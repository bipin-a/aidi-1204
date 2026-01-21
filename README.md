# Getting started with Python using `uv` (macOS and Windows)

Audience: complete beginners who are new to programming.

Goal: install Python 3.10.16, create a virtual environment, and run `uv init` and `uv add jupyterlab` in a project folder.

## Table of contents

- [0. Prerequisites](#0-prerequisites)
- [What is a terminal and a shell?](#what-is-a-terminal-and-a-shell)
- [Install and open a terminal](#install-and-open-a-terminal)
- [Install Visual Studio Code (VS Code)](#install-visual-studio-code-vs-code)
- [Install GitHub Desktop](#install-github-desktop)
- [1. Install `uv`](#1-install-uv)
- [What does "adding to PATH" mean?](#what-does-adding-to-path-mean)
- [2. Install Python 3.10.16](#2-install-python-31016)
- [3. Create a project folder](#3-create-a-project-folder)
- [4. Pin the Python version for this project](#4-pin-the-python-version-for-this-project)
- [5. Initialize the project](#5-initialize-the-project)
- [6. Create and activate a virtual environment](#6-create-and-activate-a-virtual-environment)
- [7. Install JupyterLab](#7-install-jupyterlab)
- [Local packages and why we install them](#local-packages-and-why-we-install-them)
- [Common pitfalls](#common-pitfalls)
- [Quick checklist (final goal)](#quick-checklist-final-goal)

## 0. Prerequisites

You need three tools before installing Python:

1. A terminal
2. Visual Studio Code (VS Code)
3. GitHub Desktop

### What is a terminal and a shell?

- A **terminal** is the app where you type commands.
- A **shell** is the program inside the terminal that reads your commands.
  - macOS uses a shell like `zsh`.
  - Windows uses `PowerShell`.

Where to paste commands:
- macOS: paste commands into the Terminal app.
- Windows: paste commands into PowerShell (not Command Prompt).

### Install and open a terminal

macOS:
- Press Command + Space, type `Terminal`, press Enter.

Windows:
- Press the Windows key, type `PowerShell`, press Enter.
- Do not use Command Prompt.
- If PowerShell blocks scripts later, run this once:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```

### Install Visual Studio Code (VS Code)

Why: VS Code is where you will write and run Python code.

macOS:
1. Go to https://code.visualstudio.com/
2. Download the macOS version.
3. Open the `.zip` and drag `Visual Studio Code.app` to Applications.
4. Open VS Code from Applications.

Windows:
1. Go to https://code.visualstudio.com/
2. Download the Windows installer.
3. Run the installer and check "Add to PATH".
4. Open VS Code from the Start menu.

### Install GitHub Desktop

Why: GitHub Desktop helps you save and share your code.

macOS:
1. Go to https://desktop.github.com/
2. Download for macOS.
3. Open the `.zip` and drag `GitHub Desktop.app` to Applications.
4. Open GitHub Desktop and sign in or create an account.

Windows:
1. Go to https://desktop.github.com/
2. Download for Windows.
3. Run the installer.
4. Open GitHub Desktop and sign in or create an account.

## 1. Install `uv`

Why: `uv` installs Python and manages project packages. For this course, use `uv` (not `pip`) for local setup.

Official instructions:
- https://docs.astral.sh/uv/getting-started/installation/

macOS (paste into Terminal):
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Windows (paste into PowerShell):
```powershell
irm https://astral.sh/uv/install.ps1 | iex
```

After installation, close and reopen your terminal, then check:
```bash
uv --version
```

## What does "adding to PATH" mean?

Your computer only runs commands stored in folders listed in the PATH. When an installer "adds to PATH", it tells your system where to find `uv`. If you see `uv: command not found`, restart the terminal or re-run the installer.

## 2. Install Python 3.10.16

Why: The course uses Python 3.10.16, so everyone should have the same version.

macOS and Windows (paste into your terminal):
```bash
uv python install 3.10.16
```

Check:
```bash
uv python list
```

## 3. Create a project folder

Why: Each project should live in its own folder.

macOS and Windows:
```bash
mkdir my-python-project
cd my-python-project
```

## 4. Pin the Python version for this project

Why: Pinning makes sure `uv` always uses Python 3.10.16 in this folder.

macOS and Windows:
```bash
uv python pin 3.10.16
```

## 5. Initialize the project

Why: `uv init` creates project files like `pyproject.toml`.

macOS and Windows:
```bash
uv init
```

## 6. Create and activate a virtual environment

Why: A virtual environment keeps this project's packages separate from other projects.

Create it (macOS and Windows):
```bash
uv venv
```

Activate it:

macOS:
```bash
source .venv/bin/activate
```

Windows (PowerShell):
```powershell
.\.venv\Scripts\Activate.ps1
```

If activation worked, your prompt will usually show `(.venv)`.

## 7. Install JupyterLab and core data packages

Why: JupyterLab lets you create notebooks and visualizations.

macOS and Windows (with the venv active):
```bash
uv add jupyterlab
```

Also install common data packages:
```bash
uv add pandas numpy scipy statsmodels
```

Start JupyterLab:
```bash
jupyter lab
```

## Local packages and why we install them

A package is extra functionality for Python. A **local package** is installed inside your project's `.venv` so it does not affect other projects.

- **Use `uv`, not `pip`, for this course**. `uv add <package>` installs the package and records it in `pyproject.toml`.
- `pip install <package>` installs the package in the active venv but does not automatically record it in `pyproject.toml`, so do not use it here.

Where packages are stored:

macOS:
```bash
ls .venv/lib/python3.10/site-packages
```

Windows:
```powershell
Get-ChildItem .venv\Lib\site-packages
```

Check which Python is running:
```bash
python -c "import sys; print(sys.executable)"
```

If the path points to `.venv`, you are using the local environment.

## Common pitfalls

- `uv` not found: restart the terminal or reinstall `uv`.
- Wrong folder: run `pwd` to confirm you are in the project folder.
- Venv not active: look for `(.venv)` in the prompt before running `uv add` or `jupyter`.
- Windows script error: run `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` in PowerShell.
- Wrong Python version: run `uv python list` and confirm 3.10.16 is installed and pinned.

## Quick checklist (final goal)

From your project folder, you should be able to run:
```bash
uv init
uv venv
uv add jupyterlab
uv add pandas numpy scipy statsmodels
```

If these work, you are ready to start running statistical tests and creating visualizations.
