# renovate-config

Shared Renovate preset for KyTiXo repos.

## Usage

In a repo's `renovate.json`, add only repo-specific settings (e.g. `ignorePaths`) on top:

```json
{
  "extends": ["github>KyTiXo/renovate-config"]
}
```

## What it does

- Extends `config:best-practices` and `:semanticCommits`.
- Waits 3 days after a release before proposing it (`minimumReleaseAge`), for every manager.
- Security fixes (GitHub vulnerability alerts) skip the 3-day wait and any schedule. Dependabot alerts must be enabled on the repo.
- Automerges only:
  - `devDependencies` minor/patch/pin/digest updates (pins only once the pinned release is 3 days old)
  - Non-major GitHub Actions `uses:` updates (actions and reusable workflows), grouped into one PR. Action majors get their own grouped PR and stay manual. Versions passed to setup actions (e.g. `node-version`, `bun-version`), runner labels and container images stay manual.
- Everything else, including runtime dependencies, waits for a manual merge.
- Automerge happens only after CI passes. GitHub native auto-merge is off (`platformAutomerge: false`) because target repos have no required status checks. A branch with no checks at all never automerges.
