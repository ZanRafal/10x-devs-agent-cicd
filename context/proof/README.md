# 10xChampion (Module 5) — Submission bundle

Path: **Shared AI Registry, Model 1 — GitHub Packages.**
Prerequisite: 10xArchitect (Module 4) already granted (done in a separate repo).

## Screenshots in this folder

| File | Requirement it satisfies |
|---|---|
| `2_versions.png` | List of released versions (**≥2** required — shows `0.1.0` and `0.1.1`) |
| `package_definition.png` | Package/artifact definition (`package.json` with `publishConfig.registry`) |
| `install_in_consumer.png` | Successful install in a consumer repo |

## Live links (paste into the form if it asks for URLs)

### Source-of-truth repo & package
- **Source repo:** https://github.com/ZanRafal/10x-devs-agent-cicd
- **Package folder in repo:** https://github.com/ZanRafal/10x-devs-agent-cicd/tree/main/ai-toolkit
- **`package.json` (with `publishConfig`):** https://github.com/ZanRafal/10x-devs-agent-cicd/blob/main/ai-toolkit/package.json
- **GitHub Packages listing (version list):** https://github.com/ZanRafal/10x-devs-agent-cicd/pkgs/npm/ai-toolkit
- **Package name (npm):** `@zanrafal/ai-toolkit`
- **Published versions:** `0.1.0`, `0.1.1`

### CI/CD (publishing pipeline)
- **Workflow file:** https://github.com/ZanRafal/10x-devs-agent-cicd/blob/main/.github/workflows/publish-ai-toolkit.yml
- **Workflow runs:** https://github.com/ZanRafal/10x-devs-agent-cicd/actions/workflows/publish-ai-toolkit.yml
- Auth model: ephemeral `GITHUB_TOKEN` with `permissions: packages: write` — no long-lived secret.

### Consumer repo (install proof)
- **Consumer repo:** https://github.com/ZanRafal/ai-toolkit-consumer
- **Consumer `.npmrc` (scope mapping):** https://github.com/ZanRafal/ai-toolkit-consumer/blob/main/.npmrc
- **Consumer `package.json` (dependency pinned):** https://github.com/ZanRafal/ai-toolkit-consumer/blob/main/package.json
- **Rules block landed in consumer `CLAUDE.md`:** https://github.com/ZanRafal/ai-toolkit-consumer/blob/main/CLAUDE.md
- **Skill file landed in consumer:** https://github.com/ZanRafal/ai-toolkit-consumer/blob/main/.claude/skills/code-review/SKILL.md
- **Install manifest (records what was installed):** https://github.com/ZanRafal/ai-toolkit-consumer/blob/main/.claude/.ai-toolkit-manifest.json

### Local artifacts in this repo
- **Decision note (why Model 1):** `context/foundation/decision.md`
- **Agent brief used:** `m5-champion-agent-brief.md`

## Consumer install steps (for the form's "how does it install" question)

```bash
# 1. In the consumer, map the scope to GitHub Packages
echo "@zanrafal:registry=https://npm.pkg.github.com" >> .npmrc
echo '//npm.pkg.github.com/:_authToken=${GH_PACKAGES_TOKEN}' >> .npmrc

# 2. Provide a classic PAT with read:packages scope (env var only, not in git)
set "GH_PACKAGES_TOKEN=ghp_..."

# 3. Install — postinstall places the skill + rules in the right paths
npm install @zanrafal/ai-toolkit
```

## Submission

Certification form appears in the last week of the course, in the
[10xDevs Mission Log](https://platforma.przeprogramowani.pl/10xdevs-3/mission-log).
Attach the three screenshots above and paste the source-repo URL if asked.
