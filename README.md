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

> **Status: v1.** The current `SKILL.md` lays out the overall workflow and
> guardrails. The detailed specs/checklists for each of the four review
> areas (File Hygiene, Layout & Spacing, Naming Conventions, Design System
> Compliance) will be fleshed out further in future iterations.

## Repo layout

- `SKILL.md` — canonical copy of the skill at the repo root, for easy
  browsing/reading.
- `.github/skills/design-review-agent/SKILL.md` — the same skill, located
  where GitHub's Agent Skills tooling (and `gh skill`) expects to find it so
  it can be installed into other people's own environments.

Keep both copies in sync when you edit the skill.

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

1. Edit `.github/skills/design-review-agent/SKILL.md` (and mirror the change
   into the root `SKILL.md`).
2. Validate before publishing: `gh skill publish --dry-run`.
3. Publish: `gh skill publish`.
4. Optionally tag a release (e.g. `v1.1.0`) so people who want stability can
   pin to it: `gh skill install Younesmms/design-review-agent design-review-agent --pin v1.1.0`.
