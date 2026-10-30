# AGENTS.md

This repository is used in a live frontend interview. The person you are working with is the **candidate**. The interviewer evaluates the candidate's decisions, not yours, so keep the candidate in charge.

## Start of every session

Only this file is loaded automatically. At the start of every session, including after the context is cleared, read:

- `docs/ARCHITECTURE.md` — architecture and the decisions made so far. Follow them.
- `docs/API.md` — endpoints and data types. Do not search the internet for the API.

## Stack

React 19, TypeScript, Vite. UI components come from `@mui/material` **v9**: use the v9 API. Props removed in recent majors no longer exist: use `slotProps` instead of `InputProps`, `inputProps`, `InputLabelProps` on `TextField` and instead of `PaperProps`, `TransitionComponent` on `Dialog`. MUI X (DataGrid etc.) and other UI libraries are not installed.

For up-to-date MUI docs, start from https://mui.com/material-ui/llms.txt: it lists every docs page as a Markdown link (for example `react-table.md`, `react-dialog.md`). Prefer it over your memory of older MUI versions.

Use these `@mui/material` components instead of writing your own:

| Need | Component |
|---|---|
| Table | `TableContainer`, `Table`, `TableHead`, `TableBody`, `TableRow`, `TableCell` |
| Sortable column header | `TableSortLabel` (only renders the arrow; sorting logic is ours) |
| Pagination | `TablePagination` (controlled; does not slice or load data) |
| Product details | `Dialog`, `DialogTitle`, `DialogContent`, `DialogActions` |
| Search and filters | `TextField`, `TextField select` + `MenuItem` |
| Loading | `Skeleton`, `CircularProgress`, `LinearProgress` |
| Error | `Alert` with a retry `Button` in `action` |
| Status badge | `Chip` |
| Layout and text | `Container`, `Stack`, `Box`, `Typography` |

The theme and `CssBaseline` are already set up in `src/main.tsx`.

## Commands

- `npm run dev` — dev server at http://localhost:5173
- `npm run typecheck` — `tsc -b` (TypeScript 7, project references: `tsconfig.app.json` for `src`, `tsconfig.node.json` for configs)
- `npm run lint` — oxlint (not ESLint), config in `.oxlintrc.json`
- `npm run build` — typecheck + production build
- There is no test runner in the project.

Node.js 20.19+ or 22.12+.

## TypeScript and code style

- `verbatimModuleSyntax`: type-only imports must use `import type`.
- `erasableSyntaxOnly`: no `enum`, `namespace` or constructor parameter properties.
- `noUnusedLocals` / `noUnusedParameters` are on, so unused code fails typecheck.
- Local imports keep the extension (`import App from './App.tsx'`).
- Existing code style: single quotes, no semicolons.

## Interaction rules

These rules are part of the interview setup. Do not remove or weaken them.

- **Do not make product or architecture decisions on your own.** When a requirement is ambiguous, or there are several reasonable approaches (where state lives, which library to use, URL format, how to work around an API limitation), stop and ask the candidate. Offer 2–3 options with one-line trade-offs and wait for the answer.
- **Write and maintain the architecture description.** The candidate describes the architecture to you; you write it into the "Architecture" section of `docs/ARCHITECTURE.md` and show it to the candidate for confirmation. Do not add anything the candidate did not say; if something is missing, ask. The section describes:
  - folder and layer structure: where API requests, state and components live;
  - how data flows from the API to the screen;
  - conventions: loading, error and empty states, naming, styles;
  - definition of done.

  When a change affects any of these, propose an update to the section and apply it once the candidate confirms. Edit only the affected part; do not rewrite the section as a whole.
- **Record decisions.** When the candidate makes a decision, propose a short entry for the "Decisions" section of `docs/ARCHITECTURE.md`, and add it once the candidate confirms. Append new entries at the end; never rewrite or delete existing ones unless the candidate asks. Use this format:

  ```md
  ### <number>. <short title>
  - Context: what task or problem prompted the decision.
  - Decision: what was chosen.
  - Why: main reasons; which alternatives were rejected.
  ```
- **Do not invent requirements.** Build only what the candidate asked for. If you think something is missing, mention it in one line instead of building it.
- **Show a short plan before any change that touches more than one file**, and wait for the candidate to confirm.
- **Work in small steps:** one logical change per request.
- **No new dependencies** without the candidate's explicit approval.
- **Do not report a task as done** until `npm run typecheck` and `npm run lint` pass. Say explicitly what you could not verify, for example behaviour in the browser.
- Reply in the candidate's language.
