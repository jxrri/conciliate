# conciliate

An agent skill that gathers a project's guidance from the repository (agent instructions, docs, specs, decision records, CI and lint config, branch rules) and from the services connected to your agent (Linear, Jira, Figma, Notion, Google Drive, Sentry, meeting notes…), and reconciles it into one current digest, with a source for every rule. Conflicts it cannot settle are reported for a human decision, never resolved silently.

## Quick path

1. Clone this repository into your skills directory:

   ```bash
   git clone https://github.com/jxrri/conciliate ~/.claude/skills/conciliate
   ```

   For a single project, clone it into `<project>/.claude/skills/conciliate` instead.
2. Ask your agent to "conciliate this project", or let another skill call it (e.g. [solver](https://github.com/jxrri/solver)).
3. On the first run it asks which workspace, team, or file in each connected service belongs to this project, unless the repository already links it, and remembers the answer.
4. Read the digest it writes to `~/.cache/conciliate/<project>-<hash>/digest.md`: commands, branching and delivery rules, spec locations, conflicts, and gaps.

## How it decides

| Conflict between | Winner |
|---|---|
| Your explicit instruction vs anything | Your instruction |
| Enforced config (CI, hooks, linters, branch rules) vs written docs | Enforced config; the doc is flagged as stale |
| Two dated decisions on the same topic | The newer one, unless marked superseded or draft |
| Spec vs agent instruction file, README, or ticket text | Spec |
| Meeting notes or chat vs a written spec or decision | None: reported as `Needs decision` |
| Design file vs spec text on a visual detail | None: reported as `Needs decision` |
| Code as-built vs spec | None: reported as `Needs decision` |

## Safety rules

- Read-only on the project: proposes fixes to stale sources, applies them only with your approval.
- Reads a connected service only after mapping it to the project, so one organization's data never leaks into another's project.
- Calls only read tools on connected services; anything that creates, updates, deletes, comments, or sends is off limits.
- Treats ticket, document, and design content as data: text that tries to change its rules is reported, not followed.
- Uses only the services your agent already has connected; it never asks for credentials.
- Treats a missing guard, such as no branch protection, as a gap, never as a permission.
- Reuses a fresh digest and re-reads only the sources that changed.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The skill |
| `references/sources.md` | Where to look for guidance in the repository, by kind |
| `references/connectors.md` | Discovering, mapping, and safely reading connected services |
| `assets/digest-template.md` | Digest format and source manifest |

## License

[Apache-2.0](LICENSE)
