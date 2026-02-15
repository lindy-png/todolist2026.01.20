# CLAUDE.md

## Project Overview

Google Sheets Todo List — a single-file web application that connects to public Google Sheets and displays todo items with interactive completion tracking. Built with vanilla HTML/CSS/JavaScript; no frameworks, build tools, or package dependencies.

## Repository Structure

```
.
├── index.html    # Entire application (HTML + CSS + JavaScript)
├── README.md     # User-facing documentation and setup guide
└── CLAUDE.md     # This file
```

## Tech Stack

- **Language**: HTML5, CSS3, JavaScript (ES6+)
- **Framework**: None (vanilla)
- **Build system**: None — open `index.html` directly in a browser
- **Package manager**: None — zero npm/yarn dependencies
- **Testing**: None configured
- **External APIs**: Google Sheets CSV export, Google Visualization API (gviz)
- **CORS proxies**: corsproxy.io, allorigins.win (used as fallbacks)

## Running the Application

Open `index.html` in any modern browser. No server, build step, or installation required.

## Architecture

### Single-file layout (`index.html`)

| Section | Lines | Purpose |
|---------|-------|---------|
| HTML structure | 1–222 | DOM elements: config panel, todo list container |
| CSS (`<style>`) | 7–192 | All styling, responsive layout |
| JavaScript (`<script>`) | 224–392 | Application logic |

### Data flow

1. User enters a Google Sheets URL/ID and sheet GID in the config panel
2. `connectSheet()` parses the input, stores config in `localStorage`
3. `fetchTodos()` attempts 4 methods in sequence (direct CSV, corsproxy.io, allorigins.win, gviz JSONP)
4. On success, `parseCSV()` or gviz callback converts data to a 2D array
5. `renderTodos()` auto-detects columns by header name and renders the list
6. `toggleTask()` persists completion state in `localStorage`

### Key functions

| Function | Purpose |
|----------|---------|
| `connectSheet()` | Parse spreadsheet URL/ID, save config, trigger fetch |
| `fetchTodos()` | Try 4 fetch methods with fallback chain |
| `parseCSV(text)` | Parse CSV handling quoted fields and escaping |
| `renderTodos(rows)` | Detect columns dynamically, render HTML todo items |
| `fc(headers, names)` | Find column index by fuzzy-matching header names |
| `toggleTask(key, el)` | Toggle task completion, update DOM and localStorage |
| `esc(text)` | HTML-escape text to prevent XSS |

### Dynamic column detection

Headers are matched case-insensitively against these variants:
- **Task**: `task`, `todo`, `title`, `name`
- **Status**: `status`, `done`, `completed`
- **Context/Project**: `context`, `project`, `category`
- **Owner/DRI**: `dri`, `owner`, `assignee`

Completed status values recognized: `done`, `complete`, `completed`, `yes`, `true`, `x`, `✓`

### State management

All state is stored in browser `localStorage`:
- `spreadsheetId` — the connected sheet ID
- `sheetGid` — the selected sheet tab GID
- `completedTasks` — JSON-serialized array of completed task keys

## Code Conventions

### Naming
- **HTML IDs**: camelCase (`configPanel`, `todoList`, `spreadsheetId`)
- **CSS classes**: kebab-case (`.todo-item`, `.config-panel`, `.indicator-done`)
- **JS variables/functions**: camelCase (`fetchTodos`, `completedTasks`)
- Short names used in tight loops/helpers (`fc`, `h`, `r`, `c`, `t`, `q`)

### Style
- Template literals for HTML generation
- Inline `onclick` handlers on config panel button; event delegation on todo list
- Ternary operators and short-circuit evaluation used heavily
- DOM manipulation via `innerHTML` and `classList`
- `document.createElement('div').textContent` technique for HTML escaping

### CSS
- System font stack (`-apple-system, BlinkMacSystemFont, ...`)
- Flexbox layout throughout (no CSS Grid)
- Design colors: `#007AFF` (primary blue), `#e53935` (red accent), `#10B981` (green/done), `#F59E0B` (amber/urgent)
- Border-radius: `12px` for cards, `8px` for inputs/buttons, `50%` for indicators
- SVG icons are inline

## Security Notes

- All user-provided text is HTML-escaped via `esc()` before insertion into `innerHTML`
- No server-side component; all processing is client-side
- Spreadsheet must be publicly readable (no OAuth flow)
- localStorage data is accessible to any script on the same origin

## Making Changes

Since this is a single-file application:
- All edits happen in `index.html`
- CSS changes go in the `<style>` block (lines 7–192)
- JavaScript changes go in the `<script>` block (lines 224–392)
- Test changes by refreshing `index.html` in a browser
- No linting, formatting, or build commands to run
