## Lab 2 — Instructions

Goal: remove confusing noise, then add stable repository facts.

### Part A — create repo-wide instructions

Create `.github/copilot-instructions.md`.

```markdown
# Hackathon Dashboard

A single-page React dashboard that tracks hackathon teams and a countdown
timer. It is built as a static site and deployed to GitHub Pages.

## Stack

- React 19 with plain JavaScript and JSX. There is no TypeScript here.
- Vite 7 for the dev server and the build.
- `@phosphor-icons/react` for all icons.
- Hand-written CSS in `src/App.css`. There is no Tailwind, no CSS framework, and no CSS-in-JS.

## Commands

- `npm install` installs the dependencies.
- `npm run dev` starts the dev server.
- `npm run build` writes the static site to `dist/`.
- `npm run lint` runs ESLint. Always run it before you finish.

There is no test runner in this repository. Do not add a test command and do not import a test library unless you are asked to set up testing.

## Layout

- `src/main.jsx` mounts the application.
- `src/App.jsx` holds every component: `useKV`, `formatTime`,
  `CountdownTimer`, `TeamForm`, and `App`.
- `src/App.css` holds all application styling and all design tokens.
- `src/index.css` holds leftover Vite template styles. Do not add rules to it.
- `docs/design-system.md` describes the visual language. Read it before you change the interface.
- `.github/workflows/deploy.yml` builds and deploys to GitHub Pages.

## Styling rules

The utility class names in `src/App.css` look like Tailwind, but Tailwind is not installed. Only the classes declared in `src/App.css` exist. Any other utility class renders with no style.

Confirm that a class is declared in `src/App.css` before you use it. Add a new utility or component style to `src/App.css` first if you need one.

## Product conventions

- The interface uses a CRT terminal look. Keep it.
- Button labels start with a shell prompt, for example `$ new-team`.
- Every control has visible text or an `aria-label`.
- Persist state with the `useKV` hook in `src/App.jsx`. Do not add a second
  storage helper.

## Constraints

- Do not edit `package-lock.json` by hand. Change dependencies with npm.
- Do not edit `.github/workflows/deploy.yml`, `CODEOWNERS`, or `LICENSE`.
- Give a reason in your summary before you add a dependency.
```

### Part B — create JSX instructions

Create `.github/instructions/jsx-components.instructions.md`.

```markdown
---
applyTo: "src/**/*.jsx"
---

- Use function components and hooks. Do not add class components.
- Import icons from `@phosphor-icons/react`. Do not add a second icon package.
- Compute derived values, such as a filtered list, during render. Do not copy
  them into `useState`.
- Give every interactive element an accessible name. Use visible text, or
  `aria-label` when the control shows only an icon.
- Follow the dialog pattern already in the file: a `.dialog-overlay` that
  closes on click, wrapping a `.dialog` that calls `stopPropagation()`.
- Use only `className` values that are declared in `src/App.css`.
```

### Part C — create CSS instructions

Create CSS related instruction files using command `/instructions` and text "CSS related instructions based on given design in #App.css and #index.css"

### Prompt — re-run the challenge as a Plan (Use Plan Mode)

```text
Add a search box and a room filter to the Registered Teams section of the dashboard. Users must be able to type text that matches a team name or topic, pick a room from a dropdown, and see a count of matching teams. Show a message when nothing matches.

List:
1. The file and the component that will own the filter state.
2. Every className you will use, and the line in src/App.css that declares it.
3. Every class you must add, with the CSS you will write.
4. How each new control gets an accessible name.
5. The commands you will run to validate the change.

Stop after the plan.
```