# OpenCode Learner-First Configuration

This installation is configured so AI acts as a tutor, planner, debugger, reviewer, and testing coach, while you write the production code yourself.

## Installed Layout

```text
~/.config/opencode/
  AGENTS.md                        Global rules applied to every agent
  opencode.jsonc                   Selects mentor as the default agent
  agents/
    mentor.md                      Learner-first primary agent; also maintains the Obsidian vault
    scout.md                       Read-only search subagent (Haiku); mentor verifies its reports
  skills/
    plan-with-me/SKILL.md          Small, student-owned plans
    learn-by-doing/SKILL.md        Guided implementation, one step at a time
    debug-with-me/SKILL.md         Evidence-led debugging
    review-my-work/SKILL.md        Findings and hints, no rewrites
    verify-my-work/SKILL.md        Choosing and interpreting real proof
    save-to-vault/SKILL.md         Approved concept notes and vault organization
    organize-vault/SKILL.md        Drains the TO_ORGANIZE inbox, fixes broken links
    team-conventions/SKILL.md      Reference coding standard for Java/Spring, TS/JS, React, Postgres, MongoDB
```

These are the exact paths OpenCode discovers automatically. No activation step is required.

## Usage

`mentor` is the default for sessions without an existing agent selection. Existing sessions keep their selected agent; select `mentor` in the agent picker when needed.

Skills load on demand; just describe the task and the agent picks the matching one. To force one, name it: "use `debug-with-me`".

## How Your Learning Is Protected

`AGENTS.md` sets the behavior for all agents. Permissions enforce it, because prose alone is not a guarantee. The baseline below lives in `opencode.jsonc`'s top-level `permissions` and applies to every agent, including the hidden built-ins:

| Permission | Value | Effect |
|---|---|---|
| `read`, `glob`, `grep` | Built-in defaults | Ordinary inspection is allowed; `.env` and `.env.*` reads ask, except `.env.example`. |
| `edit` | `ask` | Edit, write, and patch tools request approval. |
| `subagent` | `deny` | No agent can launch child agents. |
| `shell` | `ask`, selected commands allowed | `git status/diff/log/branch/show/grep/ls-files`, plus `ls`, `head`, `Select-Object`, `Select-String`, are allowed; `rm` is denied. Other commands ask. |

`agents/mentor.md` only lists what differs from that baseline:

- `external_directory` asks everywhere except the vault (allowed there, edits still ask).
- `git push` is denied; you run git commands yourself.
- A scoped `Remove-Item *TO_ORGANIZE*` is allowed for inbox cleanup.

See `agents/mentor.md` for the exact rules.

These use native OpenCode V2 permission rules. The last matching rule wins. Choosing **Allow always** can save a project-scoped approval for later requests; use **Allow once** when you want each operation reviewed.

The built-in `build` agent keeps its normal permissions for implementation work. `AGENTS.md` includes an escape hatch: explicitly ask for the work to be done for you. Tool permissions still apply.

## Knowledge Vault

The mentor and `save-to-vault` use `C:\Users\alexvek\Documents\Obsidian\Office`. The skill searches for existing notes, proposes the exact changes and related links, and waits for approval before writing. The vault directory exception permits access, not automatic edits.

The mentor creates brand-new notes directly in `TO_ORGANIZE`, unlinked, by default — no topic-folder search, no `related` links, no `_Index.md` update at creation time. Updating an existing note still happens in place as normal. Finding the right home, cross-linking, and updating `_Index.md` only happens when you explicitly ask to organize the vault, which loads `organize-vault` to drain `TO_ORGANIZE` and sweep for broken links.

## Validating The Kit

After editing `opencode.jsonc`, any `agents/*.md`, or any `skills/*/SKILL.md`, check before trusting it:

1. `opencode.jsonc` must stay valid JSONC (balanced braces, no trailing comma OpenCode's parser rejects) — reread the file after editing.
2. Every skill named in `README.md`, `AGENTS.md`, or an agent's body (e.g. `plan-with-me`, `team-conventions`) must have a matching `skills/<name>/SKILL.md` on disk — grep the name if unsure.
3. Every permission rule removed from an agent's frontmatter because it moved to `opencode.jsonc`'s top-level `permissions` must still be covered there — diff the two files if you change either.
4. Start a session and ask the agent to state its own rules in one sentence; a garbled or missing frontmatter block usually surfaces as the agent ignoring its constraints.

There's no build or test command for this repo (it's Markdown and JSONC), so this is a manual checklist, not an automated one — don't add tooling for it without a concrete recurring failure to justify it.

## Working Agreement

1. Start with `plan-with-me`, `debug-with-me`, or `learn-by-doing`.
2. Write the next small change yourself.
3. Ask `review-my-work` for findings, explanations, or a hint.
4. Use `verify-my-work` to pick and run the smallest useful proof.
5. Request a full solution only when you want a worked example, then read the explanation.
