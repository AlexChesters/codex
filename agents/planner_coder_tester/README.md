# Planner/Coder/Tester

A collection of three subagents designed to work together whilst keeping context clean.

Example `AGENTS.md` usage:
```
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
