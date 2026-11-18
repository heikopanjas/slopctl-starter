# Project Instructions for AI Coding Agents

**Last updated:** 2026-11-18

<!-- {preamble} -->

# ⚠️ Before You Start

Run `/init-session` at the beginning of each new session, OR read this entire file before proceeding.

**DO NOT** make code changes or commits until you have done one of the above.

<!-- {mission} -->

## Mission Statement

slopctl-starter is the default template catalog for [slopctl](https://github.com/heikopanjas/slopctl), a Rust CLI that manages coding agent instruction files (AGENTS.md, CLAUDE.md, and friends) across workspaces. It holds data only, no code. `slopctl templates --update` downloads it into a local cache and installs from there.

Key features:

- `templates/templates.yml` defines which files go where, per agent, language, and integration, with fragment insertion points in the main `AGENTS.md` template
- per-language conventions, build commands, and formatter/editor configs (C, C++, CMake, Rust, Shell, Swift, SwiftUI)
- per-agent prompts and instruction stubs (Claude Code, Copilot, Cursor, OpenCode, Pi, Kiro, and others)
- Agent Skills (agentskills.io) under `templates/skills/`
- `defaults/` with per-agent filesystem conventions and per-LLM-provider config
- usable as is, as a fork, or as the base for template layers; `templates.yml` declares `id: slopctl-starter`

## Technology Stack

- **Language:** Markdown and YAML, plus TOML, JSON, and INI for formatter and editor configs
- **Framework:** slopctl template catalog format, version 5 (`version:` in `templates/templates.yml`)
- **Version Control:** Git, working branch `develop`, release branch `main`
- **Package Manager:** none; consumed by slopctl via `slopctl templates --update`
- **CI/CD:** GitHub Actions (`.github/workflows/release.yml`) zips `templates/`, `defaults/`, and `LICENSE` and publishes a `catalog-<timestamp>-<sha>` release on every push to `main`
- **License:** MIT

## Session Protocol

When starting a new session, read this entire file and confirm you have
understood the project instructions before proceeding. Summarize the project
purpose and key conventions briefly. Do not make changes until you have
confirmed your understanding.

<!-- {principles} -->

## Primary Instructions

- Avoid making assumptions. If you need additional context to accurately answer the user, ask the user for the missing information. Be specific about which context you need.
- Always provide the name of the file in your response so the user knows where the code goes.
- Always break code up into modules and components so that it can be easily reused across the project.
- All code you write MUST be fully optimized. ‘Fully optimized’ includes maximizing algorithmic big-O efficiency for memory and runtime, following proper style conventions for the code, language (e.g. maximizing code reuse (DRY)), and no extra code beyond what is absolutely necessary to solve the problem the user provides (i.e. no technical debt). If the code is not fully optimized, you will be fined $100.

### Working Together

This file (`AGENTS.md`) is the primary instructions file for AI coding assistants working on this project. Agent-specific instruction files (such as `CLAUDE.md`, `.github/copilot-instructions.md`, `.cursorrules`) reference this document, maintaining a single source of truth.

When initializing a session or analyzing the workspace, refer to instruction files in this order:

1. `AGENTS.md` (this file - primary instructions and single source of truth)
2. Agent-specific reference file (if present - points back to AGENTS.md)

### Update Protocol (CRITICAL)

**PROACTIVELY update this file (`AGENTS.md`) as we work together.** Whenever you make a decision, choose a technology, establish a convention, or define a standard, you MUST update AGENTS.md immediately in the same response.

**Update ONLY this file (`AGENTS.md`)** when coding standards, conventions, or project decisions evolve. Do not modify agent-specific reference files unless the reference mechanism itself needs changes.

**When to update** (do this automatically, without being asked):

- Technology choices (build tools, languages, frameworks)
- Directory structure decisions
- Coding conventions and style guidelines
- Architecture decisions
- Naming conventions
- Build/test/deployment procedures

**How to update AGENTS.md:**

- Maintain the "Last updated" timestamp at the top
- Add content to the relevant section (Project Overview, Coding Standards, etc.)
- Log every change in the "Recent Updates & Decisions" log in `UPDATES.md` with:
  - Date (with time if multiple updates per day)
  - Brief description
  - Reasoning for the change
- New log entries go directly below the changelog marker in `UPDATES.md`, newest first; load the `recent-updates` skill for the full rules
- Preserve the AGENTS.md structure: title header → timestamp → main instructions

## Best Practices

### When Updating This Repository

1. **Maintain Consistency**: Keep code style consistent across the codebase
2. **Test First**: Write tests before implementing features when applicable
3. **Document Changes**: Update documentation when changing functionality
4. **Code Review**: solo self-review. Review the diff locally on `develop`, verify it with slopctl (see Testing), then fast-forward `main`. Every push to `main` publishes a release, so only push reviewed and verified changes
5. **Date Changes**: Update the "Last updated" timestamp in this file when making changes
6. **Log Updates**: Add entries to the "Recent Updates & Decisions" log in `UPDATES.md`

### Development Guidelines

- Edit only `templates/` and `defaults/` for catalog changes. The root `AGENTS.md`, `CLAUDE.md`, `UPDATES.md`, `.claude/`, and `.slopctl/` are slopctl's own install output in this workspace, not catalog sources
- Register every new template file or skill in `templates/templates.yml`, following the existing conventions for `agents`, `shared`, `languages`, `integration`, `principles`, `mission`, and `skills`
- Keep paired content in sync: a fragment in `templates/` and its skill in `templates/skills/` (for example `rust-build-commands.md` and `skills/rust-build-commands/SKILL.md`), and coding conventions with the matching `*-format-instructions.*` file (for example max line length)
- Skills follow the agentskills.io layout: one directory per skill containing `SKILL.md`
- Preserve the insertion-point markers (`<!-- {preamble} -->`, `<!-- {mission} -->`, `<!-- {principles} -->`, `<!-- {languages} -->`, `<!-- {integration} -->`) and the `<!-- {changelog} -->` marker in the templates
- Update `README.md` when the layout or the change workflow changes

### Security & Safety

- Never include API keys, tokens, or credentials in code
- Always require explicit human confirmation before commits
- Maintain conventional commit message standards
- Keep change history transparent through commit messages
- Remote skill sources in `templates.yml` (GitHub URLs) are fetched onto users' machines; only reference trusted repositories
- Templates ship to every slopctl user; never put personal paths, private hostnames, or secrets in them

### Testing

There is no code and no test suite. Verify catalog changes against a local copy from a slopctl workspace before pushing to `main`:

```
slopctl templates --update --from /path/to/slopctl-starter/templates
slopctl agents --update --from /path/to/slopctl-starter/defaults
slopctl models --update --from /path/to/slopctl-starter/defaults
slopctl templates --verify
```

- Unit tests: none
- Integration tests: install into a scratch workspace and inspect the generated files
- Test coverage requirements: none
- Testing framework: `slopctl templates --verify`

### Documentation

- Code comments: YAML comments in `templates.yml` document the format history, placeholders, and insertion points; extend them when the format changes
- API documentation: none; the catalog format is documented in the `templates.yml` header
- README updates: required when the layout or the change workflow changes
- Changelog: maintained in `UPDATES.md` (see the `recent-updates` skill)

<!-- {languages} -->



<!-- {integration} -->

## Commit Protocol

- **NEVER commit automatically** — always wait for explicit user confirmation
- Stage changes, write a conventional commits message (max 50-char subject, 72-char body lines), then commit
- Load the `git-workflow` skill for the full message format, character limits, and examples before committing
- Never add co-authorship trailers or any attribution naming an AI coding agent to commit messages

## Recent Updates & Decisions

Project decisions and notable changes are logged in `UPDATES.md`, below its changelog marker. The log is append-only: add new entries at the top, never edit or delete existing ones, and keep the "Last updated" timestamp in `AGENTS.md` in sync. Load the `recent-updates` skill for the entry format and full rules.

## Semantic Versioning

This repository has no project version. Releases are identified by their `catalog-<timestamp>-<sha>` tag. The only version is the template format version (`version:` in `templates/templates.yml`). Bump it only when the format changes in a way slopctl must understand, document the addition in the `templates.yml` header, and include the bump in the same commit. Content-only changes get no bump.
