---
name: status-writer
description: Write or update a short project status.md for cross-model planning. Trigger when the user says "update status", "write status", "status update", or at the end of significant work. Do not use on routine turns.
license: MIT
metadata:
  author: Marton Rozsas
  version: "1.0"
---

# Status Writer

Write a short, strict `status.md` that other models can read cheaply.

## Configuration
This skill folder ships the format (`schema.md`) and the aggregator (`scripts/aggregate.py`).  
Status data lives in `STATUS_ROOT`, an environment variable holding the absolute path of the status folder.  
If it is unset, ask the user once for the path and remember it for the session.

Expected layout:
```
$STATUS_ROOT/
├── projects/<project-name>/status.md
└── summary.md          (written by the aggregator)
```

## Steps
1. Determine the project name (kebab-case). Reuse an existing folder under `$STATUS_ROOT/projects/` if it matches. If unclear, ask once.
2. Read the existing `status.md` (if any) and preserve unchanged fields (e.g. `priority`).
3. Get the current time (do not guess):
   ```
   python -c "import datetime; print(datetime.datetime.now().astimezone().isoformat(timespec='seconds'))"
   ```
4. Write `$STATUS_ROOT/projects/<project>/status.md` following `schema.md` in this skill folder exactly. Read the schema if it is not already in context.
5. Run the aggregator from this skill folder:
   ```
   python <skill-folder>/scripts/aggregate.py --root "$STATUS_ROOT"
   ```
   Fix and re-run if it reports warnings or failures for this project.

## Rules
- Folder name must equal the `project` field.
- Body ≤ 200 words, facts only. No chat history, reasoning, or code dumps.
- `priority`: integer 1–5 (1 = highest).
- `status: blocked` requires at least one item in `blockers`.
- `last_updated` = timestamp from step 3. `last_session` = today (`YYYY-MM-DD`).
- `next_actions`: 1–5 concrete steps, most important first.
- Write only when asked or when real work has ended. Never write on every turn.

## No file access
If the environment cannot write files, output the full `status.md` in a single code block and name the target path so the user can save it and run the aggregator.

## Output
Confirm the path, show the frontmatter, and report the aggregator result. Do not paste the body unless asked.
