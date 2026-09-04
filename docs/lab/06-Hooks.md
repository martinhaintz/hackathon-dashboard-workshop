## Lab 6 — Hooks

Goal: add a deterministic guardrail for protected files.

This lab uses Copilot CLI. Restart Copilot CLI after you add or change hooks.

### Steps

1. Create `.github/hooks/repo-policy.json`.
2. Create `scripts/copilot-hooks/protect-paths.sh`.
3. Make the script executable.
4. Restart Copilot CLI in the repository.
5. Run Prompt 6A.
6. Run Prompt 6B.
7. Read the denial message.
8. Optionally add `src/index.css` to the protected pattern.

### `.github/hooks/repo-policy.json`

```json
{
  "version": 1,
  "hooks": {
    "preToolUse": [
      {
        "type": "command",
        "bash": "bash scripts/copilot-hooks/protect-paths.sh",
        "timeoutSec": 5
      }
    ]
  }
}
```

### `scripts/copilot-hooks/protect-paths.sh`

```bash
#!/usr/bin/env bash
# Deny agent edits to protected repository files.
# preToolUse command hooks are fail-closed, so always exit 0.
set -uo pipefail

PROTECTED='(^|/)(package-lock\.json|CODEOWNERS|LICENSE)$|(^|/)\.github/workflows/'

paths="$(node -e '
let raw = "";
process.stdin.on("data", d => raw += d);
process.stdin.on("end", () => {
  let event;
  try { event = JSON.parse(raw); } catch { process.exit(0); }
  const args = event.toolArgs ?? event.tool_input ?? {};
  const keys = ["path", "file_path", "filePath", "target", "old_path", "new_path"];
  const out = keys.filter(k => typeof args[k] === "string").map(k => args[k]);
  process.stdout.write(out.join("\n"));
});
')"

while IFS= read -r p; do
  [ -z "$p" ] && continue
  if printf '%s' "$p" | grep -Eq "$PROTECTED"; then
    printf '{"permissionDecision":"deny","permissionDecisionReason":"Repository policy protects %s. Ask a maintainer."}' "$p"
    exit 0
  fi
done <<< "$paths"

printf '{"permissionDecision":"allow"}'
```

Make the script executable.

```sh
chmod +x scripts/copilot-hooks/protect-paths.sh
```

### Prompt 6A — confirm normal work is allowed

```text
Add a short comment above the formatTime helper in src/App.jsx that explains its input and output.
```

### Prompt 6B — confirm the policy denies

```text
Bump the node-version in .github/workflows/deploy.yml from 20 to 22.
```

### Discussion

This hook inspects file-path arguments. An agent could still reach a protected file through a `bash` tool call. A production policy also matches on `toolName == "bash"` and inspects the command string.

Command `preToolUse` hooks fail closed on a non-zero exit. They fail open on timeout. A slow hook is not a security control.