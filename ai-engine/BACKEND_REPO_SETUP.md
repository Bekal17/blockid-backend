# BlockID Backend – Standalone Repo Setup

Use these steps to create a new GitHub repo from `app/ai-engine` and push it to **https://github.com/bekal17/blockid-backend.git**.

---

## Prerequisites

- Git installed.
- You are in the **monorepo root** (the folder that contains `app/` and `landing/`).  
  Example: `blockid-protocol/` or `app_BLOCKID/` (if your app lives under `app_BLOCKID/app/`, use `app_BLOCKID/app` as the root for the commands below).

---

## Step 1 – Copy `app/ai-engine` to `blockid-backend`

**Windows (PowerShell):**

```powershell
# From the folder that contains app/ (e.g. app_BLOCKID\app or blockid-protocol)
Copy-Item -Path ".\ai-engine" -Destination "..\blockid-backend" -Recurse -Force
```

If your monorepo root is one level up (e.g. you have `app_BLOCKID\app\ai-engine`):

```powershell
cd E:\app_BLOCKID\app
Copy-Item -Path ".\ai-engine" -Destination "..\blockid-backend" -Recurse -Force
```

**macOS / Linux (bash):**

```bash
# From the folder that contains app/
cp -r app/ai-engine ../blockid-backend
```

**Result:** New folder `blockid-backend` at the same level as `app/`, with all backend files (and optionally venv/trust.db; they will be ignored by git via `.gitignore`).

---

## Step 2 – Initialize Git in `blockid-backend`

```bash
cd ../blockid-backend
git init
```

---

## Step 3 – Add all files and commit

```bash
git add -A
git status
git commit -m "Initial commit: BlockID backend ai-engine"
```

- `requirements.txt` and `main.py` should appear in `git status`. If either is **missing**, fix the copy or paths before pushing.
- Optional: remove the copied `venv` and `trust.db` from the new folder so they are not present at all (they are already ignored by `.gitignore`):

  **Windows:**  
  `Remove-Item -Recurse -Force venv, trust.db -ErrorAction SilentlyContinue`

  **macOS/Linux:**  
  `rm -rf venv trust.db`

---

## Step 4 – Connect to GitHub and push

```bash
git branch -M main
git remote add origin https://github.com/bekal17/blockid-backend.git
git push -u origin main
```

- If the repo on GitHub already has content (e.g. a README), you may need:

  `git pull origin main --allow-unrelated-histories`

  then fix any conflicts and run `git push -u origin main` again.

---

## Step 5 – Verify on GitHub

1. Open **https://github.com/bekal17/blockid-backend**.
2. Confirm you see:
   - `main.py`
   - `requirements.txt`
   - `analyzer.py`, `db.py`, `models.py`, `rpc_client.py`, `scoring.py`
   - `README.md`, `.gitignore`
3. Ensure **venv**, **trust.db**, and **.env** are **not** in the repo (they should be ignored).

---

## Warnings

| Check              | Action |
|--------------------|--------|
| **requirements.txt missing** | Copy it from `app/ai-engine/requirements.txt` or recreate it (FastAPI, uvicorn, solana, solders, pydantic, python-dotenv). |
| **main.py missing**         | Copy from `app/ai-engine/main.py`; do not push without the FastAPI app entrypoint. |
| **venv or trust.db in repo** | Ensure `.gitignore` in `blockid-backend` contains `venv/`, `trust.db`, `.env`. Run `git rm -r --cached venv` (or `trust.db`) if they were already committed. |

---

## One-shot (PowerShell, from `app` folder)

```powershell
Copy-Item -Path ".\ai-engine" -Destination "..\blockid-backend" -Recurse -Force
cd ..\blockid-backend
Remove-Item -Recurse -Force venv -ErrorAction SilentlyContinue
Remove-Item -Force trust.db -ErrorAction SilentlyContinue
git init
git add -A
git commit -m "Initial commit: BlockID backend ai-engine"
git branch -M main
git remote add origin https://github.com/bekal17/blockid-backend.git
git push -u origin main
```

Then verify the repo at **https://github.com/bekal17/blockid-backend**.
