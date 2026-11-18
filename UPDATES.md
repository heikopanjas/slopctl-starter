# Recent Updates & Decisions

This file is the append-only log of project decisions and notable changes, maintained by coding agents following the `recent-updates` skill. Everything below the marker line is user-owned history: slopctl never overwrites it during init or merge.

<!-- {changelog} -->

### 2026-11-18 (format v5, pi project settings)

- added templates/pi/settings.json, installed to .pi/settings.json for the pi agent
- sets the pi settings json schema and adds grep, find, and ls to the default tools
- rationale: gives pi workspaces editor validation and a broader read-only tool set; additive tool entries keep the user's own tool selection intact, and no personal settings are shipped
- no format version bump: content-only change

### 2026-11-18 (format v5, fill in agents.md placeholders)

- filled in mission, technology stack, code review, development guidelines, security, testing, and documentation sections with repo-specific content
- code review is solo self-review on develop, verified with slopctl before fast-forwarding main
- versioning limited to the templates.yml format version, bumped only on format changes
- rationale: agents.md was still the unfilled starter template; every push to main publishes a release and the repo has no project version, so the generic auto-bump rule did not apply

### 2025-10-05 (v0.1.0, initial setup)

- initial AGENTS.md setup
- established core coding standards and conventions
- defined repository structure and governance principles
