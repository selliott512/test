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

If a workflow fails after creating its draft release, inspect that draft before
retrying; this minimal example does not overwrite existing releases.

See [build attestations](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations)
and [immutable releases](https://docs.github.com/en/code-security/concepts/supply-chain-security/immutable-releases).

## Security

Releases use two signed attestations: release identity and build provenance.
The workflow builds on a GitHub-hosted runner, pins its actions to exact Git
commits, and attests the resulting file. Publishing an immutable release adds
GitHub's release attestation and locks its assets and associated tag.

### Verify a downloaded release

Use a recent GitHub CLI. For example, download the file from `v0.9.4` into a
fresh directory:

```sh
gh release download v0.9.4 -R selliott512/test -p out-file.txt
```

Use the same release tag in both commands below when checking another version.
Do not generate the file with a local `make`: its timestamp and hash will differ.

**Verify release identity:**

```sh
gh release verify-asset v0.9.4 out-file.txt -R selliott512/test
```

Success establishes that:

- The local file's hash matches an asset in the signed release attestation.
- The asset belongs to release `v0.9.4` in `selliott512/test`.
- The release attestation binds the release to an exact Git commit.
- The release is immutable: its published assets and associated tag are
  protected against replacement.

**Verify build provenance:**

```sh
gh attestation verify out-file.txt \
  -R selliott512/test \
  --deny-self-hosted-runners \
  --source-ref refs/tags/v0.9.4 \
  --signer-workflow selliott512/test/.github/workflows/release.yml
```

Success establishes that:

- The local file's hash matches a valid signed build-provenance attestation
  (`https://slsa.dev/provenance/v1`).
- The authenticated source repository is `selliott512/test`, with source
  reference `refs/tags/v0.9.4`.
- The signer is this repository's `.github/workflows/release.yml` workflow.
- The attestation was generated on a GitHub-hosted runner, not a self-hosted one.
- The signature and signing identity pass GitHub CLI's trust checks.

Neither attestation is embedded in the downloaded file. These checks establish
file integrity, release identity, and authenticated provenance; they do not
prove the file is harmless or independently prove faithful execution of the
build instructions. External tools and dependencies require their own scrutiny.

### Signed commits

Going forward, project commits will be GPG-signed by Steven Elliott
(`selliott512@gmail.com`) using the RSA-3072 signing key with fingerprint:

```text
90BB 5F79 7B0E D14D 310F 90BE 39B3 786A 18C9 3DAD
```

This policy does not assert that historical commits are signed. A valid commit
signature authenticates the commit to the signing key; it is separate from the
GitHub-issued artifact attestations above. With the public key imported and its
fingerprint independently confirmed, use `git verify-commit COMMIT` to check a
particular commit's signature.
