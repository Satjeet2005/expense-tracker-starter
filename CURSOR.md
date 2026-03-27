This file provides guidance to AI coding agents like Claude Code (claude.ai/code), Cursor AI, Codex, Gemini CLI, GitHub Copilot, and other AI coding assistants when working with code in this repository.

## Project Overview

- This is a small Vite + React expense tracker starter project used for a course exercise.
- The current app is intentionally imperfect (per `README.md`), so expect code quality and behavior bugs to be part of normal work.
- There is no backend; all data is local in React component state.

## Common Commands

- Install dependencies: `npm install`
- Start dev server: `npm run dev` (Vite default is `http://localhost:5173`)
- Build production bundle: `npm run build`
- Preview production build locally: `npm run preview`
- Run linting: `npm run lint`

## Tests

- There is currently no test framework or `npm test` script configured in `package.json`.
- If tests are added, include both:
  - a project-wide test command
  - a documented single-test invocation pattern for that specific test runner

## High-Level Architecture

- Entry point is `src/main.jsx`, which mounts the app using `createRoot(...)` and wraps `App` in `StrictMode`.
- `src/App.jsx` is now an orchestration layer:
  - owns the `transactions` collection and static `categories`
  - appends new transactions via `handleAddTransaction`
  - composes child components instead of rendering all sections inline
- `src/components/Summary.jsx`:
  - receives `transactions`
  - computes `totalIncome`, `totalExpenses`, and `balance`
  - renders the summary cards
- `src/components/TransactionForm.jsx`:
  - owns form-local state (`description`, `amount`, `type`, `category`)
  - validates and parses form input (`amount` string -> number)
  - emits normalized payload through `onSubmit`
- `src/components/TransactionList.jsx`:
  - receives `transactions` and `categories`
  - owns list-local filter state (`filterType`, `filterCategory`)
  - renders filtered transactions table
- Styling is split between `src/index.css` (global/base styles) and `src/App.css` (component/page styles).

## Data Model and Behavior Notes

- Transaction shape:
  - `id`, `description`, `amount`, `type`, `category`, `date`
- Seeded and stored transaction `amount` values are numbers.
- Form input amount starts as a string (HTML input behavior) and is converted to number in `TransactionForm` before submission.
- Filtering is two-step in-memory filtering inside `TransactionList`:
  - first by `filterType`
  - then by `filterCategory`
- No persistence layer exists (reload resets data to seed state).

## Tooling and Config

- Build tool: Vite (`vite.config.js` with `@vitejs/plugin-react`).
- Linting uses ESLint flat config (`eslint.config.js`) with:
  - `@eslint/js` recommended rules
  - `eslint-plugin-react-hooks`
  - `eslint-plugin-react-refresh` (Vite preset)
- `dist/` is globally ignored by ESLint.

## Important Source Files

- App bootstrap: `src/main.jsx`
- App orchestration and transaction state: `src/App.jsx`
- Summary calculations and summary UI: `src/components/Summary.jsx`
- Add transaction form and validation: `src/components/TransactionForm.jsx`
- Transaction filters and table: `src/components/TransactionList.jsx`
- Lint config: `eslint.config.js`
- Vite config: `vite.config.js`
- Project setup/context: `README.md`
