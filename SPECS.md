# Design Review Specs — MediaMarktSaturn Foundation Design Tokens

This document is the concrete, checkable rulebook the `design-review-agent`
skill uses when reviewing a Figma file for hygiene. It replaces the generic
placeholders from v1 of `SKILL.md` with exact values and naming rules pulled
directly from the canonical token source:

> **Source of truth:** [`MediaMarktSaturn/foundation-design-tokens`](https://github.com/MediaMarktSaturn/foundation-design-tokens), `release` branch.
> These tokens are synced into Figma via Tokens Studio, so any Figma file
> built on this design system should bind to variables/styles with these
> exact names and values — not hard-coded ones.

If the token repo changes, re-run the extraction (see
["Keeping this in sync"](#keeping-this-in-sync)) and update this file. If a
reviewed Figma file is **not** on this design system, fall back to the
generic guidance in `SKILL.md` instead of these specific values.

---

## 1. File Hygiene

Generic rules (design-system agnostic), plus token-system-specific structure:

- **Pages/sections should mirror the system's own layers**, where relevant:
  Core primitives → Semantic roles → Brand/Theme. Don't mix raw-value
  exploration frames with production, brand-mapped frames on the same page
  without a clear section boundary.
- **Brand & mode coverage should be explicit.** This system ships two brands
  (`mediamarkt`, `saturn`) across theme variants (`base`, `inverse`,
  `prominent`, `sustainable`, plus deprecated `error`/`warning`/`success`/`info`)
  and two color modes (`light`, `dark`). If a flow is designed for multiple
  brands/modes, each combination needs its own clearly named frame/page —
  don't leave only one brand/mode designed and call the set "done" without
  labeling it as partial.
- No stray loose layers on the canvas outside a frame.
- No duplicate/unused frames left over from exploration (flag, don't assume
  intent — ask before deleting).
- Deeply nested groups that should be frames with auto layout.
- Consistent page structure across similar/parallel flows (e.g. if
  MediaMarkt and Saturn versions of a flow exist, they should have matching
  page/frame structure, differing only in content).

## 2. Layout & Spacing

### Base grid

Everything derives from a **4px base unit** (`1rem = 16px`, step = `0.25rem`
= `4px`). Any spacing, gap, or padding that isn't a multiple of 4px (or one
of the named exceptions below) is a hygiene violation.

### Spacing / dimension scale (`spacing.*`, `dimensions.*`)

| Token key | rem | px |
|---|---|---|
| `0` | 0 | 0 |
| `0_25` | 0.25rem | 4px |
| `0_5` | 0.5rem | 8px |
| `0_75` | 0.75rem | 12px |
| `1_0` | 1rem | 16px |
| `1_25` | 1.25rem | 20px |
| `1_5` | 1.5rem | 24px |
| `1_75` | 1.75rem | 28px |
| `2_0` | 2rem | 32px |
| `2_25`…`6_0` | …(same 0.25 step)… | up to 96px |

Plus special value `auto`.

### Semantic sizing scale (t-shirt names, `sizes.*`)

| Name | Value |
|---|---|
| `4xs` | 1px |
| `3xs` | 2px |
| `2xs` | 4px |
| `xs` | 8px |
| `sm` | 12px |
| `md` | 16px (base) |
| `lg` | 24px |
| `xl` | 28px |
| `2xl`…`10xl` | 32px…64px (0.25rem steps) |
| `full` | 100% |
| `1/2`, `1/3`, `2/3`, `1/4`, `3/4` | fractional % |
| `prose.min` / `prose.max` | 45ch / 80ch (optimal line length) |
| `container.7xs`…`container.7xl` | 128px…1280px named container widths |
| `container.max` | = `xxl` breakpoint (1472px) |

### Border radius scale (`radii.*`)

| Name | Value |
|---|---|
| `none` | 0 |
| `xs` | 0.125rem (2px) |
| `sm` | 0.25rem (4px) |
| `md` | 0.375rem (6px) |
| `lg` | 0.5rem (8px) |
| `xl` | 0.625rem (10px) |
| `2xl` | 0.75rem (12px) |
| `3xl` | 1rem (16px) |
| `4xl` | 1.25rem (20px) |
| `5xl` | 1.5rem (24px) |
| `full` | 999rem (pill/fully rounded) |

### Border width scale (`borderWidths.*`)

| Name | Value |
|---|---|
| `none` | 0px |
| `xs` | 1px |
| `sm` | 2px |
| `md` | 4px |
| `lg` | 8px |

### Breakpoints

| Name | Min width |
|---|---|
| `xxs` | 0 |
| `xs` | 368px |
| `sm` | 512px |
| `md` | 752px |
| `lg` | 1008px |
| `xl` | 1232px |
| `xxl` | 1472px |

### Shadows / elevation (`shadow.*`)

Named elevation scale: `none`, `xs`, `sm`, `md`, `lg` (increasing
offset/blur/spread), each resolving to a semantic shadow color
(`shadow.subtle` / `shadow.DEFAULT` / `shadow.emphasized`). A parallel
**glow** variant exists for dark-mode/secondary-brand-accent emphasis:
`shadow.glow.{xs,sm,md,lg}` / `shadow.glow.{subtle,DEFAULT,emphasized}`.
Flag any custom drop-shadow that doesn't match one of these five elevations.

### Aspect ratios (`aspectRatios.*`)

| Name | Ratio |
|---|---|
| `square` | 1/1 |
| `product` | 3/2 |
| `video` | 16/9 |

### Violations to flag

- Gaps/padding not on the 4px grid or not using a named spacing/size token.
- Corner radii not matching the `radii.*` scale.
- Borders not matching the `borderWidths.*` scale.
- Frames not using auto layout where content is linear/repeating.
- Hug/fill sizing misused (e.g. fixed width where content should hug or
  fill the breakpoint's container width).
- Custom shadows that don't map to the 5-step elevation (+ glow) scale.

## 3. Naming Conventions

### Core token naming (raw scales — should rarely appear directly bound in
product frames; flag if they are, see §4)

- Colors: `colors.<palette>.<mode>.<step>` — palette ∈ `grey`, `slate`,
  `red`, `blue`, `green`, `emerald`, `orange`, `yellow`, `purple`; mode ∈
  `light`/`dark`; step ∈ `25,50,100,200,300,400,500,600,700,800,900,950`.
  Special: `colors.inherit`, `colors.current`, `colors.transparent`.
- Scales (spacing/sizes/radii/dimensions/opacity/zIndex): numeric key with
  underscore for decimals, e.g. `1_25` = `1.25`. Never a bare "Rectangle 45"
  / "Frame 123" style default name.

### Semantic token naming (the naming pattern component/layer names should
follow when they represent a design-system role)

Full segment order (all but visual category are optional):

```
{component?}.{higher-visual-category?}.{visual-category}.{role?}.{feedback?}.{hierarchy?}.{state?}.{attribute?}
```

- **Visual category** (required): `text`, `border`, `background`, `shadow`,
  `surface`.
- **Higher visual category** (optional): `control` (interactive elements).
- **Role**: `primary`, `secondary`, `tertiary`, `quaternary`, `feedback`.
- **Feedback category**: `success`, `warning`, `error`, `info`.
- **Hierarchy**: `DEFAULT`, `emphasized`, `subtle`, `inverted`, `accent`,
  `on-accent`.
- **State**: `DEFAULT`, `hover`, `active`, `focus`, `selected`, `disabled`,
  `loading`.
- **Attribute**: `required`, `optional`, `readonly`, `valid`, `invalid`,
  `visited`.

Valid examples: `text.DEFAULT`, `text.subtle`, `text.primary.success.DEFAULT`,
`background.overlay`, `background.accent.subtle`,
`control.background.accent.hover`, `control.border.feedback.error`,
`shadow.glow.sm`. Minimum valid name: `text`. These names are **stable
contracts** — don't invent new segment values outside the vocabulary above
without flagging it as a proposed system extension (don't just rename
freely).

### File/page/screen/component/a11y/git naming (MMS organizational standard)

Files, pages, screens, components, properties, accessibility annotations,
local variables, and branch/commit workflows must follow the dedicated MMS
ruleset in **[`NAMING-CONVENTIONS.md`](./NAMING-CONVENTIONS.md)** (next to
this file) — check it alongside the token-naming grammar above, not instead
of it. In short, that file additionally covers:

- Figma/FigJam file naming (`{team}-{funnel}-{Flow}` /
  `{team}-{funnel}-{category}-{TicketNumber}`).
- The required page spine (`Cover` → `UserFlow[-{salesline}]` → `AB-Test` →
  `Playground` → `ComponentRequests` → `Local Components` →
  `Shared Components` → `Archive`).
- A/B test page status-emoji pattern.
- Screen/artboard naming
  (`{pagename}-{state}-{stateDescription}-{screenSize}-{salesline}-{draft}`).
- BEM component naming (`{Block}{Element}-{Modifier}`, no nested elements).
- Component property/variant casing (`camelCase`; booleans prefixed
  `is`/`has`/`show`; named slots/swaps).
- Accessibility annotation conventions (focus order, screen-reader/alt text,
  heading levels, touch-target/contrast badges).
- Figma local variable kebab-case naming (separate from, but expected to
  correspond to, the semantic token grammar above).
- Figma branch naming and conventional-commit message format.
- The canonical MMS library registry components/styles should be pulled
  from (`LibraryFoundation`, `LibraryWebComponents`,
  `LibraryAccessibilityAnnotationKit`, `LibraryAppComponents`,
  `LibraryIcons`, `LibraryIllustrations`, `LibraryStyleguide`).

Also still applies as general Figma hygiene, regardless of the above:

- No default/placeholder names: "Frame 123", "Rectangle 45", "Group 7",
  "Ellipse 2", copy-pasted "Frame 123 copy".
- Names should describe content/role, not just type (`Promobar CTA`, not
  `Button`).
- Consistent casing/delimiters within a file for the same kind of layer,
  per whichever convention above applies to that layer type.

## 4. Design System Compliance

Figma nodes should bind to variables/styles from the Tokens Studio sync of
this repo — never hard-code a raw value that has a matching token.

### Colors

Must bind to a **semantic** color token (§3 naming), never directly to a
core `colors.<palette>.<mode>.<step>` token and never a hard-coded hex. Key
groups to check, by visual category:

| Category | Tokens to expect |
|---|---|
| `text.*` | `DEFAULT`, `emphasized`, `subtle`, `inverted`, `accent`, `on-accent` |
| `border.*` | `DEFAULT`, `emphasized`, `subtle`, `accent`, `focus` |
| `background.*` | `DEFAULT`, `inverted`, `subtle`, `overlay`, `accent.DEFAULT`, `accent.subtle` |
| `surface.*` | `DEFAULT`, `secondary`, `tertiary`, `feedback.{state}` |
| `control.{background,text,border}.*` | per-state: `DEFAULT`, `hover`, `active`, `focus`, `selected`, `disabled` |
| `*.feedback.{success,warning,error,info}` | variant-independent — does **not** inherit the active brand/theme palette; flag if a feedback color looks tinted by the surrounding brand accent |

### Typography

Must bind to `fonts.*` (`body` = Noto Sans Display, `heading` = Noto Sans
Display, `brand` = MM Headline Pro WebTT — used for prices/labels in
branded contexts), `fontSizes.*`, `fontWeights.*`
(`thin`…`black`, i.e. 100–900, plus `normal`=400, `medium`=500,
`semibold`=600, `bold`=700), `lineHeights.*`, and `letterSpacings.*`
(`tighter`, `tight`, `normal`, `wide`, `wider`, `widest`) — or, better, a
predefined text style: `body.{2xs,xs,sm,md,lg}`, `body.brand.{…}`, or
`heading.{2xs,xs,sm,md,lg}`. Flag raw font sizes/weights not from these
scales (e.g. `15px`, `font-weight: 550`).

Semantic font size scale for reference: `2xs`=10px, `xs`=12px, `sm`=14px,
`md`=16px, `lg`=18px, `xl`=20px, `2xl`=24px, `3xl`=28px, `4xl`=32px,
`5xl`=36px, `6xl`=40px, `7xl`=44px, `8xl`=48px, `9xl`=56px.

### Spacing, sizing, radii, borders, shadows

Must bind to the scales in §2 — flag hard-coded px/rem values that match
(or are close to) a scale step but aren't bound to the token.

### Components, and local components specifically

- Flag detached/local instances that should be swapped for the matching
  published library component.
- Flag off-library one-off components that duplicate an existing library
  component's purpose.
- **Local components vs. the web components Storybook.** The canonical
  implementation reference for this org's production web components is the
  internal Storybook (`webmobile-storybook`, SSO-gated — `/docs/welcome--docs`
  is the entry point). It documents each component's real controls/args
  (e.g. a `Button` component's `buttonSpacing` control exposed as t-shirt
  values matching the sizing scale in §2: `xs`, `sm`, `md`, `lg`, …). Treat
  it as the source of truth for *what props/variants a correctly-built
  component instance should expose*, the same way `SPECS.md` §1–§3 are the
  source of truth for tokens and `NAMING-CONVENTIONS.md` is for
  organizational naming.
  - If a Figma file has a **local (non-instance) component** that visually
    or functionally matches something documented in the Storybook (button,
    input, card, badge, etc.), flag it: it should be swapped for the
    library component instance instead of being hand-built/detached.
  - If a local component's **properties/variants** don't match the
    Storybook component's documented controls (different value sets,
    missing a control the Storybook exposes, or a property named
    differently than the Storybook's prop), flag the mismatch — the Figma
    component property panel should mirror the real component API. Property
    naming itself still follows `NAMING-CONVENTIONS.md` §3.6
    (`camelCase`, boolean `is`/`has`/`show` prefixes).
  - The agent cannot browse the Storybook directly when it's behind
    interactive SSO. When live access isn't available, ask the user to
    paste the specific component's docs/args (or an exported snapshot)
    before asserting a prop-level mismatch — don't guess at a component's
    real controls.
  - If no matching Storybook/library component exists for a local
    component, don't force a swap — flag it as a candidate for a new
    library component request instead (see `NAMING-CONVENTIONS.md` §3.2,
    the `ComponentRequests` page).

### Brand/variant/theme usage

- A frame representing `mediamarkt` content shouldn't reference `saturn`
  brand tokens (or vice versa) — check the bound theme/variant matches the
  frame's declared brand.
- Legacy variants (`error`, `warning`, `success`, `info` as whole theme
  variants) are **deprecated** — new designs should use the embedded
  `*.feedback.{state}` tokens instead of switching the entire frame to a
  legacy severity theme. Flag use of a legacy severity theme variant.

### Don't invent

If no matching token/component exists for something that looks
off-library or hard-coded, flag it — don't force a swap to the
closest-but-wrong token.

---

## Keeping this in sync

This file was generated by reading, on the `release` branch of
`MediaMarktSaturn/foundation-design-tokens`:

- `design-tokens/tokens/*.ts` (core scales)
- `design-tokens/semantic-tokens/*.ts` (semantic t-shirt names/roles)
- `design-tokens/themes/*` (brands, variants, modes)
- `docs/token-naming.mdx` (the semantic naming pattern in §3)

If that repo's `release` branch changes (new scale steps, new semantic
roles, new brands), re-fetch those files and update the tables above —
don't let this drift from the real token source.

§3's organizational naming rules (files/pages/screens/components/a11y/git)
come from a separate source — see `NAMING-CONVENTIONS.md`'s own "Keeping
this in sync" note.

§4's "local components vs. Storybook" check references
`webmobile-storybook-dev.dev.webmobile-mreg.d-p.io` (internal, SSO-gated)
as the canonical implementation reference for web component
props/controls. The agent could not fetch its contents directly (it
redirects to Google sign-in) — the component/prop details above (e.g. the
`buttonSpacing` control) are inferred only from the URL's query args, not
verified against the full component catalog. Whoever next has SSO access
should browse the Storybook, extract the real per-component control list,
and replace this placeholder note with a concrete component-by-component
table (component name → documented props/controls → expected Figma
property-panel shape).
