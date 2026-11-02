---
name: conciliate
description: "Trigger: conciliate, reconcile guidelines, are the rules current, before starting project work. Gather and reconcile project guidance into one current digest."
license: Apache-2.0
metadata:
  author: "David Reyes"
  version: "1.0"
---

## Activation Contract

Load when:
- Another skill (e.g. `solver`) starts work and needs the project's current rules.
- The user asks to reconcile, refresh, or audit the project's guidelines.
- Sources visibly disagree (docs vs CI, spec vs code) and the work depends on which one holds.

Works on one repository, a workspace holding several repositories, or an organization whose sources are declared.

## Hard Rules

- Read-only on the project: never edit, delete, or rewrite a source. Propose source fixes in the output; apply them only with explicit user approval.
- Never resolve a conflict silently. Apply the precedence table; anything it cannot decide goes to `Needs decision`.
- Every rule in the digest cites its source (`path:line`, URL, or ticket ID).
- Read only sources found locally or declared by the project or the user, through tools already available. Never assume access to external systems.
- Code shows what IS; specs and decisions show what SHOULD BE. A mismatch between them is always reported, never auto-resolved.
- A missing guard (e.g. no branch protection) is a gap, never a permission.
- Reuse a fresh digest; re-read only sources that changed since it was written.

## Decision Gates

| Conflict between | Winner |
|---|---|
| Explicit user instruction in this session vs anything | User instruction |
| Enforced config (CI, hooks, linters, branch rules) vs written docs | Enforced config; flag the doc as stale |
| Two dated decisions on the same topic | The newer one, unless marked superseded or draft |
| Spec vs agent instruction file or README | Spec |
| Code as-built vs spec | None → `Needs decision` |
| Same-rank sources without dates | None → `Needs decision` |

| Scope | Read |
|---|---|
| `scoped` (default when another skill calls) | Global rules + sources for the task's paths or modules |
| `full` | Every discovered source |

## Execution Steps

1. Resolve the root (git top-level, or the workspace directory) and the cache path `${XDG_CACHE_HOME:-$HOME/.cache}/conciliate/<root-name>-<first 8 hex of sha256(abs root)>/digest.md`.
2. If a digest exists, compare its source manifest with the current revision of each source; if nothing changed, return it.
3. Discover sources with `references/sources.md`; include external sources only when declared.
4. Extract rules per topic: commands, branching and delivery, specs, conventions, security and data.
5. Detect conflicts and gaps; resolve with the precedence table.
6. Write the digest from `assets/digest-template.md`, including its manifest, and return it.

## Output Contract

Return the digest path and a summary:
- Sources read and rule count per topic.
- `Conflicts resolved`: rule, winner, overridden source, reason.
- `Needs decision`: each conflict with both sources; say "none" when empty.
- `Gaps`: topics with no source (e.g. no test command, no branch policy).
- Proposed source fixes, marked as not applied.

## References

- `references/sources.md` — where to look for guidance, by kind.
- `assets/digest-template.md` — digest format, including the source manifest.
