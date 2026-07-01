# Kandoo

A single-file HTML kanban board with a sticky-note aesthetic. Vintage scientific instrument vibe, dark mode default, no build step, no dependencies, no server. Download the HTML file, open it in a browser, and you have a full project management workspace.

Current version: **v6.15**

![Kandoo screenshot](docs/screenshot.png)

## What it is

Kandoo is a hand-drawn sticky-note kanban that runs entirely in your browser. All data lives in `localStorage` on the machine that opens the page. Move a project between machines by exporting JSON. Get a printable snapshot by hitting Print.

Multiple projects, custom workflow stages per project, real people tracking, subtasks with per-subtask responsible and dates, per-task attachments, materials cost tracking on billable work, and about a dozen ways to look at the same tasks: board, list, calendar, reports, gantt, invoice desk, and more.

## Views

- **Board**: stickies on a whiteboard, one column per stage, drag to move
- **List**: flat table with sortable columns
- **Calendar**: monthly grid with tasks landing on their due dates
- **Reports**: four SVG charts (burndown, velocity, stage distribution, type breakdown) plus headline stats for hours, billable amount, materials, and completion percentage
- **Gantt**: timelines with dependency arrows and a focus banner
- **Dashboard**: project overview with counts, progress, and health signals
- **Invoice Desk**: labor / materials / total per billable task
- **Focus**: one task, no chrome, no distractions
- **Timeline**, **Templates**, **Sharing**, **Control Room**, and more

## Sticky features

Each sticky task can carry:

- Title, description, type (General / Bug / Feature Request), priority (Low / Medium / High / Urgent / Critical)
- Person responsible, with per-project autocomplete from the project's people list
- Need-by date, start date, and age tracking
- Subtasks, each with their own title, responsible, date, and custom-styled checkbox
- Hourly rate, hours actual and estimate, materials cost, billable flag
- Attachments (PDFs, images, whatever; base64-encoded into `localStorage`)
- Blocked status with a reason
- Health indicator (green / yellow / red based on overdue and blocked signals)
- Comments log
- Custom color from the theme palette

## Two ways to edit

1. Click **Edit** on the note for the full modal with every field
2. Click any editable pill on a sticky (type, priority, person, date, blocked) to open a popover with just that one field, save on selection, no modal needed

Empty fields show as dim placeholder pills (`👤 unassigned`, `📅 no date`) so they are one click away, not hidden behind the full modal.

## Keyboard shortcuts

Press `?` anywhere (as long as no input has focus) for the full cheatsheet. Quick reference:

| Key | Action |
|-----|--------|
| N | New sticky |
| / | Focus the search box |
| B | Board view |
| G | Gantt view |
| D | Dashboard view |
| F | Toggle focus mode |
| ? | Show the shortcut cheatsheet |
| Esc | Close modals, popovers, focus mode |
| Ctrl+K or Cmd+K | Open command palette |

## Mobile

Below 860px viewport the app rearranges:

- Hamburger menu opens a drawer containing all the sidebar panels
- Drawer starts with a **Quick Actions** panel: New Task, New Project, Board, Focus, Print, Shortcuts
- Board columns snap horizontally with chevron hints on the sides and column dots at the bottom
- Task-modal selects open bottom-sheet pickers with 48px+ tap targets, big cancel button, and translucent backdrop
- The same bottom sheets appear at any viewport when the Large Targets accessibility toggle is on

## Sidebar

The sidebar contains: Projects, Quick Add, Project Details, Visible Workflow Stages, Theme Drawer, Stage Rules (Control Room). Also People management inside Control Room.

- Every panel is collapsible. Click the heading to collapse or expand.
- Collapse state persists per-panel across reloads under the localStorage key `kandoo-panels-collapsed`.
- Keyboard accessible: focus the heading, press Enter or Space to toggle.

## Search and filters

Search box, type filter, priority filter, person filter, and a Clear button, all in a collapsible bar. When the bar is collapsed while a filter is active, the toggle label shows `Search + filters · active` so you never lose track of a stray filter that is quietly hiding tasks from you.

## Print

Print produces a formatted PDF with:

