# design-review-agent

This repo holds the **Design Review Agent** Copilot skill (`SKILL.md`).

The skill reviews a Figma file for *hygiene* issues — not design quality or
UX — across four areas:

1. File Hygiene (page/frame/layer organization)
2. Layout & Spacing (alignment, auto-layout, spacing scale)
3. Naming Conventions (descriptive, consistent naming)
4. Design System Compliance (library component/variable/style usage)

It can also implement the fixes it finds directly in Figma via Figma MCP
tools.

> **Status: v1.1.** `SKILL.md` lays out the overall workflow and guardrails.
> `SPECS.md` now has concrete, checkable specs for all four review areas,
> grounded in the real
> [`MediaMarktSaturn/foundation-design-tokens`](https://github.com/MediaMarktSaturn/foundation-design-tokens)
> design-token system (exact spacing/radii/typography scales, the semantic
> color-naming grammar, brand/variant/mode coverage, etc.). If a reviewed
> Figma file isn't built on that token system, the skill falls back to the
> generic guidance in `SKILL.md`.

## Repo layout

- `SKILL.md` — canonical copy of the skill at the repo root, for easy
  browsing/reading.
- `SPECS.md` — the concrete rulebook (exact scales, naming grammar, binding
  rules) the skill checks Figma files against. Mirrored alongside `SKILL.md`
  in both locations below.
- `.github/skills/design-review-agent/SKILL.md` and
  `.github/skills/design-review-agent/SPECS.md` — the same files, located
  where GitHub's Agent Skills tooling (and `gh skill`) expects to find them
  so they can be installed into other people's own environments.

Keep both copies of each file in sync when you edit the skill or specs.

## Installing this skill (for the 30+ reviewers)

This skill is distributed using GitHub's [Agent
Skills](https://docs.github.com/en/copilot/concepts/customizations/agent-skills)
framework, via the `gh skill` command (GitHub CLI 2.90.0+, currently in
public preview).

Each person who wants to use this skill on their own Figma files runs, once:

```sh
gh skill preview Younesmms/design-review-agent design-review-agent   # optional: inspect before installing
gh skill install Younesmms/design-review-agent design-review-agent --scope user
```

`--scope user` installs it to `~/.copilot/skills/`, so it's available in
every project/session they work in — not just this repo — and works with any
Figma file they point Copilot at (they'll need the Figma MCP server
connected).

To pick up future improvements to this skill:

```sh
gh skill update design-review-agent
# or, to update everything they've installed:
gh skill update --all
```

### Maintaining/publishing updates

When you change the skill:

1. Edit `.github/skills/design-review-agent/SKILL.md` and/or `SPECS.md` (and
   mirror the change into the root copies).
2. Validate before publishing: `gh skill publish --dry-run`.
3. Publish: `gh skill publish`.
4. Optionally tag a release (e.g. `v1.1.0`) so people who want stability can
   pin to it: `gh skill install Younesmms/design-review-agent design-review-agent --pin v1.1.0`.
