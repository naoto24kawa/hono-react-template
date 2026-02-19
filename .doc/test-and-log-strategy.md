# Test and Log Strategy

## Purpose
- Minimize standard output during agent execution and return only short, decision-relevant information.
- Store detailed test results, build logs, and failure reproduction details in separate files.
- Keep both a "summary" and "reference paths" on GitHub (Issue/PR) to ensure reproducibility.

## Core Policy
- Keep `stdout/stderr` to summaries only. Do not paste long logs directly into chats or PR bodies.
- Save details under `artifacts/`, and include only file paths in summaries.
- Use a machine-friendly format for error lines.
  - Format: `ERROR: <category>: <reason>`
- Pre-calculate aggregates (counts, success rate, duration) in the harness, and do not make the agent recalculate them.

## Directory Conventions
- `artifacts/test/`: test execution results
- `artifacts/logs/`: build/runtime logs
- `artifacts/summary/`: cycle summaries (short text)
- `artifacts/test/latest.json`: latest test summary (machine-readable)
- `artifacts/logs/latest.log`: latest detailed log (human-readable)

Examples:
- `artifacts/test/2026-02-12T17-10-00Z-fast.json`
- `artifacts/logs/2026-02-12T17-10-00Z-build.log`

## Output Contract (Terminal/PR Comment)
Only show the following per cycle.
- `TEST_MODE: fast | full`
- `RESULT: pass=<n> fail=<n> skip=<n> duration=<sec>`
- `ERRORS: <count>` (can be omitted when 0)
- `ARTIFACTS: <summary_path> <test_path> <log_path>`
- `NEXT_ACTION: <one line>`

If there is any failure, add exactly one extra line:
- `ERROR: <category>: <primary_reason>`

## Test Execution Modes
- `fast` (default):
  - For regression checks. Run a fixed subset equal to 1-10% of the total.
  - Use the same subset every time within the same agent (to keep diffs comparable).
- `full`:
  - Run before PR, before release, or after major changes.

## Subset Design Rules
- Manage `fast` targets as a fixed list, not random.
- Maintain different fixed lists by role/owner, and ensure coverage at the whole-team level.
- When adding test cases, update bias in the `fast` list.

## Log Separation Rules
- Keep progress output low-frequency (for example, every 30-60 seconds).
- Write verbose warnings and stack traces to `artifacts/logs/*.log`.
- Use `latest.*` for the latest reference. Keep timestamped files as history.
- Rotate large logs so a single file does not grow too large.

## GitHub Recording Rules (Minimum Requirements of This Strategy)
- In PR bodies, include summaries only and clearly point to references under `artifacts` for details.
- Leave inspection judgment in PR comments:
  - `OK | REWORK | ACCEPT_WITH_RISK`
- For `ACCEPT_WITH_RISK`, owner and deadline are mandatory.
- If decisions were made in Slack or elsewhere, summarize and transfer them to Issue/PR as final records.

## CI Gate
- Automatically run regression tests on every change.
- If tests fail, merging is not allowed.
- CI output must also follow "summary + link to detailed logs".

## Role-specific Guide
- Implementer:
  - Run `fast` for each change, and keep detailed logs when failures occur.
- Verifier:
  - Perform independent inspection with `full` or expanded tests for the needed scope.
- Recorder:
  - Record summary in PR, judgment in comments, and file references for details.

## Minimal Template
```text
TEST_MODE: fast
RESULT: pass=128 fail=2 skip=4 duration=93
ERRORS: 2
ERROR: parser: invalid token handling regression
ARTIFACTS: artifacts/summary/2026-02-12T17-10-00Z.txt artifacts/test/2026-02-12T17-10-00Z-fast.json artifacts/logs/2026-02-12T17-10-00Z-build.log
NEXT_ACTION: fix parser token boundary in lexer state machine
```

## Rollout Steps (Recommended Order)
1. Create `artifacts/` convention directories.
2. Add `fast/full` modes and summary output to the test harness.
3. Standardize errors to `ERROR: <category>: <reason>` format.
4. Configure CI to block merges on failure.
5. Add a "summary + references" section to the PR template.
