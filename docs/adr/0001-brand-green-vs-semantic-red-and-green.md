# 0001: Brand green must stay clear of critical-red and success-green

- **Status:** Accepted
- **Date:** 2026-10-22

## Context

We are rebranding the webapp to a black / green / white "cyber" palette,
dark-first. The brand accent moves from crimson (`#e53935`) to green.

The webapp's color system (custom CSS tokens, no Tailwind) separates two layers:

- **Brand/accent tokens** — `--accent-primary*` in `src/styles/tokens/semantic.css`,
  today mapped to the `--color-crimson-*` ramp. Used for links, active states,
  hover, and previously-crimson text/headings.
- **Semantic tokens** — severity, status, graph-node, and table colors, also in
  `semantic.css`. These encode *meaning* users rely on: **critical = red**,
  **success/pass = green** (`#22c55e`).

The rebrand collides with this layer twice:

1. If the brand accent becomes green, it shares a hue with **success/pass green**.
2. "Turn the red things green" naively read would also recolor **critical red**,
   making a CRITICAL finding the same color as a SUCCESS — a safety problem in a
   penetration-testing tool.

## Decision

1. **Brand accent = green `#00c853`**, fully replacing crimson for accent usages
   (links, active, hover, previously-crimson text). `#00c853` was chosen over the
   brighter `#00e676` precisely because it sits *further* from success green,
   verified in the `webapp/prototypes/rebrand-palette.PROTOTYPE.html` comparison
   across both themes.
2. **Critical severity stays red** (`--color-red` / `#e53935`). The red→green
   change applies *only* to brand-accent red, never to semantic severity.
3. **Two greens coexist and must stay visually distinct**: brand green (`#00c853`)
   vs. success green (`#22c55e`). Separation was confirmed in the prototype across
   dark + light. The unused fallback, had they been too close, was a **cyan accent**
   (`#22d3ee`) with green reserved for chrome/glow only.

## Resolved design parameters (prototype verdict, 2026-10-22)

- Brand accent: **`#00c853`** (variant 3 of 4).
- Dark theme: soft-black background **`#0b0b0c`** (not pure `#000`), white text,
  buttons white at rest → green on hover + green focus ring.
- Light theme: white background, near-black text, same green-on-hover behavior.
- Secondary accent: **blue kept for now** (not retired).
- Primary CTA: **white/neutral at rest, green on hover** (no resting green fill);
  promote a single CTA to green fill only if it later reads as buried.
- Frozen: severity / status / graph-node / table semantic colors.

## Consequences

- A future reader will see critical findings still rendered red while everything
  else that was brand-red went green. **This is intentional** — do not "fix" it.
- A future reader will see two greens with different meanings. **Also intentional**
  — do not collapse them to one token.
- The semantic layer (severity, status, graph-node, table colors) is otherwise
  frozen for this rebrand.
- Rejected alternative: recoloring critical to green. It would invert a convention
  every security tool shares and make critical findings indistinguishable from
  success — unacceptable.
