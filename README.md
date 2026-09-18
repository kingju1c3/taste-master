# Taste Master

**Anti-AI-slop front-end design skill.**

Taste Master is a Claude skill (`/taste-master`) that turns generic-looking AI-generated
web UI into deliberate, brief-specific design work. It's a judgment process, not a
template: it reads the brief, sets three tunable "dials" (Variance, Motion, Density),
plans and critiques a design before writing code, and then checks the rendered output
against a long mechanical pre-flight list of known AI design tells (default beige/clay
palettes, purple gradient heroes, three identical rounded cards, fake dashboards, etc.)
before calling anything done.

See [`taste-master-skill.md`](./taste-master-skill.md) for the full skill text (v3,
2026-09-09).

## What's in it

- **Judgment gate** — extract page kind, vibe, references, audience, and constraints
  from the brief before any rule fires.
- **Three dials** — Variance / Motion / Density, set from the brief instead of a fixed
  default, with a table mapping common brief signals to starting values.
- **Design-system routing** — sends dashboards, data tables, forms, and native mobile to
  the *right* tool (Fluent, Carbon, Polaris, etc.) instead of force-fitting a marketing
  aesthetic onto them.
- **Plan → critique → build** — a two-pass discipline that checks a design plan against
  known AI-generated clusters (warm cream + terracotta, near-black + acid accent,
  broadsheet, SaaS-card kit, template chrome) before code is written.
- **Typography, color, layout, motion, copy, and interactive-state rules** — concrete,
  checkable defaults (contrast ratios, hero element caps, motion engineering floor,
  banned filler copy, etc.), all overridable by an explicit brief.
- **A named pattern vocabulary** — hero paradigms, navigation patterns, scroll behaviors,
  and micro-interactions to reach for deliberately, so the skill expands the solution
  space rather than only restricting it.
- **A mechanical pre-flight checklist** (Section 16) and a rotation log (Section 15) so
  repeated builds don't converge on the same palette, font, and layout every time.

## Usage

This file is a portable copy of the skill's instructions. To use it as a Claude Code
skill, drop `taste-master-skill.md` into a project's skills directory (or wherever your
Claude Code setup loads custom skills from) and trigger it with `/taste` or
`/taste-master`, or by asking for a landing page / redesign / UI build.

## License

No license specified — content shared for personal/reference use.
