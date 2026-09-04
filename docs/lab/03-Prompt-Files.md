## Lab 3 — Prompt files

Goal: turn a repeated feature request into a reusable prompt.

This lab is for VS Code. Prompt files do not run on GitHub.com or in Copilot CLI.

### Steps

1. Create `.github/prompts/add-dashboard-feature.prompt.md`.
2. Paste the file content below.
3. Save the file.
4. Run Prompt 3A in Copilot Chat.
5. Check whether the agent confirms class existence before it writes code.

### File content

````markdown
---
description: Add a feature to the hackathon dashboard so that it matches the existing design system.
agent: agent
argument-hint: Describe the feature, for example "filter teams by room"
---

# Goal

Add this feature to the hackathon dashboard:

${input:feature:Describe the feature to add}

# Before you write code

1. Read [../../src/App.jsx](../../src/App.jsx). Name the component that owns
   the state you need.
2. Read [../../docs/design-system.md](../../docs/design-system.md). List the
   tokens and component classes you will reuse.
3. Search `src/App.css` for every `className` you intend to use. Confirm that
   each one is declared. Report every class you must create.

# Constraints

- Reuse the existing classes and the `useKV` hook. Do not add a dependency.
- Keep the CRT terminal look and the `$ command` label style.
- Give every new control an accessible name.
- Add new CSS to `src/App.css` only.

# Finish with

1. Run `npm run lint`. Fix everything it reports.
2. Run `npm run build`. Confirm that it succeeds.
3. A summary that lists the changed files, the classes you added, and any work
   you could not complete.
````

### Prompt 3A — run the prompt file

```text
/add-dashboard-feature a search box and a room filter for the Registered Teams section, with a live count of matching teams
```

### Note

`docs/design-system.md` does not exist yet. That is deliberate. The model should report the missing file. This sets up Lab 4.