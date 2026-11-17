# OpenCode Learner-First Configuration

This setup makes the AI act as a tutor, planner, debugger, reviewer, and testing coach. **You write the production code; the AI guides you.**

## The Big Picture

```text
You ──► mentor (default agent) ──► loads a skill that matches your task
              │                         │
              │                         └─ skills give step-by-step procedures
              └─► scout (subagent) for broad "where is X?" searches (read-only)

AGENTS.md ........ global rules every agent obeys
opencode.jsonc ... default agent + permissions every agent obeys
```

Three layers, each with one job:

| Layer | File(s) | Job |
|---|---|---|
| Rules | `AGENTS.md` | What no agent may do (e.g. write your code unasked, run git for config repos). |
| Persona | `agents/mentor.md`, `agents/scout.md` | Who answers you and how. |
| Procedures | `skills/*/SKILL.md` | Step-by-step recipes, loaded only when the task matches. |

## Layout

```text
~/.config/opencode/
  AGENTS.md                  Global rules for every agent
  opencode.jsonc             Default agent (mentor) and shared permissions
  agents/
    mentor.md                Primary agent. Teaches, reviews, maintains the vault
    scout.md                 Read-only search subagent (Haiku). Mentor verifies its reports
  skills/
    -- Coding workflow --
    plan-with-me/            Turn an idea into a small plan
    learn-by-doing/          Build something one small step at a time
    debug-with-me/           Evidence-led debugging
    review-my-work/          Review your code: findings + one next action
    verify-my-work/          Choose and run checks that prove behavior
    team-conventions/        Reference standard (Java/Spring, TS/JS, React, Postgres, MongoDB)
    -- Learning and notes --
    teach-concept/           "Teach me X" so it clicks
    save-to-vault/           Save approved concept notes to Obsidian
    organize-vault/          Empty the TO_ORGANIZE inbox, fix broken links
    draw-diagram/            Propose Excalidraw drawings for concepts
    -- Team tasks --
    write-pr/                PR title and description with a Status checklist
    xray-security-scan/      JFrog Xray scan, Critical/High findings only
```

Skills are discovered automatically; there is nothing to activate.

## Typical Flow

1. **Describe your task.** The mentor picks the matching skill. To force one, name it: "use `debug-with-me`".
2. **Not sure where to start?** `plan-with-me`. **Know what to build?** `learn-by-doing`. **Something broke?** `debug-with-me`.
3. **You write the next small change yourself.**
4. **`review-my-work`** reads your code and gives findings plus one *Next action*.
5. **`verify-my-work`** runs checks and tells you what a pass does and does not prove.
6. **`write-pr`** drafts the PR when you are done.

Review vs. verify: review *reads* code, verify *runs* checks.

For concepts: `teach-concept` explains it, then the mentor offers `save-to-vault` to keep a note (and `draw-diagram` if a picture helps).

Ask for a full solution only when you want a worked example; the mentor will explain the key decisions and give a follow-up exercise.

## Conventions

Whenever the stack is Java/Spring, TS/JS, React, PostgreSQL, or MongoDB, the mentor loads `team-conventions` while planning, guiding, reviewing, or verifying. This rule lives once, in `agents/mentor.md`; skills just say "check conventions". The repository's own code and config win over `team-conventions`, and conflicts are reported, not silently resolved.

## Permissions

Prose alone is not a guarantee, so permissions enforce the behavior. Shared rules live in `opencode.jsonc` (top-level `permissions`); agent files list only what differs. The last matching rule wins.

| Permission | Value | Effect |
|---|---|---|
| `read`, `glob`, `grep` | Built-in defaults | Inspection allowed; `.env` and `.env.*` ask, except `.env.example`. |
| `edit` | `ask` | Every edit/write/patch needs approval. |
| `subagent` | `deny` | No child agents, except the mentor may call `scout`. |
| `shell` | `ask` | Safe read-only commands allowed (`git status/diff/log/branch/show/grep/ls-files`, `ls`, `head`, `Select-Object`, `Select-String`); `rm` denied. |

Per agent:

- **mentor**: `external_directory` asks, except the vault (allowed; edits still ask). `git push` denied. `Remove-Item *TO_ORGANIZE*` allowed for inbox cleanup.
- **scout**: edit, shell, and subagents denied. External directories are denied except the Obsidian vault, which it can read. It can never write anywhere.

These are native OpenCode V2 permission rules. **Allow always** saves a project-scoped approval; use **Allow once** to review each operation. The built-in `build` agent is hidden; ask explicitly for work to be done for you if you want that (tool permissions still apply).

## Knowledge Vault

Location: `C:\Users\alexvek\Documents\Obsidian\Office`.

1. After an important explanation, the mentor offers `save-to-vault`.
2. It shows the exact change and waits for your approval. Nothing is written without it.
3. New notes go straight into `TO_ORGANIZE`, unlinked. Existing notes are updated in place.
4. Only when you ask to organize the vault does `organize-vault` file notes into topics, add links, update `_Index.md`, fix broken links, and report drawings no note links to.

