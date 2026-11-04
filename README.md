# stalwart-build

**Unofficial** build of the [Stalwart](https://github.com/stalwartlabs/stalwart)
mail server for `aarch64-unknown-linux-gnu`, compiled with the SQLite store
only:

```
cargo build --release --locked -p stalwart --no-default-features --features sqlite
```

No RocksDB, PostgreSQL, MySQL, S3, Redis, Azure, NATS or enterprise code.
The upstream source is used unmodified, at the release tag. This repository
only holds the GitHub Actions workflow; it is not affiliated with or endorsed
by Stalwart Labs.

## Releases

Each release contains:

- `stalwart-<version>-aarch64-sqlite.tar.gz`: the binary
- `stalwart-<version>-source.tar.gz`: the exact corresponding source (AGPL-3.0)
- `SHA256SUMS`

The release notes record the upstream commit, the Rust version and the
workflow run. The binary has a build provenance attestation:

```
gh attestation verify stalwart-<version>-aarch64-sqlite.tar.gz --repo nicheosala/stalwart-build
```

## Building a new version

`check.yml` runs every 5 minutes and dispatches `build.yml` when upstream's
latest release has no matching release here. It deletes its own successful
runs, so only failures stay in the Actions tab.

To build by hand: Actions → Build → Run workflow, with the upstream version
(e.g. `1.0.0`), or:

```
gh workflow run build.yml -R nicheosala/stalwart-build -f version=1.0.0
```

If upstream renames the `sqlite` feature, the build fails: check
`crates/main/Cargo.toml`.

## License

Stalwart is licensed under AGPL-3.0-only; the binaries published here are
under the same license.
