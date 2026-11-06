# slopctl-starter

The starter template catalog for [slopctl](https://github.com/heikopanjas/slopctl), a Rust CLI
that manages coding agent instruction files (AGENTS.md, CLAUDE.md, and friends) across workspaces.

It is a sample catalog: slopctl uses it by default, and you can use it as is, fork it as the
starting point for your own templates, or keep it as the baseline and add your own (including
private) template systems on top of it as
[template layers](https://github.com/heikopanjas/slopctl#adding-template-systems-layers).
This repository was previously named `slopctl-templates`; GitHub redirects the old URLs.

This repo holds no code — only data. `slopctl templates --update` downloads it into a local
cache and installs from there; you never need to clone it directly unless you're editing the
catalog itself.

## Layout

```
templates/
  templates.yml          # catalog definition: which files go where, for which agent/language
  AGENTS.md               # the main AGENTS.md template, with fragment insertion points
  preamble.md              # session-start guard, merged first into AGENTS.md
  *-skills-hint.md, *-format-instructions.*, *-editor-config.ini
                           # per-language config file templates
  claude/ copilot/ cursor/ opencode/
                           # per-agent prompts and instruction stubs
  skills/                 # Agent Skills (agentskills.io) installed by the catalog

defaults/
  agent-defaults.yml      # per-agent filesystem conventions (prompt/skill dirs, markers)
  model-defaults.yml      # per-LLM-provider config (endpoints, API key env vars, default models)
```

`templates.yml`'s own `version:` field and header comments track the template format's history
and the insertion-point/placeholder reference — the directory name doesn't need to encode it.
`defaults/` is a sibling of `templates/`, not nested inside it, since it describes agent and
provider conventions rather than the template format itself.

`templates.yml` declares `id: slopctl-starter`. slopctl records that id as the origin of every file
it installs from this catalog. When you fork the catalog into your own, change the id; template
layers stacked on top of it need ids of their own.

## Making changes

Edit files under `templates/` (or `defaults/`), following the conventions already in
`templates.yml`. Then, from a slopctl workspace:

```
slopctl templates --update --from /path/to/slopctl-starter/templates
slopctl agents --update --from /path/to/slopctl-starter/defaults
slopctl models --update --from /path/to/slopctl-starter/defaults
slopctl templates --verify
```

to test against your local copy before publishing.

## License

MIT — see [LICENSE](LICENSE).
