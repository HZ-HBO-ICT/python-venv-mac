# python-venv-mac template

This a template you can use for setting up a virtual environment for Python and Jupyter Notebook.

## What is a Python virtual environment?

A **virtual environment (venv)** is an **isolated Python installation for one project**.

Think of it like this:

* Your **system Python** = the kitchen
* A **virtual environment** = a lunchbox for one project

Each lunchbox contains:

* Its own Python interpreter
* Its own installed packages (`pip install ...`)
* Its own package versions

### Why this matters

Without a virtual environment:

* All projects share the same Python packages
* Installing or upgrading a package for one project can break another
* Version conflicts happen very easily

Example problem:

* Project A needs `Django==3.2`
* Project B needs `Django==5.0`

👉 **Virtual environments solve this by isolating dependencies per project.**

---

## What happens when you activate a venv?

When you activate a virtual environment:

* `python` and `pip` point to the **project’s local versions**
* Packages install into `.venv/` instead of system Python
* Nothing outside the project is affected

When you deactivate it:

* Everything goes back to normal

---

## Recommended setup (Apple Silicon + VS Code)

This is the **best-practice setup** for macOS (M1/M2/M3).

---

## Step 1: Make sure Python is installed correctly

Check:

```bash
python3 --version
```

If it’s missing or very old, install via Homebrew:

```bash
brew install python
```

⚠️ Apple Silicon note: Homebrew installs Python natively for ARM — no extra steps needed.

---

## Step 2: Create the virtual environment

Open your project folder in VS Code.

In the VS Code terminal:

```bash
cd your-project
python3 -m venv .venv
```

This creates:

```
your-project/
├── .venv/
├── main.py
├── requirements.txt
```

`.venv` is the standard name and is auto-detected by VS Code.

---

## Step 3: Activate the virtual environment (macOS)

```bash
source .venv/bin/activate
```

You’ll see:

```text
(.venv) your-project %
```

That means it’s active.

---

## Step 4: Tell VS Code to use this venv (important)

1. Press **Cmd + Shift + P**
2. Search for **Python: Select Interpreter**
3. Choose:

   ```
   .venv/bin/python
   ```

VS Code will now:

* Run code using the venv
* Install packages into the venv
* Use correct linting & autocomplete

💡 VS Code usually remembers this automatically.

--_

## Step 6: Save dependencies

```bash
pip freeze > requirements.txt
```

This allows others (or CI) to recreate the environment.

To reinstall later:

```bash
pip install -r requirements.txt
```

---

## Step 7: Deactivate when done

```bash
deactivate
```

---

## Important macOS & VS Code tips

### Add venv to `.gitignore`

```gitignore
.venv/
```

### Always use `python`, not `python3`, once activated

```bash
python app.py
pip install numpy
```

### Never use `sudo pip install`

That installs packages globally and defeats the purpose.

---

## How this all fits together (mental model)

* `.venv/` → isolated Python + packages
* `activate` → “use this Python now”
* VS Code interpreter → editor knows which Python to use
* `requirements.txt` → reproducible environment


---

## What you need for Jupyter (short answer)

You need **three things**, all installed **inside your virtual environment**:

1. **Jupyter** (or JupyterLab)
2. **ipykernel** (connects your venv to Jupyter)
3. **VS Code Python + Jupyter extensions** (editor side)

---

## Step-by-step setup (recommended)

### 1. Activate your virtual environment

In VS Code terminal:

```bash
source .venv/bin/activate
```

Confirm:

```bash
which python
```

It should point to:

```
your-project/.venv/bin/python
```

---

### 2. Install Jupyter and ipykernel

Inside the venv:

```bash
pip install jupyter ipykernel
```

That’s it for Python-side installs.

What each does:

* `jupyter` → runs notebooks
* `ipykernel` → lets this venv appear as a selectable kernel

---

## Using Jupyter in VS Code (best experience)

### 4. Install VS Code extensions

Install these **once**:

* **Python** (by Microsoft)
* **Jupyter** (by Microsoft)

VS Code will usually prompt you automatically.

---

### 5. Create or open a notebook

* Create a file:

  ```
  notebook.ipynb
  ```
* Or:

  ```
  Cmd + Shift + P → Jupyter: Create New Notebook
  ```

---

### 6. Select the correct kernel

In the top-right of the notebook:

* Click **Select Kernel**
* Choose:

  ```
  Python (.venv)
  ```

Now:

* All cells run using your virtual environment
* `pip install` installs into `.venv`

---

## Installing packages for notebooks

Always do this **with the kernel selected** or in the terminal with the venv activated.

### Option A (recommended): terminal

```bash
pip install numpy pandas matplotlib
```

### Option B (inside notebook)

```python
%pip install seaborn
```

⚠️ Use `%pip`, not `!pip`, in notebooks.

---

## Verify everything works

Run this cell:

```python
import sys
print(sys.executable)
```

Expected output:

```python
.../your-project/.venv/bin/python
```

✅ That confirms Jupyter is using the correct environment.