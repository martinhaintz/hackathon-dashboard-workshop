## Lab 4 — Build the `extract-ui-design` skill

Goal: make Copilot extract the implemented design system from the code, then prove that a procedure can find defects that normal gates miss.

### Steps

1. Run Prompt 4A using the `/create-skill` command to generate the skill.
2. Review the generated files under `.github/skills/extract-ui-design/`.
3. Make any generated script executable.
4. Run Prompt 4B.
5. Open `src/App.jsx` around line 266.
6. Start the dev server if it is not already running.
7. Click `$ new-team`.
8. Look at the drag-and-drop upload area.
9. Run Prompt 4C.
10. Start a new chat.
11. Run Prompt 4D.
12. Confirm that Copilot selects the skill from its description.

### Prompt 4A — generate the skill

Run `/create-skill` and answer any follow-up questions it asks, using this text:

```text
Create a skill named extract-ui-design that extracts the implemented design system of this repository from its CSS and JSX, and writes or refreshes docs/design-system.md.

Use it when someone asks for the design system, the design tokens, the colour palette, or the UI conventions; when someone adds or changes a visible component; or when docs/design-system.md is missing or out of date.

The skill must:
- Collect declared CSS class selectors, colour values, and font stacks from src/*.css.
- Collect every className used in src/*.jsx, and report which ones are not declared in src/*.css. This check must also work against a diff, not only the whole file.
- Classify what it finds into: colour, typography, spacing, border and radius, layout utility, component class, motion, and state.
- Record conventions that only exist in the JSX, such as the shell prompt prefix on button labels, the icon package and default icon sizes, the dialog overlay and stop-propagation pattern, the aria-label rule for icon-only buttons, and the empty-state pattern.
- Report every token where src/index.css and src/App.css both set :root with a different value, and state which one wins at runtime.
- Write docs/design-system.md with sections for palette, typography, spacing, borders, layout utilities, component classes, motion, conventions, and known conflicts. Every value it lists must exist in the CSS; never invent one.

Put any helper script in a scripts/ folder and any supporting reference material in a references/ folder.
```

Confirm the skill lands in `.github/skills/extract-ui-design/`, and that `name` in `SKILL.md` matches the directory name.

If the skill added a shell script, make it executable.

```sh
chmod +x .github/skills/extract-ui-design/scripts/*.sh
```

### Prompt 4B — find missing classes

```text
Use the /extract-ui-design skill to find every CSS class that the JSX uses but the stylesheet does not declare.
```

You should see `flex-col`, `min-h-[80vh]`, and `mt-1` reported as missing, or a close equivalent. The exact wording depends on what Copilot generated, but the missing classes it finds should not change.

Now see the bug with your own eyes.

1. Open `src/App.jsx` around line 266.
2. Find the upload area in `TeamForm`.
3. Confirm that it uses `className="flex flex-col items-center gap-2 text-neutral-11"`.
4. Confirm that `.flex` is declared in `src/App.css`.
5. Confirm that `.flex-col` is not declared in `src/App.css` or `src/index.css`.
6. Start the dev server if needed.
7. Click `$ new-team`.
8. Look at the drag-and-drop area.

```sh
npm run dev
```

The Upload icon and the text lay out in a row. They should be stacked. `npm run lint` passes. `npm run build` succeeds. The bug still ships.

### Prompt 4C — write the design system

```text
/extract-ui-design write docs/design-system.md for this repository.
```

### Prompt 4D — prove automatic selection

Start a new chat. Paste this prompt.

```text
What colours and fonts does this dashboard use? I need to add a new card that matches.
```
