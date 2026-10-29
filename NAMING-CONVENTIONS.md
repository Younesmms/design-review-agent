# MMS Naming Conventions & Design Architecture

> **Document type:** AI agent system knowledge base & audit ruleset.
> **Use case:** Automated design review/linting of Figma files, page
> structures, screens, components, properties, accessibility annotations,
> variables/tokens, and Git/branch workflows.
> **Organization:** MMS Technology GmbH.

This file is a **second, independent naming-conventions ruleset** that the
`design-review-agent` skill must check **alongside** the token-naming
grammar in `SPECS.md` §3 (core/semantic color and scale-token naming).
Use both together for the "Naming Conventions" review category:

- `SPECS.md` §3 → does a *token/variable* follow the design-token naming
  grammar (`text.subtle`, `colors.grey.light.500`, etc.)?
- This file → does a *file, page, screen, component, property, a11y
  annotation, variable, branch, or commit* follow MMS's organizational
  naming standards?

A single file can violate either, both, or neither — check both
independently and report violations under their respective rule source.

---

## 1. Primary authority sources

1. **MMS UX Get Started: Naming Convention** (internal Confluence) — core
   governance page for workspace casing, file taxonomy, page architecture,
   frame/screen standards, A/B testing tags, and library references.
2. **[BEM Methodology](https://bem.info/en/methodology/naming-convention/)**
   — Block-Element-Modifier pattern for naming UI components, sub-elements,
   state variations, and avoiding deep nesting.
3. **MMS LibraryAccessibilityAnnotationKit** (Figma, node `2:670`) —
   official accessibility annotation library: focus order, screen-reader
   labels, semantic heading levels, touch-target boundaries, WCAG tags.
4. **MMS LibraryWebComponents Property & Variant Specification** (Figma,
   node `22:6205`) — production component library: property keys, variant
   values, boolean flags, slots, interactive states.

## 2. Core casing & syntax matrix

| Scope / target entity | Casing | Syntax | Examples |
|---|---|---|---|
| General elements / defaults | `PascalCase` | UpperCamelCase | `DateFormat`, `DatabaseConnection`, `EventHandler` |
| Component properties & variants | `camelCase` | lowerCamelCase | `variant`, `colorVariant`, `alignment`, `hasEEL`, `isSelected`, `isDisabled` |
| Variables, tokens & themes | `kebab-case` | hyphen-delimited | `background`, `background-hover`, `text-primary`, `surface-subtle` |
| Screen & file segments | `Hyphenated` | segment-delimited | `Checkout-Step1-Pickup-xl`, `Web-PDP-Prototype-EXP-123` |
| Git / branch types | `lower-case` | `type(scope): message` | `feat(wtf-234): message`, `fix(checkout): message` |

## 3. Review checklist

### 3.1 File & board naming (Figma & FigJam)

Files must reflect team ownership, product funnel, and purpose:

- **Standard/default files:** `{team}-{funnel}-{Flow (optional)}`
  e.g. `Web-Checkout`, `App-Basket`, `MyStore`.
- **Special-purpose/exploratory/testing files:**
  `{team}-{funnel}-{category}-{TicketNumber}`, where `{category}` is the
  file's objective (`prototype`, `usertest`, …) and a Jira ticket id is
  required. e.g. `Web-PDP-Prototype-EXP-123`, `App-Checkout-UserTest-EXP-456`.
- Flag files missing team/funnel segments, or exploratory files missing a
  ticket number.

### 3.2 Page hierarchy & flow structure

Pages inside a file represent distinct flows and must follow this spine, in
order:

1. `Cover` — mandatory cover artboard with project metadata.
2. `----` — divider.
3. `UserFlow` — English base flow.
4. `UserFlow-{salesline}` *(optional)* — market-specific flow when it
   diverges from the default (e.g. `UserFlow-DE`, `UserFlow-ES`).
5. `---` — divider.
6. `AB-Test` — experimentation pages (§3.3).
7. `---` — divider.
8. `Playground` — scratchpad for exploratory concepts.
9. `ComponentRequests` — queue for new component needs.
10. `Local Components` — components scoped strictly to this file.
11. `Shared Components` — cross-functional components maintained by this
    team.
12. `---` — divider.
13. `Archive` — deprecated designs; all library instances must be detached
    before archiving.

Flag: missing `Cover`, pages out of order, missing dividers between
sections, or archived content with live (non-detached) library instances.

### 3.3 A/B test pages

Pattern: `{Status Indicator} {TicketNumber}-{Salesline}-{TitleOfTest}-{Device}`

Allowed status indicators: ✅ (success/winner), 🏃‍♀️ (running),
⛔️ (failed/rejected), 🛠️ (work in progress).

Valid examples: `✅ EXP123_MMDE_Testname_Desktop`,
`🏃‍♀️ EXP123_MMDE_Testname_Mobile`, `⛔️ EXP123_MMDE_Testname_All`,
`🛠️ EXP123_MMDE_Testname_Tablet`.

Flag: missing/incorrect status emoji, missing ticket number, or a page under
`AB-Test` that doesn't follow the pattern.

### 3.4 Artboards, screens & frames

Pattern: `{pagename}-{state}-{stateDescription}-{screenSize}-{salesline}-{draft}`

- English is the default baseline; omit `{salesline}` unless localized —
  when localized, append the uppercase country/salesline code (`DE`, `AT`,
  `ES`, …) and confirm the UI copy is fully translated.
- `{screenSize}` uses standard responsive tokens: `xs`, `sm`, `md`, `lg`,
  `xl`.
- `{draft}` (`draft1`, `draft2`, …) is only permitted during active
  exploration and must be removed before production sign-off.

Valid examples: `Checkout-Step1-Pickup-xl`, `Checkout-Step1-Delivery-xl`,
`PDP-xl-DE`, `PDP-MarketSelectorOpen-xl-DE`,
`PDP-MarketSelectorOpen-xl-DE-draft1`.

Flag: missing screen-size token, lowercase/invalid salesline code, untranslated
copy on a localized screen, or a lingering `draft*` suffix on a
production-signed-off screen.

### 3.5 Components & design architecture (BEM)

- **Block** — standalone entity with independent semantic meaning
  (`Drawer`, `Button`, `Card`, `Header`).
- **Element** — subordinate part tied to a block with no standalone
  existence (`DrawerItem`, `CardHeader`, `ButtonLabel`). **Never** nest
  element names within element names (no `BlockElement1Element2`).
- **Modifier** — flag indicating appearance/state/behavior change
  (`-primary`, `-open`, `-disabled`).

Pattern: `{Block}{Element}-{Modifier}`. Valid examples: `Drawer`,
`DrawerItem`, `SlotDrawer`, `DrawerMarketSelector`, `Button-Primary`.

Flag: element names nested two or more levels deep, modifiers applied
without a block, or a block name describing content rather than role.

### 3.6 Component properties & variants

- **Property keys:** always `camelCase` — `variant`, `colorVariant`,
  `alignment`, `size`, `type`.
- **Boolean properties:** must start with `is`, `has`, or `show` —
  `hasEEL`, `isSelected`, `isDisabled`, `isLoading`, `hasIconLeading`,
  `showBadge`.
- **Slots & swaps:** must clearly name their location/intended element —
  `slotLeading`, `slotTrailing`, `iconSwap`.
- **Content/text overrides:** `label`, `titleText`, `descriptionText`.

Flag: boolean properties missing the `is`/`has`/`show` prefix, non-camelCase
property keys, or ambiguous slot names (e.g. `slot1`, `icon2`).

### 3.7 Accessibility (A11y) annotations

1. Screens under accessibility audit need a dedicated overlay frame named
   `Accessibility` or `A11y-Annotations`.
2. Focus-order badges must use sequential natural numbering (`1`, `2`,
   `3`, …) matching keyboard reading flow.
3. Interactive elements without visible text need an explicit
   `ScreenReader`/`AltText` annotation with real copy.
4. Semantic heading components must declare their level (`H1`…`H6`)
   without skipped levels in the visual hierarchy.
5. Touch-target overlays must confirm minimum bounding size
   (48×48 dp / 44×44 pt); contrast badges must document WCAG AA compliance
   (4.5:1 body copy, 3:1 large text/icons).

Flag: missing annotation overlay, out-of-sequence/missing focus-order
numbers, interactive icon-only elements with no screen-reader annotation,
skipped heading levels, or missing/failing touch-target or contrast badges.

### 3.8 Variables, tokens & color system

Pattern: `kebab-case` — `{element}-{modifier}` or
`{category}-{role}-{modifier}`. Valid examples: `background`,
`background-hover`, `surface-primary`, `surface-subtle`, `text-primary`,
`text-muted`, `border-focus`.

> Note: this is the MMS org-level kebab-case convention for Figma local
> variable names. Cross-check against `SPECS.md` §3's semantic
> dot-notation grammar (e.g. `text.subtle`) when the file is also expected
> to bind to the MediaMarktSaturn token system — the two naming surfaces
> (Figma variable display name vs. underlying semantic token path) should
> correspond, not contradict.

### 3.9 Branches & conventional commits

- **Figma branch naming:** `{JIRA_NUMBER}_{Description}` — e.g.
  `WTF-234_Checkout_Payment_Redesign`, `EXP-123_PDP_Sticky_Bar`.
- **Commit messages:** `<type>(ticketName/component/page): <description>`,
  type ∈ `feat` (new feature/component), `fix` (bug/alignment/color fix),
  `chore` (cleanup, detaching unused layers, reorganizing pages), `docs`
  (documentation/a11y notes). e.g. `fix(wtf-234): Adapt button color`,
  `feat(wtf-234): Add global language switch`, `chore: Clean Up files`.

Flag: Figma branches without a Jira ticket prefix, or commit messages
missing a valid `type` or ticket/scope reference.

## 4. Master MMS design-system library registry

Designs should consume instances directly from these canonical libraries —
flag any component/style that looks locally recreated instead of pulled
from the matching library below:

| Library | Purpose & scope |
|---|---|
| `LibraryFoundation` | Core styles: colors, elevation, spacing, typography, tokens |
| `LibraryWebComponents` | Web UI components, variants, properties, patterns |
| `LibraryAccessibilityAnnotationKit` | A11y documentation, focus orders, landmark tags |
| `LibraryAppComponents` | Mobile app components (Consumer App, Pick'n'Pack) |
| `LibraryIcons` | Official SVG icon sets and symbols |
| `LibraryIllustrations` | Official brand and promotional illustration assets |
| `LibraryStyleguide` | In-file presentation layouts, notes, and documentation cards |

## 5. Keeping this in sync

This file was transcribed from the MMS "Get Started: Naming Convention"
knowledge base plus the referenced BEM, accessibility-kit, and
web-components-spec sources. If the org's governance doc or any referenced
Figma library spec changes, update this file to match — don't let it drift.
