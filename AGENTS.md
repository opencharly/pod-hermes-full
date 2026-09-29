# AGENTS.md — pod-hermes-full

Standalone candy repo for the `hermes-full` candy — a pure meta-composition of
the Hermes agent, the AI coding CLIs, and the developer / DevOps toolchains. The
candy lives in `charly.yml` at the repo root.

Canonical files:

- `charly.yml` — the `hermes-full:` candy entity (description, the composed
  `candy:` list, `plan:`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-hermes:hermes-full-layer` — the closest family skill: the metalayer
  composition details. **This candy has no `skill:` entity of its own** — the gap
  is recorded on
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-hermes:hermes` — the core agent candy this composition includes.
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference (this candy is
  composed into a box; tree-position nesting, volumes, ports).
- `/charly-check:check` — the check/R10 framework: the `check:` step verbs and
  `charly check run <bed>`.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs, the inline `candy:` composition list).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The live R10 witness is a composing box's `check` bed; the candy's own `check:`
  steps assert one key artifact per composed candy (the hermes binary +
  entrypoint, each AI CLI on PATH, `rg` / `nvim`, `tmux`, `aws` / `tofu` / `jq`)
  and the running hermes service.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.

## Modify this repo

- Edit the `hermes-full:` candy entity in `charly.yml`; there is no `skill:`
  entity in this repo (the gap is tracked on opencharly/opencharly#291).
- This is a pure composition: add or remove candies in the `candy:` list and add
  the matching `check:` in the same change — a composed candy without a check is
  invisible, and a check without its candy fails.
- Keep the composed pins in step with the sibling `pod-hermes` repo; a version
  bump to `pod-hermes` here must match the released tag.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
