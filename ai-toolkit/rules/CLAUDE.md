# Team AI conventions (@zanrafal/ai-toolkit)

These conventions apply to any repo that installs `@zanrafal/ai-toolkit`.
They are re-injected between the sentinel markers on every install, so edit
them in the toolkit source, not here.

## Change flow

- Use `/10x-new`, `/10x-plan`, `/10x-implement` for any non-trivial change.
  One-line fixes can skip the ceremony.
- Never edit files under `context/foundation/` by hand — regenerate them via
  their owning skill (`/10x-prd`, `/10x-roadmap`, `/10x-stack-assess`, …).
- Archive completed changes with `/10x-archive`; don't leave finished
  folders in `context/changes/`.

## Code review

- Invoke the `code-review` skill (shipped by this toolkit) before every merge
  to `main`. Correctness beats style; every finding needs a concrete failure
  scenario.
- Do not merge with `--no-verify` or bypassed hooks.

## Commits

- Small, focused commits over one giant blob. If a commit description would
  need "and" more than twice, split it.
- Never commit secrets (`.env`, tokens, service-account JSON). If you spot one
  in a diff, stop and rotate.

## AI agent hygiene

- Prefer editing existing files over creating new ones.
- Do not create planning / decision / analysis docs (`NOTES.md`, `PLAN.md`)
  unless the user asks — the `/10x-*` skills already write to the right paths.
- Trust framework and stdlib guarantees; only validate inputs at true system
  boundaries.
