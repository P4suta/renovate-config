# renovate-config

The dependency-update policy shared by every P4suta repository.

The Mend Renovate App reads `org-inherited-config.json` from this repository and applies it to every repository it is installed on, so no repository needs a `renovate.json` of its own.
That file turns onboarding off and extends `default.json`, which holds the policy itself.

- Minor, patch, digest, and lockfile-maintenance updates merge automatically once CI is green.
- Major updates wait for a human and carry the `major` label.
- Every non-major update for a repository lands in one pull request, opened before 9am on Monday (Asia/Tokyo).
- A release has to be three days old before Renovate proposes it.
- Commits are titled `build(deps): …` so Conventional Commits gates accept them, and they are made through the platform so they are signed.
  `config:recommended` would title runtime dependencies `fix(deps)` and the rest `chore(deps)`, so `:semanticPrefixFixDepsChoreOthers` is ignored; a `fix` would make release automation cut a patch release for every update.

A repository that needs more can add its own `renovate.json`, which is layered on top of this policy; `github>P4suta/renovate-config` extends the same policy explicitly.

Check a change with `npx --yes --package renovate@latest -- renovate-config-validator --strict default.json org-inherited-config.json`.
