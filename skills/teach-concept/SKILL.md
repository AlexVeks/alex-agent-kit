---
name: teach-concept
description: Teach a concept so it is understood, not memorized. Use when the student says "teach me X", "help me understand X", "explain X so it sticks", or asks for a deep explanation of a concept. Not for coding tasks, debugging, or reviews.
---

# Teach Concept

Goal: the student ends with **connected** knowledge (a few core truths that generate the rest), not a pile of lone facts. The felt result is "the click": many facts collapse into a few ideas.

Use this for concepts ("teach me how HTTP works", "teach me recursion"). For a concrete coding task use `learn-by-doing`; for a bug use `debug-with-me`.

## Two principles

**i. Unconditional truths first.** Start from facts the student can accept at face value, with no caveats ("ALL communication between computers is done by sending packets"). They are the safest to lock in. If a truth needs "usually..." or "it depends", dig deeper for a simpler one. Prefer universal statements ("all X are Y") and *real* definitions (not a list of tendencies). Say "unconditional truth" by default; use "axiom" only for facts that follow from nothing else.

**ii. "How could I have discovered this?"** Facts feel arbitrary when they seem decreed. Walk through how the student could have found the idea alone. Motivate every step: why are we doing this at all, why try this formula, why this move? Nothing appears from nowhere (3Blue1Brown style).

## Process: probe, plan, teach

Scale the *size* of each phase to the topic; keep the *shape*.

### 0. Accuracy first
Do not state any fact, name, date, formula, or definition you are even slightly unsure of. Check it with `websearch`/`webfetch` (or read the code, if the topic is in the repository) before saying it. If a check corrects what you were about to teach, say so plainly. A wrong foundation corrupts everything built on it.

### 1. Probe (do not skip)
Find where the student's knowledge ends before teaching. Ask **one question at a time** with the `question` tool, with options where possible.

- Start with 2-3 questions; add more only if the edge is still unclear.
- Find a floor (something they get right) and a ceiling (something they miss or do not know). All-correct means the questions were too easy, so go sharply harder.
- One wrong answer is not a verdict. Probe around it: slip, gap, or misconception? A confidently held wrong model must be dislodged, not topped up.
- Include "I don't know" as an option and treat it as information, not failure.
- Then ask what they actually want from the topic ("understand LLMs" can mean ten things). Goal questions have no right answer.

The `question` tool does not grade. You grade the answer yourself in your next message: say right or wrong, give the correct answer, and give a one-line reason.

### 2. Plan
Reason about the best path for this topic and this student, from what the probe found.

- What are the unconditional truths it rests on? Which does the student already hold?
- What is the motivated path from those truths to their goal?
- Socratic (they attempt the discovery first; default when they can plausibly reason it out) or expository (you narrate; use when it is beyond cold reasoning or they are low on energy)?
- Stress-test the roots: is each foundation truly unconditional *for them*, or a disguised theorem? If it derives from something simpler, push it down.

Present the plan in chat: a few sentences of approach, plus a **dependency map** as a plain indented list with arrows (roots first, goal last; few nodes, short labels). Example:

```
packets (root)
  -> routing
  -> ordering + retransmit
       -> reliable stream (goal)
```

For a small topic, a two-line plan is enough. For a large one, **wait for the student's go-ahead** before teaching; fixing a wrong root is cheap now and expensive mid-lesson.

### 3. Teach (loop per node)
For every node, foundation or derived:

1. **Motivate**: why do we need this now? What gap does it close?
2. **Establish**: state a foundation plainly, or build a derived step from what is already in place.
3. **Connect**: show exactly which earlier nodes it depends on.
4. **Check**: one `question` to confirm it landed. If it did not, stop and repair before building on it.

Never front-load all foundations and stop checking. If you catch yourself asking the student to take a fact on faith, either motivate it and check it, or ground it in something already established.

### 4. Close
- Give a 3-5 line recap that walks the dependency map end to end.
- Ask one **transfer question**: a new situation the lesson never covered, to test understanding rather than recall.
- Offer to save the key idea with `save-to-vault` (never write without approval).
- Suggest a short re-check in a day or two ("ask me about X again") to make it stick.

## Writing check questions

Build options so the right one is not obvious from its shape:

1. Every option is a **bare claim** with no justification. Put all reasoning in your feedback after they answer.
2. Write the correct claim first, then mutate it into each distractor using one real misconception, in the same skeleton, length, and register.
3. Each distractor is a mistake the student might actually make (so the pick is diagnostic), yet clearly wrong on the intended reading.
4. No asymmetric bolding or detail. If you could pick the answer without knowing the material, rewrite the set.

Put the correct answer only in your feedback message, never in the question text.

## Formatting

- Use LaTeX for math: inline `$f(x)$`, display `$$ ... $$` on its own lines. The vault renders it.
- Plain language first; define a term briefly on first use.
- Keep each message short. One idea, one question.
- Do not add diagrams. The dependency map above is the only visual.

## Mentor rules still apply

`AGENTS.md` still applies: the student attempts first, you do not write their production code, and you separate facts from inferences. This skill changes how you *explain*, not who writes the code.
