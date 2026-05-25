---
description: Get up to speed on recent repo activity — commits, open issues, merged PRs, working-tree state.
allowed-tools: Bash
---

You are priming yourself on the current state of this repository so you can
help the user productively without asking them to re-explain context. Be
concise — the goal is a state-of-affairs briefing, not an essay.

Run the following in parallel (single message, multiple Bash calls):

1. `git log --oneline -20` — recent local commits.
2. `git status --short` — uncommitted changes.
3. `git branch -vv` — local branches and their tracking state.
4. `git diff --stat HEAD~5..HEAD 2>/dev/null || git diff --stat` — what's
   churned recently.
5. `gh pr list --state merged --limit 10 --json number,title,mergedAt,author --template '{{range .}}#{{.number}} {{.title}} (merged {{timeago .mergedAt}} by @{{.author.login}}){{"\n"}}{{end}}'` — recently merged PRs.
6. `gh pr list --state open --limit 10 --json number,title,author,createdAt,isDraft --template '{{range .}}#{{.number}} {{.title}} ({{if .isDraft}}draft, {{end}}opened {{timeago .createdAt}} by @{{.author.login}}){{"\n"}}{{end}}'` — open PRs.
7. `gh issue list --state open --limit 15 --json number,title,labels,author,createdAt --template '{{range .}}#{{.number}} {{.title}} (opened {{timeago .createdAt}} by @{{.author.login}}){{"\n"}}{{end}}'` — open issues.

If any `gh` call fails because there is no GitHub remote or `gh` is not
authenticated, note that once and skip the remaining GitHub calls — don't
retry. If this is not a git repo, say so and stop.

Then synthesize a briefing in this exact shape (omit empty sections):

```
## State of affairs

**Branch:** <current branch> · <ahead/behind tracking, or "no upstream">
**Working tree:** <clean | N files modified, summary>

**Recent commits (last <N>):**
- <one-line each, most recent first; group obvious themes if it helps>

**Merged recently:**
- #123 <title> — <date> by @<author>

**Open PRs:**
- #456 <title> — <draft?> · @<author> · <age>

**Open issues:**
- #789 <title> — @<author> · <age>
  (cluster by label if a clear theme emerges)

**What I'd suggest looking at:** <one or two sentences — point at the
most load-bearing thing: a stale PR, a blocking issue, an in-progress
change, or "nothing on fire, pick a thread.">
```

Keep the briefing under ~30 lines total. Don't quote raw command output;
synthesize it. Don't recommend actions beyond the single "what I'd suggest"
line — the user will steer from here.
