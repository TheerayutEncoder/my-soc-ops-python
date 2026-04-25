# Copilot Workspace Instructions

## Development Checklist

Before committing any changes, ensure:

- [ ] `uv run ruff check .` passes with no errors
- [ ] `uv run pytest` passes
- [ ] Code follows Python conventions (snake_case, type hints)
- [ ] No unused variables or imports

## Version Control

**Create Pull Requests instead of pushing directly to main:**
- Create a feature branch for each task: `git checkout -b feat/description`
- Commit changes frequently with clear messages
- Push branch and create a PR for review
- Never push directly to main branch

```bash
# Start new feature
git checkout -b feat/descriptive-name

# Make changes, then commit
git add .
git commit -m "feat: descriptive message"

# Push branch and create PR
git push -u origin feat/descriptive-name
# Then create PR via GitHub UI or gh CLI: gh pr create
```

## Project Overview

**Soc Ops** is a Social Bingo game built with Python (FastAPI + Jinja2 + HTMX). Players find people who match questions to mark squares and get 5 in a row.

## Architecture

```
app/
├── templates/       # Jinja2 HTML templates
│   ├── base.html
│   ├── home.html
│   └── components/  # bingo_board, bingo_modal, game_screen, start_screen
├── static/          # CSS & JS assets
├── models.py        # Pydantic models (GameState, BingoSquare)
├── game_logic.py    # Board generation & bingo detection
├── game_service.py  # Session management (GameSession)
├── data.py          # Question bank
└── main.py          # FastAPI routes & HTMX endpoints
tests/
├── test_api.py      # API endpoint tests (httpx + TestClient)
└── test_game_logic.py  # Game logic unit tests
```

## Key Commands

```bash
uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000  # Run dev server
uv run pytest                                                     # Run tests
uv run ruff check .                                               # Lint
```

## Styling

Uses custom CSS utility classes (Tailwind-like) in `app/static/css/app.css`:
- Layout: `.flex`, `.grid`, `.items-center`
- Spacing: `.p-4`, `.mb-2`, `.mx-auto`
- Colors: `.bg-accent`, `.bg-marked`, `.text-gray-700`

See `.github/instructions/css-utilities.instructions.md` for full reference.

## State Management

- `GameSession` manages game state server-side
- State persisted via signed cookies (itsdangerous)
- HTMX handles partial page updates without full reloads

## Important Notes

- **No Simple Browser**: HTMX requires a full browser. Use `"$BROWSER" http://localhost:8000` to preview
- **Session-based state**: Each user gets a unique session ID via cookies
- **Component-based templates**: HTMX swaps specific components instead of full page reloads