- A first-page header showing project name, active view, version stamp, and timestamp
- Sticky notes stripped of tape and shadow effects, printed as clean bordered cards
- Board columns stacked vertically for readable page flow
- SVG charts with print-safe grayscale palettes
- Tables with real borders instead of the rounded card look
- Chrome (toolbar, sidebar, view tabs, filter bar, modals, drawer) hidden

`@page` sizing is US Letter with 0.6 inch side margins, 0.75 inch top on the first page. Orphans and widows are held at three lines to prevent awkward page breaks.

## Data

Everything lives in `localStorage` under the key `kandoo-v6-15-data`. Two escape hatches:

- **Export JSON** produces a full project snapshot you can carry to another machine or drop in Git for backup
- **Print** or **PDF** produces a formatted, self-contained document

## Install

There is no install.

1. Download `kandoo.html`
2. Open it in any modern browser
3. That is it

- No build step
- No dependencies
- No server
- No accounts
- No analytics
- No network calls
- No frameworks

Bookmark the file or add it to your home screen for daily use.

## Browser support

Anything with ES2020, CSS `:has()`, and `localStorage`. In practice: current Firefox, Chrome, Safari, Edge.

## Architecture

Kandoo is a single HTML file with four registries on `window` that other modules extend without wrapping:

- `kandooSavePipeline`: `preSave`, `postSave` hooks
- `kandooRenderPipeline`: `preRender`, `postRender`, `postRenderAsync` hooks
- `kandooRenderViewPipeline`: same hooks scoped to view switches
- `kandooTabRegistry`: view tabs with auto-rebuild on registration

Public API:

- `registerView(id, opts)`: register a new view module
- `registerTab(id, label, opts)`: register a new view tab
- `unregisterView(id)`: remove a view module
- `kandooDebug()`: dump internal state to the console
- `kandoo`: top-level namespace with helpers

## Development

Single-file HTML. Edit `kandoo.html` directly. Every code change should ship with matching documentation.

There is a Playwright regression harness (`kandoo_regression_tests.py`) that runs 56 tests across thirteen categories: boot, content, navigation, theming, modal, color, focus, mobile, a11y, registries, features, polish, ux.

```bash
pip install playwright pytest-asyncio
playwright install chromium
python3 kandoo_regression_tests.py kandoo.html
```

The harness catches regressions in: registries, tab rendering, sticky editing, pill popovers, bottom sheets, mobile swipe UI, print header injection, collapsible panels, collapsible filterbar, calendar grid, report charts, materials cost math, people management, attachments, and more.

## House rules

If you fork or contribute:

- **No em dashes anywhere.** Not in code comments, not in UI copy, not in README text, not in commit messages. Use hyphens, colons, or restructure the sentence. Grep-verify before every commit.
- **Single-file HTML.** No build step ever gets added.
- **Dark mode default.** Vintage scientific instrument aesthetic (sticky notes, dashed borders, hand-drawn fonts, soft shadows).
- **GPL-3.0 as plain text** in the source header, no hyperlink.
- **Targeted file removal only.** Never bulk-delete with wildcards or clear directories without explicit approval.

## Changelog

**v6.15 (current)** brought a large runtime layer on top of the base v6.15 shell, including: unified render / renderView / save pipelines, tab and view registries, materials cost tracking, per-task attachments, real monthly calendar grid, four SVG report charts, per-project people management, subtask responsible and date fields, custom subtask checkboxes, unified print stylesheet with header injection, mobile swipe UI with chevron hints and column dots, popover pill editors, bottom-sheet dropdowns, keyboard shortcut cheatsheet, mobile Quick Actions panel, collapsible sidebar panels, and collapsible filterbar.

**v6.11 through v6.14** added the clean note element, custom color palettes, drag-and-drop reliability fixes, view registration, and the archive tab.

**Earlier** established the base kanban shell, sticky note aesthetic, theming, filters, and export formats.

## License

GPL-3.0.

The full license text is included as plain text in the header of `kandoo.html`.

## Author

M.B. Parks (N1HNP), Green Shoe Garage
https://greenshoegarage.com
https://mbparks.com

Make. Hack. Learn. Share. Repeat.
