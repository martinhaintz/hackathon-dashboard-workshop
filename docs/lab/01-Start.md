## Lab 1 — Baseline and Audit

Goal: See the failure before you add guidance.

### Steps

1. Open the repository in VS Code.
2. Confirm that you are on `workshop/00-start`.
3. Open Copilot Chat in agent mode.
4. Paste Prompt 1A.
5. Run the app.
6. Look at the result.
7. Do not fix the result.
8. Paste Prompt 1B.
9. Save the audit output for discussion.

### Prompt 1A — baseline

```text
Add a search box and a room filter to the Registered Teams section of the dashboard. Users must be able to type text that matches a team name or topic, pick a room from a dropdown, and see a count of matching teams. Show a message when nothing matches.
```

Run the app after the agent stops.

```sh
npm run dev
```

Check the result.

### Prompt 1B — audit

```text
You are auditing this repository for AI readiness. Do not change any files.

Answer these questions with file and line evidence:
1. Which CSS framework does this project use? Prove it from package.json.
2. List every class name used in src/App.jsx that is NOT declared in src/App.css.
3. src/index.css and src/App.css both set :root. Which values conflict, and
   which file wins at runtime? Explain why.
4. Which command runs the tests?
5. Does the value of `base` in vite.config.js match the repository name?
6. Name three statements in README.md that the code contradicts.
7. Run `npm run lint`. Does it pass on a clean checkout? If not, quote each
   error. Were these errors caused by your change, or were they already there?

Return a table: Question | Answer | Evidence | Risk to an AI agent.
```

### Discussion

- Did Copilot use CSS classes that do not exist?
- Did it keep the `$ ` label prefix?
- Did it add a dependency?
- Did it invent a test command?
- Did lint fail before your change?

A validation command must be green before you tell an agent to trust it. This is a general AI-readiness rule. If the baseline is red, the agent cannot know whether it caused the error.
