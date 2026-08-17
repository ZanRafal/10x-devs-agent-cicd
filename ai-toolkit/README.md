# @zanrafal/ai-toolkit

Team AI artifacts (Claude Code skills and shared rules) distributed as a versioned
npm package on **GitHub Packages**.

## Install

In the consuming repo:

```bash
# .npmrc — scope mapping (no secret in this file)
echo "@zanrafal:registry=https://npm.pkg.github.com" >> .npmrc

# needs a PAT with `read:packages` in ~/.npmrc for private installs;
# same-org GITHUB_TOKEN is enough inside org CI
npm install @zanrafal/ai-toolkit
```

The postinstall hook writes:

- skills into `.claude/skills/<skill-name>/`
- shared rules into a sentinel-marked block in `CLAUDE.md`
- an install manifest at `.claude/.ai-toolkit-manifest.json`

## Uninstall

```bash
node node_modules/@zanrafal/ai-toolkit/uninstall.js
npm uninstall @zanrafal/ai-toolkit
```

Uninstall reads the manifest and removes exactly what was installed — the rest
of your `CLAUDE.md` is preserved.

## What's inside

- `skills/code-review/SKILL.md` — team code-review rubric
- `rules/CLAUDE.md` — shared conventions injected into the consumer's CLAUDE.md

## Publishing

Merges to `main` publish a new version via `.github/workflows/publish-ai-toolkit.yml`
using the ephemeral `GITHUB_TOKEN` (no long-lived secret needed for writes).
Bump `package.json#version` before merging.
