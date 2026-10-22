# Triage Labels

The skills speak in terms of canonical triage roles. This file maps those roles to the label strings used in this repo.

| Role (mattpocock/skills) | Label in our tracker | Meaning |
| --- | --- | --- |
| `needs-triage` | `needs-triage` | Maintainer (or the PM audit loop) needs to evaluate this issue |
| `needs-info` | `needs-info` | Waiting on a fact or a decision |
| `ready-for-agent` | `ready-for-agent` | Fully specified, ready for an AFK agent |
| `ready-for-human` | `ready-for-human` | Requires human implementation or access |
| `wontfix` | `wontfix` | Will not be actioned |
| | `epic` | PRDs and grouping issues; stories are sub-issues |
| | `priority` | "Do this next", at most 2 open at once |
| | `question` | Escalation: a maintainer decision is needed, with a recommendation in the comment |

## Loop labels

These are owned by the loops (`docs/agents/issue-worker-loop.md`, `docs/agents/security-review-loop.md`):

| Label | Set by | Meaning |
| --- | --- | --- |
| `implementation-in-progress` | issue worker | The worker owns the issue. Cleared at hand-off |
| `awaiting-security-review` | issue worker | The PR is green and ready, waiting for review |
| `security-review-in-progress` | security review | Review running. Don't push to the branch |
| `security-approved` | security review | Review passed and auto-merge is armed |
| `changes-requested` | security review / PM audit | Sent back to the worker. The instruction is in the latest marked PR comment |
| `security-blocked` | security review | A HIGH/MEDIUM security finding |
