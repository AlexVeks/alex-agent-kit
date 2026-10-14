---
name: team-conventions
description: Reference for the full-stack team's coding conventions (naming, structure, testing, git, tooling) across Java/Spring, TypeScript/JS, React, PostgreSQL, and MongoDB. Load when reviewing, writing, or planning code in these stacks to check against the team standard.
---

# Team Coding Conventions (condensed reference)

Source: Full-Stack Team Coding Conventions, Consolidated Draft v0.1 (12 Oct 2026). Status: DRAFT FOR TEAM REVIEW — treat as provisional, not final. The repository's own code and config always take precedence over this draft if they conflict.

Precedence when rules conflict: Project exception > Technology/framework > Language > General.

Severity: **Required** (deviation must be explicit/justified) > **Recommended** (follow unless context is clearly better) > **Optional** (use if it helps).

## General (all stacks)
- Required: separate HTTP/business logic/persistence/presentation concerns.
- Required: names communicate intent; no ignored errors/exceptions; no hardcoded secrets; parameterized queries; validate untrusted input at boundaries; least-privilege DB/service accounts.
- Required: repo README with setup, architecture overview, API doc links.
- Recommended: KISS, YAGNI, DRY (only for genuine duplication), SOLID where it helps; comments explain *why*; ADRs for significant decisions; keep dependencies current.

## Project structure
- Required: feature-first folders (`order/`, `payment/`, `user/`), not layer-first (`controllers/`, `services/`). Cross-cutting code in `shared/`/`infrastructure/`.
- Recommended: start flat; add `dto/` or full `api/domain/service/repository/` layering only when a feature genuinely needs it.

## Java / Spring
- Required: UTF-8, one top-level class per file, no wildcard imports, 4-space indent (no tabs; **note: indentation width is an open decision in the source doc, confirm with team**), 100-char lines, one statement/declaration per line, `@Override` always used, named constants for magic numbers.
- Naming: packages lowercase, classes `UpperCamelCase`, methods/vars `lowerCamelCase`, constants `UPPER_SNAKE_CASE`, test classes `<Class>Test`.
- Spring: `@RestController`, plural resource paths, correct HTTP verbs/status codes, `@Valid` on request bodies, exceptions handled via `@ControllerAdvice`.

## TypeScript / JavaScript / React
- Required: 2-space indent, semicolons, single quotes outside JSX, 100-char lines, trailing commas in multiline; `const`/`let` only (no `var`); strict TS, no `any`; don't mix callbacks and Promises.
- Naming: files kebab-case, classes/components/interfaces PascalCase (no `I` prefix), functions/vars camelCase, constants UPPER_SNAKE_CASE.
- React: functional components + hooks, typed props, hook names prefixed `use`, correct dependency arrays with cleanup, state kept local, avoid storing derived state.

## Databases
- PostgreSQL: snake_case plural tables, snake_case columns, migrations only (no manual prod schema edits), `created_at`/`updated_at` on tables, index query-critical columns, uppercase SQL keywords, parameterized queries.
- MongoDB: camelCase plural collections/fields, boolean fields prefixed `is`/`has`, schema validation per collection, `createdAt`/`updatedAt`, indexes for operational queries, avoid deep nesting.

## Testing
- Required: independent and deterministic tests, one behavior per test, no network/DB calls in unit tests, integration tests never touch production databases.
- Recommended: Arrange-Act-Assert structure; cover success/failure/edge/error paths over chasing a coverage number.
- Naming: Java `method_scenario_expectedResult` or `should_expectedBehavior_when_condition`; JS/TS `describe`/`it`; React `it("renders/displays/handles ...")`.

## Git & review
- Branch naming: `feature/<ticket>-<desc>`, `bugfix/...`, `hotfix/...`, `release/<version>`.
- Commits: Conventional Commits — `<type>(<scope>): <description>` (`feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`).
- PRs: at least one approval required, review correctness/readability/naming/tests/security, descriptive title linked to a ticket, resolve discussions before merge. Recommended: keep PRs under ~400 changed lines.

## Tooling
- Required: shared formatter/linter config committed to the repo, shared `.editorconfig`, lint failures block commits/builds, static-analysis quality gate passes before merge to main, Critical/Blocker findings fixed before merge.
- Authoritative tools per stack: Checkstyle/SonarLint + google-java-format or shared IntelliJ formatter (Java); ESLint + Prettier (TS/JS/React); Stylelint + Prettier (CSS/SCSS); SQLFluff (SQL, optional).
- Recommended: format-on-save, pre-commit lint/format/affected-tests, ~80% coverage / <3% duplication / A ratings on new code as a target, not a hard gate.

## Explicitly NOT adopted here (open in the source doc — do not treat as settled)
- Java indentation width (4 vs 2 spaces) and formatter choice (google-java-format vs IntelliJ).
- Whether `React.FC` is preferred, allowed, or discouraged.
- Auth storage convention; Express/NestJS-specific rules.
