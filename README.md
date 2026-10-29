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
