# GitHub Implementation Record Protocol

The objective is to make `GOAL / PLAN / CHANGES / TESTS / INSPECTION_STATUS / RISKS / NEXT_ACTION`
for each cycle traceable on GitHub.

## 1. Traceability (Mandatory)
- Start every task from an issue.
- Always include `Closes #<issue_number>` in the PR description.
- Always include `Refs #<issue_number>` in major commits.
- When updating `.docs/`, include the target paths in the PR description.

## 2. Cycle Records (Mandatory)
Leave a summary of each development cycle as an issue comment or PR comment.
- `GOAL:` Objective to achieve in this cycle
- `PLAN:` Execution steps (minimum task unit)
- `CHANGES:` Implementation/fix details
- `TESTS:` Executed commands and results
- `INSPECTION_STATUS:` `OK | REWORK | ACCEPT_WITH_RISK`
- `RISKS:` Regression/operational risks and impact scope
- `NEXT_ACTION:` One task to start in the next cycle

## 3. Separate Execution and Inspection Records (Mandatory)
- Separate execution assignee and inspection assignee (even if the same person, record roles separately).
- Inspection comments must include:
  - Decision (`OK | REWORK | ACCEPT_WITH_RISK`)
  - Rationale
  - Impact scope
  - Recommended action
- For `ACCEPT_WITH_RISK`, owner and due date are mandatory.

## 4. PR Quality Contract (Mandatory)
The PR description must include the following.
- Objective (GOAL)
- Change scope (CHANGES)
- Non-change scope (what is out of scope for this PR)
- Verification results (TESTS)
- Risks and mitigations (RISKS)
- Rollback procedure
- Related issue links
- `.docs/` update links
- Screenshot or preview URL when UI changes are included

## 5. Asynchronous Communication (Mandatory)
- Decisions made in Slack/chat are provisional until summarized and transferred to an issue or PR.
- Decisions not transferred to GitHub are not treated as formal decisions.

## 6. Merge Gate (Mandatory)
Merging is not allowed unless all of the following are met.
- Required CI checks pass
- Required review approval conditions are satisfied
- No unresolved critical findings
- Rollback procedure is documented
- Required `.docs/` updates are complete

## 7. Failure Tolerance and Learning Records
- Failed PRs are acceptable (as long as they are not merged).
- On failure, record cause, learnings, and prevention actions in issue or PR comments.
- Reflect prevention actions as concrete tasks in `NEXT_ACTION`.

## 8. Final Record Format (Before PR Close)
- ISSUE_LINK:
- BRANCH:
- PR_LINK:
- FINAL_INSPECTION_STATUS:
- ACCEPTED_RISKS:
- MERGE_READINESS:
- HANDOVER_NOTE:
