# 📚 Book & Notes Vault

A personal reading tracker for keeping a catalog of books and the notes attached to them. It is built with plain HTML, CSS, and JavaScript: no frameworks, ES modules for structure, and `localStorage` for saving data.

| | |
|---|---|
| **Theme** | Book & Notes Vault |
| **Live site (GitHub Pages)** | https://favour-nziza.github.io/E_library_book/ <!-- TODO: open this link and confirm it loads, not a 404 --> |
| **Repository** | https://github.com/favour-nziza/E_library_book |
| **Demo video (unlisted)** | <!-- TODO: paste unlisted video link --> |
| **Author** | Favour Nziza · GitHub: [favour-nziza](https://github.com/favour-nziza) · f.nziza1@alustudent.com |

---

## Features

**Catalog**
- Add, edit, and delete books with title, author, pages, tag, date added, ISBN, and notes
- Live regex search: the pattern is compiled safely, case sensitivity can be toggled, and matches are highlighted with `<mark>`
- Sort by date, title (A→Z / Z→A), or pages (ascending / descending)
- Filter by tag from a dropdown
- Table layout on desktop and card layout on mobile
- Inline row editing on desktop; the form is used for editing on mobile
- Confirm dialog before deleting, with a keyboard focus trap and Escape to close

**Dashboard**
- Total books, total pages, top tag, average pages
- 7-day activity chart and tag breakdown bars, built with CSS and JS
- Reading target (page cap) with a progress bar and ARIA live messages

**Data**
- Everything saves automatically to `localStorage`
- JSON import (validated before loading) and JSON export
- CSV export with proper escaping
- `seed.json` with 12 records, including edge dates, large and small page counts, and tricky strings

**Settings and extras**
- Light / dark theme toggle, saved between visits
- Page ↔ time converter in the catalog
- Hamburger navigation on small screens

**Responsive design**
- Mobile-first: the base styles target small phones (~360px), with breakpoints at **480px**, **768px**, and **1024px**

---

## Regex Catalog

| # | Pattern | Used for | Examples |
|---|---------|----------|----------|
| 1 | `/^\S(?:.*\S)?$\|^\S$/` | Title and author: no leading or trailing spaces | ✓ `Dune` · ✗ ` Dune` |
| 2 | `/ {2,}/` | Forbid consecutive spaces <!-- TODO: confirm this matches validators.js exactly --> | ✗ `Dune  Messiah` |
| 3 | `/^(0\|[1-9]\d*)(\.\d{1,2})?$/` | Numeric field (pages) | ✓ `312` · ✗ `abc`, `012` |
| 4 | `/^\d{4}-(0[1-9]\|1[0-2])-(0[1-9]\|[12]\d\|3[01])$/` | Date, strict `YYYY-MM-DD` | ✓ `2025-09-25` · ✗ `25/09/2025` |
| 5 | `/^[A-Za-z]+(?:[ -][A-Za-z]+)*$/` | Tag: letters, spaces, hyphens | ✓ `Sci-Fi` · ✗ `123` |
| 6 | `/^(?:(?:\d[\s-]?){9}[\dX]\|(?:\d[\s-]?){13})$\|^$/` <!-- TODO: paste the exact ISBN pattern from validators.js --> | ISBN-10 or ISBN-13 (optional field) | ✓ `978-0-7564-0407-1` |
| 7 ★ | `/\b(\w+)\s+\1\b/i` | **Advanced (back-reference):** catches duplicated words in author and notes | ✗ `the the`, ✗ `Herbert Herbert` |
| 8 ★ | `/^(?![A-Z\s]+$).+$/` | **Advanced (negative lookahead):** title must not be all uppercase | ✗ `DUNE` · ✓ `Dune` |

The search box accepts any pattern the user types. It is compiled inside a `try/catch`, so an invalid pattern shows a message instead of breaking the page.

---

## Keyboard Map

| Key / Action | Effect |
|---|---|
| `Tab` / `Shift+Tab` | Move focus forward / backward through interactive elements |
| `Enter` / `Space` | Activate the focused button or link |
| First `Tab` on page load | Reveals the skip link, which jumps to `#main-content` |
| `Esc` | Closes the confirm dialog |
| `Tab` inside the dialog | Focus stays trapped between the dialog's buttons |
| Arrow keys on a `<select>` | Change the sort or filter option |

<!-- TODO: add anything else your app supports, e.g. Enter/Space on sortable table headers, if you built it -->

---

## Accessibility Notes

- **Landmarks:** `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`
- **Skip link** at the top of the page, visible on focus
- **Labels:** every form input has a `<label>` tied to it with `for`/`id`; required fields use `aria-required="true"`; invalid fields use `aria-invalid="true"`
- **Focus styles:** every interactive element gets a visible 3px focus ring (`:focus-visible`)
- **Live regions:**
  - `aria-live="polite"` for the record count, form status, and cap status while under the target
  - `aria-live="assertive"` for search errors and the cap-exceeded message
  - `role="alert"` for inline field errors
- **Highlighting:** `<mark>` keeps the original text, so screen readers read it normally
- **Progress bar:** `role="progressbar"` with `aria-valuenow`, `aria-valuemin`, and `aria-valuemax`
- **Dialog:** `role="dialog"`, `aria-modal="true"`, `aria-labelledby`, focus trap, Escape to close
- **Contrast:** <!-- TODO: run both themes through a contrast checker and state the result you actually got -->

---

## Design Decisions

<!-- TODO: write 3–5 short points in your own words. Graders look for real reasoning here. Ideas:
- Why you chose Book & Notes Vault over the other themes
- Why cards on mobile and a table on desktop
- Why you split the code into state / storage / ui / validators / search
- A bug you hit and how you fixed it (e.g. escaping HTML before highlighting)
-->

---

## How to Run Tests

1. Clone or download the repo.
2. Serve it locally, because ES modules don't load from `file://`: `npx serve .` or VS Code Live Server.
3. Open `http://localhost:<PORT>/tests.html`.
4. The assertions run on page load and show pass/fail counts.

---

## Project Structure

```
E_library_book/
├── index.html          # App shell (semantic HTML, ARIA landmarks)
├── tests.html          # Test page with assertions
├── seed.json           # 12 sample records
├── README.md
├── styles/
│   └── main.css        # Mobile-first CSS, custom properties, animations
└── scripts/
    ├── main.js         # Entry point: wires events, nav, form, import/export
    ├── state.js        # State, filtering, sorting
    ├── storage.js      # localStorage read/write
    ├── ui.js           # DOM rendering (table, cards, dashboard, dialog)
    ├── validators.js   # Regex rules, safe regex compiler, import validator
    └── search.js       # Match highlighting with HTML escaping
```

---

## Running Locally

```bash
git clone https://github.com/favour-nziza/E_library_book
cd E_library_book
npx serve .        # or: python3 -m http.server 8080
```

Open the address it prints. To load sample data, go to **Catalog → Import JSON** and choose `seed.json`.

---

## Milestones

| Milestone | Focus |
|---|---|
| M1 | Spec, wireframes, data model, a11y plan |
| M2 | Semantic HTML and mobile-first base CSS |
| M3 | Form with regex validation rules, `tests.html` |
| M4 | Catalog table/cards, sorting, safe regex search, highlighting |
| M5 | Dashboard stats, progress bar, ARIA live cap messages |
| M6 | Persistence, JSON/CSV import-export, settings |
| M7 | Polish, dark mode, accessibility pass, README, demo video |

---

## AI Usage

As permitted by the assignment, AI assistance was used only for drafting and formatting this README. <!-- TODO: keep this line accurate to what you actually did -->
