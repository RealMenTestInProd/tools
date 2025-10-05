# Python virtual environment (venv) — macOS (zsh)

This README shows how to create, activate, use, and remove a Python virtual environment on macOS using the standard library `venv` module (recommended for most projects). It assumes your shell is zsh.

## Quick contract

- Inputs: macOS machine with Python 3 installed (system or Homebrew). Default shell: `zsh`.
- Output: an isolated virtual environment folder (commonly `.venv` or `venv`) and the ability to install packages with `pip` without affecting the system Python.
- Error modes: Python 2 or missing Python 3; PATH issues; wrong interpreter selected in an editor.

## 1) Check prerequisites

Confirm you have Python 3 available:

```bash
# macOS: check Python 3
python3 --version
# or (if you installed via Homebrew and have `python` mapped)
python --version
```

If Python 3 is not installed, install it with Homebrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install python 
```

After Homebrew installs, `python3` and `pip3` should be available.

## 2) Choose a folder name for your venv

Common names: `.venv`, `venv`, or `env`. Using a dot-prefixed folder like `.venv` keeps it out of `ls` by default and is recommended for many projects.

## 3) Create the virtual environment

Run this from your project root:

```bash
# create a venv in a hidden folder named .venv
python3 -m venv .venv

# or explicitly with a versioned python (if you have multiple):
/usr/local/bin/python3.11 -m venv .venv
```

This creates a directory `.venv/` with an isolated Python and `pip`.

## 4) Activate the virtual environment (zsh)

For zsh (macOS default since Catalina):

```bash
source .venv/bin/activate
# or
. .venv/bin/activate
```

When activated you should see the environment name in your prompt, e.g. `(.venv) $` and `which python` will point inside `.venv`:

```bash
which python
# -> /.../your-project/.venv/bin/python
python --version
```

## 5) Install packages

Use `pip` from within the activated environment:

```bash
pip install requests flask
pip install -r requirements.txt
```

To capture dependencies for the project:

```bash
pip freeze > requirements.txt
```

## 6) Deactivate the environment

```bash
deactivate
```

This returns you to the system Python.

## 7) Remove the virtual environment

If you want to delete it completely:

```bash
rm -rf .venv
```

Note: removing the folder is safe if you don't need the installed packages anymore. Keep `requirements.txt` to recreate the environment later.

## 8) Using different tools (optional)

- pyenv: if you need to manage multiple Python versions, install `pyenv` and use it to install and select specific Python versions, then create a venv from that interpreter. Example:

```bash
brew install pyenv
pyenv install 3.11.7
pyenv local 3.11.7
python -m venv .venv
```

- virtualenv: third-party tool that works similarly; not necessary for Python 3.6+.

## 9) VS Code integration

Open the project in VS Code and select the interpreter from the Command Palette (Cmd+Shift+P) → `Python: Select Interpreter` → pick the one inside `.venv/bin/python`.

You can also add a workspace setting (optional) to pin the interpreter:

```json
{
	"python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python"
}
```

## 10) Troubleshooting

- "command not found: python3": install Python with Homebrew and ensure `/usr/local/bin` or Homebrew's bin is on your PATH.
- `python` still points to system Python after activation: verify you sourced the `activate` script and check `which python` and `echo $PATH`.
- Permission errors creating `.venv`: make sure you are creating it in a directory you own (avoid system paths).
- macOS system Python (pre-installed) is not recommended for development; prefer Homebrew or pyenv-managed Pythons.

## 11) Common quick commands

```bash
# create
python3 -m venv .venv
# activate
source .venv/bin/activate
# install
pip install -r requirements.txt
# freeze
pip freeze > requirements.txt
# deactivate
deactivate
# remove
rm -rf .venv
```

