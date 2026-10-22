# Using the mattpocock Skills to Rebrand This Fork

Goal: replace the `redamon` branding — UI, colors, logos, and the application
name — so that `redamon` no longer appears anywhere, including in code.

## Reality check

A case-insensitive scan finds **`redamon` in ~1,085 files**. This is not a UI
tweak. It is two very different jobs wearing one request.

### Job A — the visual rebrand (design judgement)
Colors, logo, UI. You have to *see* this to know it's right. Grilling can't
settle "what should the palette be" — that's a prototype question.

### Job B — the rename (mechanical, but mined)
`redamon` is not just display text. It appears in:
- `redamon.sh` (the control-plane script)
- Docker image names (`redamon-agent`, `redamon-recon`, …)
- Compose service names, env var prefixes
- Graph DB tenant-isolation keys (`graph-db-writes`)
- Skill names, test fixtures, golden settings

Rename the wrong identifier and the stack stops building. "Not even in code" is
the dangerous clause — it needs scoping, not a blind find-and-replace.

## Recommended path through the skills

### 1. `/grill-with-docs` first — one session, before any code
Highest-leverage step. Questions that must be nailed down:
- Does "redamon shouldn't appear in code" mean **user-facing strings only**, or
  truly **every identifier** — image names, `redamon.sh`, service names, env var
  prefixes, DB keys?
- What is the new name? (Needed for both jobs.)
- Is the fork staying in sync with upstream `samugit83/redamon`? If yes,
  renaming every internal identifier makes every future merge a conflict
  minefield — a real architectural cost to decide consciously.

Leaves an ADR + `GLOSSARY.md`, so the naming decision is written down — which
matters when the rename touches ~1,000 files.

### 2. For Job A, detour to `frontend-design` + `/prototype`
Spin up a throwaway to see the new palette/logo in the actual UI before
committing. Fold the answer back in. Don't settle colors in prose.

### 3. Split at `/to-spec` → `/to-tickets`
This is genuinely multi-session. Layer the tickets by **blast radius**, not by
folder:
- user-facing strings + assets (safe, reversible)
- config/docs
- internal identifiers: image names, script name, service names (the risky
  tier — one ticket, carefully, gated on a green `./redamon.sh test unit`)

### 4. `/implement` per ticket, `/clear` between
The rename tickets are mechanical enough that each is self-contained.

## One thing to flag now

The repo's own rules fight part of this plan. `./redamon.sh` is named in
CLAUDE.md as *the* control plane, and skills like `graph-db-writes` key tenant
isolation on identifiers. Renaming internals is doable but it is not cosmetic —
it's a refactor with a test gate. The grill session is where you decide whether
the internal rename is worth it, or whether you stop at the user-facing
boundary.

## Next step

Either kick off `/grill-with-docs` on this now, or do a quick read-only survey
first to categorize the ~1,085 files into "display string" vs "load-bearing
identifier" buckets, so the grilling starts from facts.
