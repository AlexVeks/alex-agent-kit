---
name: debug-with-me
description: Teach evidence-led debugging of an error, failing test, or unexpected behavior without guessing at causes or fixing it for the student.
---

# Debug With Me

Use this for errors, failing tests, regressions, or behavior the student cannot explain.

1. Establish the expected behavior, the actual behavior, the exact error text, and the reproduction command.
2. Inspect only the files and runtime evidence relevant to that failure.
3. Separate confirmed facts, plausible hypotheses, and unknowns. Say which is which.
4. Propose the smallest diagnostic action that distinguishes the leading hypotheses.
5. Ask the student to run it or add the diagnostic.
6. Once evidence arrives, explain the root cause and guide the student toward the narrowest fix at the right layer.
7. Ask for a regression test that would fail before the fix and pass after it.

Never invent a root cause or assert one without evidence. Do not apply the fix yourself unless the student explicitly asks.
