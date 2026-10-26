# Update policy

The Mend Renovate App can read `org-inherited-config.json` as inherited configuration for its installed repositories.
That file turns onboarding off and extends `default.json`.
It makes repository configuration optional.
The default file holds the shared update policy.

## Use the preset

Repositories using the inherited configuration don't need their own `renovate.json`.
A repository can add its own file to layer project settings onto the inherited policy.
To extend the same preset explicitly, add the following to that repository's `renovate.json`:

```json
{
  "extends": ["github>P4suta/renovate-config"]
}
```

## Update rules

The preset extends `config:recommended`, groups non-major updates, and pins GitHub Actions to commit digests.
Normal non-major updates share one pull request per repository, scheduled before 09:00 on Monday in `Asia/Tokyo`.
A release must be at least three days old before Renovate proposes it.
The preset enables automerge for minor, patch, digest, pin, `pinDigest`, and lockfile-maintenance updates after required checks pass.
It also enables automerge for vulnerability alerts.
Major updates wait for a human and carry the `major` label.

## Commit titles

Commits use the `build(deps): …` prefix and the platform commit API so GitHub signs them.
The recommended preset normally selects `fix(deps)` for runtime dependencies and `chore(deps)` for other updates.
Ignoring `:semanticPrefixFixDepsChoreOthers` keeps the shared `build(deps)` prefix.
That avoids requesting patch releases in projects whose release automation treats `fix` commits as release changes.

## Validate a policy change

Run the existing strict validator from the policy checkout:

```sh
npx --yes --package renovate@latest -- renovate-config-validator --strict default.json org-inherited-config.json
```
