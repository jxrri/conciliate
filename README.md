# conciliate

An agent skill that gathers a project's guidance (agent instructions, docs, specs, decision records, CI and lint config, branch rules) and reconciles it into one current digest, with a source for every rule. Conflicts it cannot settle are reported for a human decision, never resolved silently.

## Quick path

1. Clone this repository into your skills directory:

   ```bash
   git clone https://github.com/jxrri/conciliate ~/.claude/skills/conciliate
   ```

   For a single project, clone it into `<project>/.claude/skills/conciliate` instead.
2. Ask your agent to "conciliate this project", or let another skill call it (e.g. [solver](https://github.com/jxrri/solver)).
3. Read the digest it writes to `~/.cache/conciliate/<project>-<hash>/digest.md`: commands, branching and delivery rules, spec locations, conflicts, and gaps.

## How it decides

| Conflict between | Winner |
|---|---|
| Your explicit instruction vs anything | Your instruction |
| Enforced config (CI, hooks, linters, branch rules) vs written docs | Enforced config; the doc is flagged as stale |
| Two dated decisions on the same topic | The newer one, unless marked superseded or draft |
| Spec vs agent instruction file or README | Spec |
| Code as-built vs spec | None: reported as `Needs decision` |

## Safety rules

- Read-only on the project: proposes fixes to stale sources, applies them only with your approval.
- Reads external systems (issue trackers, wikis, drives) only when the project or you declare them.
- Treats a missing guard, such as no branch protection, as a gap, never as a permission.
- Reuses a fresh digest and re-reads only the sources that changed.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The skill |
| `references/sources.md` | Where to look for guidance, by kind |
| `assets/digest-template.md` | Digest format and source manifest |

## License

[Apache-2.0](LICENSE)
