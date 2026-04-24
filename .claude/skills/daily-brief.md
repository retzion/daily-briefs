---
name: daily-brief
description: Show the latest agentic coding daily brief, or list available reports.
args: "[list | YYYY-MM-DD | search term | (empty for latest)]"
---

# Daily Brief Skill

Fetch and display daily briefing reports from the GitHub repo `retzion/daily-briefs`. This skill does NOT use any local files — all content is pulled live from GitHub.

## Behavior

Parse the argument passed via `$ARGUMENTS`:

### Default / latest (no argument, or "latest")

1. Use Bash to run: `gh api repos/retzion/daily-briefs/contents/reports --jq '.[].name' | sort`
2. Take the last filename (lexicographic sort = most recent date).
3. Fetch its content: `gh api repos/retzion/daily-briefs/contents/reports/<filename> --jq '.content' | base64 -d`
4. Display the decoded markdown directly.

### List mode (`list`)

1. Use Bash to run: `gh api repos/retzion/daily-briefs/contents/reports --jq '.[].name' | sort -r`
2. Display as a bulleted list, marking the first entry as `(latest)`:
   ```
   Available daily briefs:
   - 2026-04-22 (latest)
   - 2026-04-21
   - 2026-04-16
   ```

### Date mode (argument matches YYYY-MM-DD)

1. Fetch: `gh api repos/retzion/daily-briefs/contents/reports/YYYY-MM-DD.md --jq '.content' | base64 -d`
2. If the API returns a 404, tell the user that date has no report and suggest running `/daily-brief list`.
3. Otherwise display the decoded markdown.

### Search mode (any other argument)

1. Use Bash to run: `gh search code "<search term>" --repo retzion/daily-briefs --json path,textMatches`
2. Display the matching filenames and relevant excerpts.
3. If no results, say so.

## Output

Display report content directly as markdown — do not wrap it in code blocks. Keep commentary minimal; the report speaks for itself.
