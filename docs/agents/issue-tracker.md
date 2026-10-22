# Issue tracker: GitHub

Issues and specs for this repo (`thschue/citizen-coding-prototype`) live as GitHub issues. Use the `gh` CLI for every operation. `gh` infers the repo from `git remote -v` when it runs inside a clone.

## Conventions

- **Create an issue**: `gh issue create --title "..." --body-file <file>`. Use a heredoc or a file for multi-line bodies.
- **Read an issue**: `gh issue view <n> --comments`.
- **List issues**: `gh issue list --state open --json number,title,labels --label <label>`.
- **Comment**: `gh issue comment <n> --body "..."`.
- **Labels**: `gh issue edit <n> --add-label "..." --remove-label "..."`. If `gh issue edit` or `gh pr edit` fails with a Projects-classic deprecation error, use `gh api -X POST|DELETE repos/thschue/citizen-coding-prototype/issues/<n>/labels…`.
- **Close**: `gh issue close <n> --comment "..."`.

## Epics and sub-issues

An epic is an issue labelled `epic` and written in PRD format. Its stories are linked as native **sub-issues**:

```bash
id=$(gh api repos/thschue/citizen-coding-prototype/issues/<child> --jq .id)
gh api -X POST repos/thschue/citizen-coding-prototype/issues/<epic>/sub_issues -F sub_issue_id=$id
```

Epics themselves are never claimed by the issue worker.

## Blocking relationships

Use GitHub's native **issue dependencies**, not prose.

- **Add an edge**: `gh api -X POST repos/thschue/citizen-coding-prototype/issues/<blocked>/dependencies/blocked_by -F issue_id=<blocker-db-id>`. `<blocker-db-id>` is the numeric database id (`gh api repos/thschue/citizen-coding-prototype/issues/<n> --jq .id`), not the `#number`.
- **Read the gate**: `gh api repos/thschue/citizen-coding-prototype/issues/<n>/dependencies/blocked_by`. An issue is unblocked once every listed blocker is `closed`.

## Picking the next issue

There is no roadmap file. "What's next" is a live query:

1. **Candidates**: `gh issue list --state open --label ready-for-agent --json number,title,labels,createdAt`. Drop `epic`s.
2. **Filter**: drop any candidate that still has an open `blocked_by` blocker.
3. **Rank**: `changes-requested` first, then `bug`, `security`, `priority`, then the oldest.

## When a skill says "publish to the issue tracker"

Create a GitHub issue.

## When a skill says "fetch the relevant ticket"

Run `gh issue view <number> --comments`.
