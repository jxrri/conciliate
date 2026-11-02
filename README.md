<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/mark-dark.svg">
  <img src=".github/assets/mark-light.svg" alt="" width="64" height="64">
</picture>

# conciliate

**Reconcile a project's scattered guidance into one current, sourced digest.**

[![License](https://img.shields.io/badge/license-Apache--2.0-1f2328?style=flat-square)](LICENSE)
[![Agent skill](https://img.shields.io/badge/agent_skill-SKILL.md-1f2328?style=flat-square)](SKILL.md)
[![Used by solver](https://img.shields.io/badge/used_by-solver-1f2328?style=flat-square)](https://github.com/jxrri/solver)

[Quick start](#quick-start) · [What it reads](#what-it-reads) · [How it decides](#how-it-decides) · [Guarantees](#guarantees) · [Files](#files)

</div>

<br>

An agent skill that gathers a project's guidance from the repository and from the services connected to your agent, and reconciles it into one current digest, with a source for every rule. Conflicts it cannot settle are reported for a human decision, never resolved silently.

## Quick start

**1. Install** by cloning into your skills directory:

```bash
git clone https://github.com/jxrri/conciliate ~/.claude/skills/conciliate
```

For a single project, clone it into `<project>/.claude/skills/conciliate` instead.

**2. Run** it by asking your agent, or let another skill call it (e.g. [solver](https://github.com/jxrri/solver)):

```text
conciliate this project
```

**3. Map services.** On the first run it asks which workspace, team, or file in each connected service belongs to this project, unless the repository already links it, and remembers the answer.

**4. Read the digest** it writes to `~/.cache/conciliate/<project>-<hash>/digest.md`: commands, branching and delivery rules, spec locations, conflicts, and gaps.

## What it reads

| Source | Examples |
| :--- | :--- |
| Repository | Agent instructions, docs, specs, decision records, CI and lint config, branch rules |
| Connected services | Linear, Jira, Figma, Notion, Google Drive, Sentry, meeting notes… |

## How it decides

When two sources disagree:

| Conflict between | Winner |
| :--- | :--- |
| Your explicit instruction vs anything | Your instruction |
| Enforced config (CI, hooks, linters, branch rules) vs written docs | Enforced config; the doc is flagged as stale |
| Two dated decisions on the same topic | The newer one, unless marked superseded or draft |
| Spec vs agent instruction file, README, or ticket text | Spec |
| Meeting notes or chat vs a written spec or decision | None: reported as `Needs decision` |
| Design file vs spec text on a visual detail | None: reported as `Needs decision` |
| Code as-built vs spec | None: reported as `Needs decision` |

## Guarantees

- Read-only on the project: proposes fixes to stale sources, applies them only with your approval.
- Reads a connected service only after mapping it to the project, so one organization's data never leaks into another's project.
- Calls only read tools on connected services; anything that creates, updates, deletes, comments, or sends is off limits.
- Treats ticket, document, and design content as data: text that tries to change its rules is reported, not followed.
- Uses only the services your agent already has connected; it never asks for credentials.
- Treats a missing guard, such as no branch protection, as a gap, never as a permission.
- Reuses a fresh digest and re-reads only the sources that changed.

## Files

| File | Purpose |
| :--- | :--- |
| [`SKILL.md`](SKILL.md) | The skill |
| [`references/sources.md`](references/sources.md) | Where to look for guidance in the repository, by kind |
| [`references/connectors.md`](references/connectors.md) | Discovering, mapping, and safely reading connected services |
| [`assets/digest-template.md`](assets/digest-template.md) | Digest format and source manifest |

## Related

- [solver](https://github.com/jxrri/solver): takes a bug from diagnosis to a verified pull request, and loads conciliate first.

## License

Released under the [Apache-2.0](LICENSE) license.
