---
name: daily-brief
description: Show the latest agentic coding daily brief, or list available reports.
args: "[list | YYYY-MM-DD | (empty for latest)]"
---

# Daily Brief Skill

Read and display daily briefing reports from the `reports/` directory in this repository.

## Behavior

1. **Parse the argument** passed via `$ARGUMENTS`:
   - If the argument is empty or "latest": show the most recent report.
   - If the argument is "list": list all available reports by date (most recent first), with the first line of each as a preview.
   - If the argument matches a date pattern (YYYY-MM-DD): show that specific report.
   - Otherwise, treat it as a search term and grep across all reports for matching lines, showing context.

2. **Show the most recent report** (default):
   - Use `Glob` to find all `reports/*.md` files.
   - Sort by filename (dates sort lexicographically) and pick the last one.
   - Read and display the full contents of that file.

3. **List mode** (`list`):
   - Find all `reports/*.md` files.
   - For each file, extract the date from the filename and the H1 header line.
   - Display as a bulleted list, most recent first, like:
     ```
     Available reports:
     - 2026-04-21 (latest)
     - 2026-04-16
     - 2026-04-15
     - 2026-04-14
     ```

4. **Date mode** (e.g. `2026-04-16`):
   - Read and display `reports/YYYY-MM-DD.md`.
   - If not found, say so and suggest running `list`.

5. **Search mode** (anything else):
   - Grep all files in `reports/` for the argument, showing matches with filenames and context.
   - Summarize which reports matched.

## Output

Display the report content directly as markdown — do not wrap it in code blocks. Keep any commentary minimal; the report speaks for itself.
