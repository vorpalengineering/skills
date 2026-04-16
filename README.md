# Vorpal Engineering Skills Marketplace

> Blockchain Intelligence & Security Skills — built by [Vorpal Engineering](https://vorpalengineering.com).

A Claude Code plugin marketplace for Vorpal Engineering's Blockchain Intelligence & Security skills.

## Install

In any Claude Code session:

```
/plugin marketplace add vorpalengineering/skills
/plugin install vulnerability-lookup@vorpalengineering
```

That's it. Skills auto-trigger from context, or invoke explicitly:

```
/vulnerability-lookup
```

## Update

```
/plugin update vulnerability-lookup@vorpalengineering
```

## Plugins

| Plugin | Description |
|---|---|
| [vulnerability-lookup](plugins/vulnerability-lookup/skills/vulnerability-lookup/) | Surface known security patterns, vulnerabilities, exploits, and mitigations from the Vorpal knowledge graph |

## Repo Conventions

- Each skill ships as its own plugin under `plugins/<plugin-name>/`.
- Plugins follow the canonical structure: `.claude-plugin/plugin.json` at the plugin root, plus `skills/<skill-name>/SKILL.md` for the skill itself.
- Tool references and supporting docs live inside each skill's `references/` folder, curated for that skill's specific needs.
- The marketplace catalog lives at `.claude-plugin/marketplace.json` at the repo root.

## Contact

Questions or feedback? Reach out to `support@vorpalengineering.com`
