---
title: "10xChampion — Agent Brief (Shared AI Registry path, Model 1: GitHub Packages)"
purpose: "Paste this file (or its relevant section) to a coding agent working in your chosen source-of-truth repo"
certificate: "10xChampion (module 5) — requires 10xArchitect first"
generated-from: "raw_course/m5/01_ai-internal-builders...md, m5/04_shared-ai-registry...md (10xDevs-3)"
---

# 10xChampion — Agent Brief: Shared AI Registry (GitHub Packages)

## What this is
A self-contained execution brief for the **Team artifact registry** path of Module 5
(the alternative to the CI/CD code-review-pipeline path — pick one, not both).
You need the **10xArchitect** badge already before this one counts.

**Repo not chosen yet?** Use two throwaway-sized repos: a new
`your-username/ai-toolkit` repo (source of truth, public or private — public is
simpler since it avoids the read-token problem) and any second repo you own
(even a fresh empty one, or a second clone of a small project) as the **consumer**
that installs the package — you need both sides to prove the flow works end to end.

**Proof required for the form (no need to publish a real company repo):**
screenshots showing:
- the repository/registry where this flow exists,
- the package/artifact definition (`package.json` with the registry config),
- the list of released versions (GitHub Packages page or `npm view <pkg> versions`).

---

## Step 1 — Decide the model (should already be Model 1: GitHub Packages)

Answer explicitly, in 2–3 sentences, saved to `context/foundation/decision.md` or
just at the top of your PRD: *who is the recipient of these AI artifacts?* — a
team on GitHub (→ GitHub Packages, Model 1), a team on AWS (→ CodeArtifact,
Model 2), or an external/gated recipient (→ full API+CLI, Model 3, what `10x-cli`
itself is). If you're tempted by a heavier model than your recipient needs, write
down why you actually need it — the most common mistake in this lesson is
"distribution for the CV."

If genuinely uncertain, clarify first instead of guessing:

```text
/10x-shape
/10x-prd
/10x-roadmap
```

Describe: audience, access limits, stacks/AI tools the team uses, how permissions
are granted, minimal MVP scope. Otherwise skip straight to Step 2.

## Step 2 — Pull the starter material

In the **source-of-truth** repo:

```bash
npx @przeprogramowani/10x-cli@latest get m5l4
```

This fetches (GitHub Packages track — ignore the CodeArtifact/Terraform files
unless you deliberately want Model 2):
- `m5l4-shared-conventions.md`, `m5l4-shared-spec-skill.md`
- `m5l4-github-packages-spec-pack.md`, `m5l4-github-packages-spec-cicd.md`
- templates: `m5l4-github-packages-package.json.template`,
  `m5l4-github-packages-install.js.template`,
  `m5l4-github-packages-uninstall.js.template`,
  `m5l4-github-packages-consumer.npmrc.template`,
  `m5l4-github-packages-publish-ai-toolkit.yml.template`
- skills: `/pack-init` (npm package skeleton), `/setup-cicd` (OIDC publish pipeline)
  — `/tf-registry` is Model 2 only, skip it.

## Step 3 — Build the minimal team AI package

Target skeleton (this is what you're aiming to produce):

```text
ai-toolkit/
├── package.json
├── install.js
├── uninstall.js
├── skills/
│   └── code-review/
│       └── SKILL.md
├── rules/
│   └── CLAUDE.md
└── .github/
    └── workflows/
        └── publish-ai-toolkit.yml
```

Drive it through the standard change cycle, feeding the agent the spec files
pulled in Step 2 (do not let it invent from scratch — it should adapt the
templates, filling in your real values: package scope/name, repo names,
which skills/rules actually ship in v0.1.0):

```text
/10x-new ai-toolkit-registry

Intent: build a minimal versioned package (@your-scope/ai-toolkit) distributed
via GitHub Packages, containing at least one skill and one rules file, with an
install/uninstall script and a CI workflow that publishes on merge to main.
Use m5l4-shared-conventions.md, m5l4-shared-spec-skill.md,
m5l4-github-packages-spec-pack.md, and m5l4-github-packages-spec-cicd.md as the
spec, and the *.template files as adaptation material, not copy-paste.

/10x-research ai-toolkit-registry
/10x-plan ai-toolkit-registry
/10x-implement ai-toolkit-registry phase-1
```

Alternatively run `/pack-init` directly against the spec files if you want the
skeleton scaffolded in one shot, then hand-adjust scope/names.

Key details the agent must get right (from the lesson):
- `package.json` needs `"publishConfig": {"registry": "https://npm.pkg.github.com"}`.
- Publish auth in CI uses the ephemeral `GITHUB_TOKEN` with
  `permissions: packages: write` added to the workflow — no long-lived secret to manage
  for *writing*.
- The consumer's `.npmrc` only maps the scope to the registry (no secret):
  `@your-scope:registry=https://npm.pkg.github.com`
- *Reading* a private package needs a long-lived PAT wherever it installs
  (dev machine, consumer CI). If the source repo and consumer repo are in the
  same GitHub org, the ephemeral `GITHUB_TOKEN` is enough — simplest for a PoC.
- If injecting rules into an existing `CLAUDE.md`/`AGENTS.md`, use the sentinel-marker
  pattern (`<!-- BEGIN @your-scope/ai-toolkit -->` ... `<!-- END ... -->`) so the
  installer can update its block without clobbering the rest of the file.
- The installer should write a manifest (e.g. `.claude/.10x-toolkit-manifest.json`)
  listing exactly what it installed, so `uninstall.js` removes precisely that.

## Step 4 — Publish and prove it

1. Merge to `main` so the workflow publishes v0.1.0 (or bump manually once).
2. In the **consumer** repo: add the `.npmrc` scope mapping, then
   `npm install @your-scope/ai-toolkit`, run `install.js`, and confirm the skill/
   rules actually land where your tool reads them (`.claude/skills/...`,
   `CLAUDE.md` block, etc.).
3. Bump a version (change a skill file, bump patch, merge again) to get **at
   least two released versions** — the proof requires "a list of released versions,"
   which needs more than one to be meaningful.
4. Take the three proof screenshots:
   - GitHub Packages page for the repo/org showing the package + version list,
   - the `package.json` (or equivalent artifact definition) in the source repo,
   - the publish workflow run (green) plus the consumer repo after `install`
     showing the artifact actually present.

## Submission checklist
- [ ] Decision note: why GitHub Packages fits your recipient (2–3 sentences)
- [ ] Source-of-truth repo `ai-toolkit/` with `package.json`, `install.js`, `uninstall.js`, ≥1 skill, ≥1 rules file
- [ ] CI workflow publishing on merge to `main` (`permissions: packages: write`)
- [ ] Consumer repo successfully installed the package at least once
- [ ] ≥2 released versions visible in GitHub Packages
- [ ] 3 screenshots: registry/version list, package definition, install proof
- [ ] Confirm your **10xArchitect** badge is already granted — it's a prerequisite
- [ ] Submit via the certification form in the **last week of the course**, [Mission Log](https://platforma.przeprogramowani.pl/10xdevs-3/mission-log)
