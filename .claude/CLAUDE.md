# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Run the app (dev server on port 5001)
python app.py

# Run all tests
pytest

# Run a single test file
pytest tests/test_auth.py

# Run a single test by name
pytest -k "test_login"
```

## Architecture

**Spendly** is a Flask expense tracker built as a step-by-step teaching project. The code is deliberately incomplete — stub routes and an empty `database/db.py` are intentional scaffolding for students to fill in.

### Request flow

```
Browser → app.py (Flask routes) → database/db.py (SQLite helpers) → SQLite file
                                ↓
                         templates/ (Jinja2, extend base.html)
                                ↓
                     static/css/style.css + static/js/main.js
```

### Key conventions

- **All routes live in `app.py`** — no blueprints. New routes go here until the file grows large enough to split.
- **`database/db.py`** must expose three functions: `get_db()` (returns a connection with `row_factory` and foreign keys on), `init_db()` (creates tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (inserts sample data).
- **Templates extend `base.html`** — which provides the navbar, footer, Google Fonts (DM Sans + DM Serif Display), and the CSS/JS includes. Child templates use `{% block content %}` and optionally `{% block title %}`, `{% block head %}`, `{% block scripts %}`.
- **CSS design tokens** are CSS custom properties on `:root` in `style.css`. Use `--accent` (green `#1a472a`) for primary actions, `--accent-2` (amber) for secondary, `--danger` for destructive actions. Don't add inline styles — extend the stylesheet.
- Auth forms POST to `/login` and `/register`. They render `{{ error }}` inside `.auth-error` when the route passes an `error` variable to the template.

### Planned feature steps (per route comments)

1. Database setup (`database/db.py`)
2. Registration POST handler
3. Login + logout (session management)
4. Profile page
5–6. Expense list + dashboard
7. Add expense
8. Edit expense
9. Delete expense
