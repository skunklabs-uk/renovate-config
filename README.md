# renovate-config

Shared Renovate presets for repositories owned by `skunklabs-uk`.

## Presets

- `github>skunklabs-uk/renovate-config` — default policy with Dependency Dashboard, strict internal checks, immediate PR creation, and minimum release ages of 7 days for patch, 14 days for minor, and 30 days for major updates of external dependencies. Docker updates use `timestamp-optional`: when a registry exposes a release timestamp the normal cooldown applies; when it does not, the update is not blocked indefinitely only because its age cannot be determined.
- `github>skunklabs-uk/renovate-config:automerge` — extends the default preset and enables PR automerge for patch and digest updates, subject to the same freshness and ownership rules.

`prCreation` is intentionally `immediate`: the shared preset must also work in repositories that only run CI for pull requests or protected branches and do not execute checks on `renovate/**` branches. Minimum release age remains the freshness gate for external dependencies when the datasource provides a supported release timestamp.

## OCI release coordinates

All proprietary OCI producers consumed by Homelab use the same immutable release-coordinate format:

`YYYY.MM.DD-bNNNNNN`

Example: `2026.08.30-b000187`.

`NNNNNN` is the zero-padded `GITHUB_RUN_NUMBER` of the canonical producer workflow. This makes versions unique for new runs and monotonically comparable within each image/workflow without a manual counter. A rerun keeps the same run number, so producer-side immutability guards continue to refuse overwrite of an already published coordinate.

`default.json` is the authoritative package list and applies the same regex ordering to direct GHCR names and canonical Harbor `private-ghcr` consumer names. Renovate maps date to `major/minor/patch` and the build number to the numeric `build` group.

These internal OCI releases are explicitly exempt from `minimumReleaseAge`. Homelab must be able to consume a newly produced internal release immediately after the producer's own build/test gates succeed. The exemption removes only the age wait; it does not weaken immutability, SHA/digest traceability, Harbor scanning or GitOps ownership.

Argo CD Image Updater remains the first-party deployment owner in Homelab; Renovate's parser exists for dependency interpretation and must not introduce a competing GitOps write path.

Mutable aliases such as `latest`, branch tags or SHA recovery tags are not the operational GitOps version source when an immutable release coordinate is available.

## Repository self-management

This repository intentionally does **not** use Renovate to manage its own dependencies. The absence of a top-level `renovate.json` is therefore expected, and Mend Renovate reporting that Renovate is disabled because no configuration file exists is not an actionable repository problem by itself.

GitHub Action references used by this repository are owned by Dependabot through `.github/dependabot.yml`. Introducing Renovate self-management requires a new explicit requirement or approved decision; the Mend notification alone is not sufficient justification.

## Ownership when Dependabot and Renovate coexist

Repository-specific manager ownership is declared in the consuming repository only when two updaters would otherwise overlap.

The standard split adopted during Wave `skunklabs-uk/developer-workspace#33` is:

- Dependabot may own GitHub Action references (`github-actions`, `depType: action`), which are committed as immutable SHAs with a readable version comment;
- when that split is used, Renovate is disabled **only** for `depType: action`, not for the whole `github-actions` manager;
- Renovate remains owner of `depType: uses-with`, including explicit tool versions such as `aquasecurity/trivy-action` `with.version`;
- the shared 7/14/30-day minimum release age remains the freshness gate for normal **external** upgrades managed by Renovate;
- proprietary OCI packages listed in the release-coordinate rule are the intentional exception and may be proposed immediately;
- do not duplicate these cooldowns in consumers or force a cross-repository upgrade merely to make all repositories show the same external version on the same day.

A repository may bypass the external cooldown only for a concrete, documented exception such as a verified security gate with an upstream fix. The standing internal-OCI exemption is defined centrally in `default.json` and does not need to be re-declared by consumers.
