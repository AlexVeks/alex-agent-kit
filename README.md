# OpenCode Learner-First Configuration

This installation is configured so AI acts as a tutor, planner, debugger, reviewer, and testing coach, while you write the production code yourself.

## Installed Layout

```text
~/.config/opencode/
  AGENTS.md                        Global rules applied to every agent
  opencode.jsonc                   Selects mentor as the default agent
  agents/
    mentor.md                      Learner-first primary agent
  skills/
    plan-with-me/SKILL.md          Small, student-owned plans
    learn-by-doing/SKILL.md        Guided implementation, one step at a time
    debug-with-me/SKILL.md         Evidence-led debugging
    review-my-work/SKILL.md        Findings and hints, no rewrites
    verify-my-work/SKILL.md        Choosing and interpreting real proof
    save-to-vault/SKILL.md         Approved concept notes and vault organization
```

These are the exact paths OpenCode discovers automatically. No activation step is required.

## Usage

`mentor` is the default for sessions without an existing agent selection. Existing sessions keep their selected agent; select `mentor` in the agent picker when needed.

Skills load on demand; just describe the task and the agent picks the matching one. To force one, name it: "use `debug-with-me`".

## How Your Learning Is Protected

`AGENTS.md` sets the behavior for all agents. `agents/mentor.md` additionally enforces it with permissions, because prose alone is not a guarantee:

| Permission | Value | Effect |
|---|---|---|
| `read`, `glob`, `grep` | Built-in defaults | Ordinary inspection is allowed; `.env` and `.env.*` reads ask, except `.env.example`. |
| `edit` | `ask` | Edit, write, and patch tools request approval. |
| `external_directory` | `ask`, vault allowed | Access to the configured vault needs no extra directory approval; edits still ask. |
| `subagent` | `deny` | The mentor cannot launch child agents. |
| `shell` | `ask`, selected Git commands allowed | `git status`, `git diff`, and `git log` are allowed; `rm` and `git push` are denied. Other commands ask. |

These use native OpenCode V2 permission rules. The last matching rule wins. Choosing **Allow always** can save a project-scoped approval for later requests; use **Allow once** when you want each operation reviewed.

The built-in `build` agent keeps its normal permissions for implementation work. `AGENTS.md` includes an escape hatch: explicitly ask for the work to be done for you. Tool permissions still apply.

## Knowledge Vault

The mentor and `save-to-vault` use `C:\Users\alexvek\Documents\ObsidianVaults\web`. The skill searches for existing notes, proposes the exact changes and related links, and waits for approval before writing. The vault directory exception permits access, not automatic edits.

## Working Agreement

1. Start with `plan-with-me`, `debug-with-me`, or `learn-by-doing`.
2. Write the next small change yourself.
3. Ask `review-my-work` for findings, explanations, or a hint.
4. Use `verify-my-work` to pick and run the smallest useful proof.
5. Request a full solution only when you want a worked example, then read the explanation.

## Origin

Derived from `td-agentcraft-kit`, an autonomous multi-agent delivery kit. Removed because their purpose is to do the work for you: `code-workflow`, `implementer`, `planner`, `specialized-agent`, `delegate`, the Architect/Builder/Reviewer/Refactorer/Specifier/Investigator agents, `static-code-analysis`, `preflight`, `notify`, and `work-item-tracking`.

Kept and rewritten for learning: evidence-based investigation (`debug-with-me`), proof of work (`verify-my-work`), requirement questioning (`plan-with-me`), and review standards (`review-my-work`).
