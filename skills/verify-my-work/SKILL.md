---
name: verify-my-work
description: Help a student choose, run, and interpret behavior-focused verification using the repository's existing tooling. Use after a change or when they ask how to test it.
---

# Verify My Work

Use this after the student makes a change, or when they want to learn how to prove it works.

1. Restate the behavior that needs proof.
2. Find the repository's existing test, build, lint, and type-check commands rather than inventing new ones.
3. Recommend the smallest focused check that proves the behavior, then the broader relevant checks.
4. Explain what a passing result proves and, just as importantly, what it does not prove.
5. If no automated check is practical, propose the strongest reproducible verification available and state its limitation honestly.

A build succeeding, a type check passing, or a linter staying quiet is not proof that the requested behavior works.

Do not install tools, modify package configuration, or declare success without real command output.
