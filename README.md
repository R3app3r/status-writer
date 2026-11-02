# status-writer

A lightweight Claude skill that keeps a machine-readable project status file so other models or sessions can plan work without re-reading long chat history.

> **Built to be cheap to run, for everyday use.**
> Each status file is capped at a short frontmatter block plus 200 words, and a small Python script (not the model) validates them and builds one `summary.md`. A planner model reads that single file instead of long chat histories, which keeps token use low.
> It is a simple everyday helper for keeping several projects in view, not a project-management product. Check the summary before relying on it.

## Main description
Writes a short, strict `status.md` (YAML frontmatter + brief body) for each project. An aggregator script then produces a single `summary.md` that a planner model can read in one pass. Designed for token efficiency and cross-model coordination.

## Use cases
- End-of-session or end-of-day status updates across multiple Claude projects (chat, Code, Cowork).
- Feeding a planner model that only reads one aggregated `summary.md`.
- Keeping status consistent when switching between different Claude surfaces or long-running sessions.
- Providing a stable, low-token hand-off between models.

## Best practices
- Keep status files short and factual; the aggregator already produces the summary.
- Update only on meaningful progress or when explicitly requested ("update status").
- Set `STATUS_ROOT` once as an environment variable so every session finds the same location (see `env.example`).
- Pair with a short custom instruction: "At the end of significant work or when I say 'update status', use the status-writer skill."
- Run the aggregator after every status write so `summary.md` stays current.
- Prefer kebab-case project names that match the folder name.

## Installation
1. Place this folder at `~/.claude/skills/status-writer/` (or the equivalent skills directory for your Claude setup).
2. Install the one dependency (Python 3.8+): `pip install -r requirements.txt`.
3. Create a status folder anywhere (for example `~/status`) and set `STATUS_ROOT` to its absolute path, either in your shell profile or system environment variables, or in Claude Code's `settings.json`:
   ```json
   { "env": { "STATUS_ROOT": "/absolute/path/to/status" } }
   ```

The skill folder holds the format (`schema.md`) and the aggregator (`scripts/aggregate.py`); `STATUS_ROOT` holds only your data (`projects/` and `summary.md`). Run the aggregator by hand with `python scripts/aggregate.py --root /absolute/path/to/status`. No network access or API keys are used.

## Author
Marton Rozsas · marton.rozsas@gmail.com  
Feedback and issue reports are welcome.

## License
MIT, see [LICENSE](LICENSE). Provided as is, without warranty.
