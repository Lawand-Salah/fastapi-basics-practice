# Back-End Development v1 — FastAPI Learning Exercises

Small, standalone FastAPI exercises written while learning the basics of building a REST API in Python — each file is a self-contained example rather than a single combined application.

## What's here

- **`main.py`** — a minimal "Hello World" FastAPI app with a single `GET /welcomemessage` endpoint, demonstrating the basic shape of a FastAPI app (the `FastAPI()` constructor, the `app` object, and the `@app.get(...)` decorator that links a URL path to a handler function).
- **`carsharing.py`** — a second, separate FastAPI app exposing `GET /api/cars`, which returns a small in-memory list of car records (size, fuel type, number of doors, transmission).

Each file runs its own independent FastAPI app — they are not wired together into one application with shared routing.

## Getting started

Each file is run on its own with Uvicorn, e.g.:

```bash
uvicorn main:app --reload
```

or

```bash
uvicorn carsharing:app --reload
```

The API will be available at `http://127.0.0.1:8000`, with interactive docs at `http://127.0.0.1:8000/docs`.

## A repo housekeeping note

This repository currently has the Python virtual environment (`Virtual_en/`) committed alongside the actual source files, rather than excluded via `.gitignore`. Of the roughly 3,850 files in this repo, all but 2 are third-party packages (FastAPI, Pydantic, pip itself, and their dependencies) rather than project code — `main.py` and `carsharing.py` are the only files that are actually this project's own work. Going forward, adding a `.gitignore` with an entry for the venv folder (and running `git rm -r --cached Virtual_en` once) would keep the repo to just the code that matters.

## Known issue

In `carsharing.py`, the car records are written as:

```python
db = [
    {id: 1, 'size': 's', 'fuel': 'gasoline', 'doors': 3, 'transmission': 'auto'},
    ...
]
```

The first key, `id`, is missing its quotes. Because `id` is also the name of a Python built-in function, this doesn't cause an error — it quietly uses the `id` *function itself* as the dictionary key, not the string `"id"`. The code runs and `/api/cars` still returns data, but trying to read `car["id"]` anywhere would fail with a `KeyError`, since no key with that string actually exists. The fix is simply to quote it: `'id': 1`.

## Author

Lawand Salah
