test
====

A test repo

## Attestation hello world

`make` writes the current UTC date to `out-file.txt`. A separate release workflow
runs on a GitHub-hosted Linux runner when a `v*` tag is pushed. The existing
sample CI workflow is unchanged.

### One-time setup

This example assumes a public GitHub repository with Actions enabled. In
[repository settings](https://github.com/selliott512/test/settings), under
**General > Releases**, enable **release immutability** before publishing.
The workflow checks this setting and stops if it is not enabled.

Immutable releases lock the published assets and tag. Use a new tag for each
experiment; the same published version cannot be replaced with another build.

### Try it

Commit these files and push them, then push a new version tag:

```sh
git add Makefile .gitignore README.md .github/workflows/release.yml
git commit -m "Add attestation hello world release"
git push origin master
git tag v0.1.0
git push origin v0.1.0
```

Use an unused tag if `v0.1.0` already exists. Watch the **Actions** tab. The
workflow runs `make`, attests its output, attaches it to a draft release, and
publishes the release. No Rust tools or custom secrets are needed.

### Download and verify both attestations

From a fresh directory, using a recent GitHub CLI:

```sh
gh release download v0.1.0 -R selliott512/test -p out-file.txt
cat out-file.txt

# Build provenance: this exact file was attested by the release workflow.
gh attestation verify out-file.txt -R selliott512/test --deny-self-hosted-runners

# Release identity: this file belongs to the immutable release and its tag.
gh release verify v0.1.0 -R selliott512/test
gh release verify-asset v0.1.0 out-file.txt -R selliott512/test
```

The build attestation uses `https://slsa.dev/provenance/v1`. GitHub automatically
generates the separate `https://in-toto.io/attestation/release/v0.2` release
attestation when the immutable release is published. Neither is embedded in
`out-file.txt`; both identify it by its hash. A local `make` generates a new
timestamp, which will not match the downloaded file's attestations.

If a workflow fails after creating its draft release, inspect that draft before
retrying; this minimal example does not overwrite existing releases.

See [build attestations](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations)
and [immutable releases](https://docs.github.com/en/code-security/concepts/supply-chain-security/immutable-releases).
