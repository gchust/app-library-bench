# Fork maintenance

This fork uses a two-branch maintenance model:

- `main` mirrors `Albert-mah/app-library-bench/main`.
- `custom/main` is the default branch and the place for fork-specific changes.
- Feature work should branch from `custom/main`, then merge back through a pull request.

The mirror branch stays clean so upstream changes can be reviewed separately from fork
changes. Do not commit fork-only work directly to `main`.

## Upstream sync

`.github/workflows/upstream-release-sync.yml` runs on a schedule and can also be started
manually. It fetches upstream `main`, fast-forwards the fork's `main` mirror branch, mirrors
the latest upstream release tag if one exists, and opens or updates a pull request from
`main` into `custom/main`.

The workflow attempts to enable auto-merge for the sync PR. If GitHub permissions or branch
rules block auto-merge, the PR remains open for manual review.

## Fork releases

`.github/workflows/release-on-custom-merge.yml` runs after a pull request is merged into
`custom/main`. It rebuilds from the merged commit, creates the next fork tag, and publishes
a GitHub release.

Fork release tags use this pattern:

```text
vX.Y.Z-fork.N
```

If upstream publishes GitHub releases, `vX.Y.Z` comes from the latest upstream release tag.
This upstream currently has no GitHub releases, so the workflow falls back to the root
`package.json` version until an upstream release exists.

## Build targets

The default build target is the macOS GitHub Actions runner. This repository does not have
Android project files or upstream Android release assets, so Android is not built by default.
