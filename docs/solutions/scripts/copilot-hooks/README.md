# protect-paths.sh

A `preToolUse` Copilot hook that denies agent edits to protected repository files.

## Protected paths

- `package-lock.json`
- `CODEOWNERS`
- `LICENSE`
- Anything under `.github/workflows/`

## How it works

- Reads a JSON event from stdin (`toolArgs` / `tool_input`) and recursively collects any path-like string values, including common keys like `path`, `file_path`, `filePath`, `target`, `old_path`, `new_path`.
- Normalizes each candidate (slashes, relative prefixes, cwd-relative form) and checks it against the protected-path patterns.
- Outputs `{"permissionDecision":"deny",...}` if a match is found, otherwise `{"permissionDecision":"allow"}`.
- Fails closed: if Node.js is unavailable, JSON parsing fails, or an unexpected error occurs, it defaults to **allow** (input can't be evaluated) rather than exiting non-zero.

## Configuration

Registered in [`repo-policy.json`](../../hooks/repo-policy.json):

```json
{
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

## Limitation

Only file-path tool arguments are inspected. A shell tool call (e.g. `sed -i`, `rm`) can still modify protected files since command strings aren't parsed. A production policy should also gate on `toolName == "bash"` and inspect the command text.
