# Guidance sources

Discover in this order. Record every source found in the manifest, even when it adds no rule.

## Agent and contributor instructions

- `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `.cursorrules`, `.cursor/rules/`, `.windsurfrules`, `.github/copilot-instructions.md`
- `CONTRIBUTING.md`, `README.md`, `docs/`, `.github/PULL_REQUEST_TEMPLATE*`, `CODEOWNERS`

## Decisions and specs

- Decision records: `docs/adr/`, `docs/decisions/`, `decisions/`, `wiki/decisions/`
- Specs: `specs/`, `spec/`, `docs/specs/`, `openapi.*`, `*.proto`, API contract files, mockups referenced by a spec
- Any spec or knowledge-base location named by the instruction files, including paths given through environment variables they mention

## Enforced configuration (what the machine checks)

- CI: `.github/workflows/`, `.gitlab-ci.yml`, `.circleci/`, `azure-pipelines.yml`, `Jenkinsfile`
- Hooks: `.husky/`, `.pre-commit-config.yaml`, `lefthook.yml`, `.git/hooks/` (local only)
- Lint, format, types: `eslint.config.*`, `.eslintrc*`, `biome.json`, `.prettierrc*`, `tsconfig*.json`, `pyproject.toml`, `ruff.toml`, `.golangci.yml`, `.editorconfig`
- Commit rules: `commitlint.config.*`, `.czrc`
- Branch rules, when `gh` is authenticated: `gh api repos/{owner}/{repo}/rulesets` and `gh api repos/{owner}/{repo}/branches/{branch}/protection`

## Commands

- `package.json` scripts, `Makefile`, `justfile`, `Taskfile.yml`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `docker-compose*.yml`, `scripts/`
- Prefer the commands CI runs over the commands docs mention.

## Connected services

- Issue trackers, design tools, docs, meetings, monitoring, and chat: follow `connectors.md`. Links to them found in the files above are mapping evidence.

## Workspace with several repositories

- Treat each repository as a source group. Mark every rule `global` or `repo:<name>`.
- A rule from one repository never applies to another unless a shared source says so.
