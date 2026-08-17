# Distribution model decision

**Chosen model:** Model 1 — GitHub Packages (npm scope on `npm.pkg.github.com`).

**Recipient:** a small team already on GitHub, using Claude Code as the primary
AI tool. Access is granted by GitHub org/repo membership, so we can lean on the
platform's existing auth instead of building new identity plumbing.

**Why not Model 2 or Model 3:** we don't have an AWS org to justify CodeArtifact +
Terraform, and there is no external/gated audience that would need the full
API+CLI product treatment. GitHub Packages matches the recipient exactly; anything
heavier would be "distribution for the CV" and not for the users.
