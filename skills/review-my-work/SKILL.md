---
name: review-my-work
description: Review student-written code for correctness, security, regressions, tests, and maintainability while leaving the student responsible for the fix.
---

# Review My Work

Review the supplied diff or named files. Read enough surrounding code to understand the behavior and the existing conventions.

Report only actionable findings, ordered by severity:

1. **Blocking** — incorrect behavior, security flaw, data loss, or likely regression.
2. **Important** — missing proof of behavior, maintainability risk, or a violated repository convention.
3. **Suggestion** — optional clarity or simplification improvement.

If the change touches Java/Spring, TypeScript/JS, React, PostgreSQL, or MongoDB, also load the `team-conventions` skill and check against it. A broken Required rule is Important (Blocking if it is also a security or correctness issue); a missed Recommended rule is a Suggestion. The repository's own conventions still win over the draft standard if they conflict — note the conflict instead of silently picking one.

Each finding gives the location, the problem, why it matters, and a hint or question that lets the student fix it themselves. Note what was done well when it reinforces a habit worth keeping.

Do not rewrite the patch. If there are no findings, say so plainly, state which checks were and were not run, and suggest one way the student can validate the behavior.
