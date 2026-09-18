# Planner/Coder/Tester

A collection of three subagents designed to work together whilst keeping context clean.

Example `AGENTS.md` usage:
```markdown
## Change verification policy

For every task that changes repository files and includes any build or
verification activity, the primary agent must use the `tester` subagent as the
sole owner of verification. This applies whether or not the planner/coder
workflow below is used; do not run builds, tests, linting, type checks,
packaging, or other verification commands directly. A task with no build or
verification activity (including, but not limited to, markdown and documentation changes)
may omit the tester.

Before any verification command is run, give the tester the requirements,
changed-file list, and planned verification. The tester must not modify
product source, tests, configuration, or documentation, revert other agents'
work, or widen scope. It returns a concise pass/fail/blocked verdict with
reproduction steps and only the log excerpts needed to diagnose a failure.

If the tester reports a defect, the agent responsible for implementation makes
a narrowly scoped repair without running verification, and the tester reruns
the affected checks and any required build/test suite afterwards. The primary
agent must inspect the final diff and confirm the tester's verification
evidence before reporting completion.

## Planner/coder/tester subagent workflow

Use this workflow only when the user explicitly asks for the planner and coder
subagents. The primary agent owns coordination and must use the tester for every
build or verification path in this workflow; do not ask the user to choose
whether the tester is needed. A task with no build or verification activity may
omit the tester.

1. The primary agent owns requirements, decisions, and the final result. Spawn
   the `planner` first and give it the complete request and relevant constraints.
2. Wait for the planner's evidence-backed plan. Resolve material user decisions
   before implementation; do not silently convert assumptions into requirements.
3. Give the `coder` the original request, the complete accepted plan, applicable
   constraints, and explicit ownership of files or responsibilities. The coder
   implements the change but does not run builds, tests, or other verification
   commands. Do not run the planner and coder in parallel.
4. If the task includes any build or verification activity, spawn a `tester`
   immediately after the coder completes and before any such command is run.
   This is mandatory even for trivial changes. The tester is the sole owner of
   build and verification execution, including compilation, packaging, linting,
   type-checking, unit tests, integration tests, contract tests, smoke tests,
   regression tests, simulator checks, and end-to-end tests. It must not modify
   product source, tests, configuration, or documentation, revert other agents'
   work, or widen scope. Give it the accepted requirements, changed-file list,
   and planned verification. It returns a concise pass/fail/blocked verdict
   with reproduction steps and only the log excerpts needed to diagnose a
   failure. Do not run the coder and tester in parallel.
5. If the tester reports a defect, the primary agent gives the coder a narrowly
   scoped repair task without asking the coder to run verification; the tester
   reruns the affected checks and any required build/test suite afterwards. The
   primary agent must inspect the final diff, review plan deviations, and
   confirm the tester's verification evidence before reporting completion.

Subagent sandbox settings are role defaults, not security boundaries. Continue
to respect the permission mode and approvals selected for the parent turn.
```

For iOS projects, consider including this in the "Change verification policy" section:
```markdown
If simulator-dependent verification is blocked because no iOS simulator is
booted, surface that blocker to the user and ask them to boot an iOS simulator
and confirm. After confirmation, resume the same tester subagent so it can
re-check simulator availability and retry the blocked checks; do not run them
directly or treat the confirmation itself as verification evidence.
```
