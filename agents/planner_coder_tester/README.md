# Planner/Coder/Tester

A collection of three subagents designed to work together whilst keeping context clean.

Example `AGENTS.md` usage:
```
## Planner/coder/tester subagent workflow

Use this workflow only when the user explicitly asks for the planner and coder
subagents. The primary agent independently decides whether the change warrants
the tester; do not ask the user to make that choice. Handle small or obvious
changes directly in the primary agent.

1. The primary agent owns requirements, decisions, and the final result. Spawn
   the `planner` first and give it the complete request and relevant constraints.
2. Wait for the planner's evidence-backed plan. Resolve material user decisions
   before implementation; do not silently convert assumptions into requirements.
3. Give the `coder` the original request, the complete accepted plan, applicable
   constraints, and explicit ownership of files or responsibilities. Do not run
   the planner and coder in parallel.
4. For non-trivial behavior changes, spawn a `tester` after the coder completes.
   The tester owns verification only: it must not modify product source, revert
   other agents' work, or widen scope. Give it the accepted requirements, the
   changed-file list, and the planned verification. It should run the relevant
   build, tests, and focused manual or end-to-end checks, then return a concise
   pass/fail/blocked verdict with reproduction steps and only the log excerpts
   needed to diagnose a failure. Do not run the coder and tester in parallel.
5. If the tester reports a defect, the primary agent gives the coder a narrowly
   scoped repair task; the tester reruns the affected checks afterwards. The
   primary agent must inspect the final diff, review plan deviations, and
   confirm the verification evidence before reporting completion.

Use the tester for API changes, authentication, persistence, infrastructure,
navigation or state-management changes, and substantial UI behavior changes.
It is optional for trivial, low-risk edits such as isolated copy changes.

Subagent sandbox settings are role defaults, not security boundaries. Continue
to respect the permission mode and approvals selected for the parent turn.
```
