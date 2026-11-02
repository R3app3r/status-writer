# status.md schema

One file per project: `projects/<project>/status.md`. The folder name must equal `project`.
Use exactly this YAML frontmatter + short body. No other keys, no chat history.

```markdown
---
project: exact-name
last_updated: ISO-8601
status: active | blocked | paused | done
progress: qualitative or 0-100
next_actions:
  - concrete next step
blockers: []
effort_estimate: low | medium | high
priority: 1-5
last_session: date
notes_for_planner: one sentence
---
Brief factual summary only (max 200 words). No chat history.
```

## Field rules

| Field | Rule |
|---|---|
| `project` | Exact project name, kebab-case, same as the folder name. |
| `last_updated` | ISO-8601 timestamp of this write, e.g. `2026-11-02T14:30:00+01:00`. |
| `status` | One of `active`, `blocked`, `paused`, `done`. Use `blocked` only if `blockers` is non-empty. |
| `progress` | Integer 0-100, or a short phrase (`prototype works`, `in review`). |
| `next_actions` | List of 1-5 concrete, doable steps, most important first. Empty list only when `done`. |
| `blockers` | List of short strings; `[]` when none. |
| `effort_estimate` | Remaining effort: `low`, `medium`, `high`. |
| `priority` | Integer 1-5, **1 = highest**. |
| `last_session` | Date of the last real work session, `YYYY-MM-DD`. |
| `notes_for_planner` | One sentence the planner should know (risk, dependency, deadline). |

Body: plain facts about current state, max 200 words. No transcripts, no reasoning, no code dumps.

## Example

```markdown
---
project: website-redesign
last_updated: 2026-11-02T14:30:00+01:00
status: active
progress: 60
next_actions:
  - Finish mobile nav component
  - Run Lighthouse on staging
blockers: []
effort_estimate: medium
priority: 2
last_session: 2026-11-02
notes_for_planner: Launch target is 2026-11-20; staging deploy is ready.
---
Home, about and contact pages are rebuilt and deployed to staging.
Mobile navigation and image optimisation remain.
```
