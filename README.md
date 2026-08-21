# blake2b-py

Blake2b hashing in Rust with Python bindings.

## Developing

Install dependencies and build the Python extension in a virtual environment:

```bash
UV_CACHE_DIR=/tmp/uv-cache uv run --group test maturin develop
```

Run the test suites:

```bash
UV_CACHE_DIR=/tmp/uv-cache uv run --group test pytest
PYO3_PYTHON=.venv/bin/python cargo test test_
```

The slow EIP-152 vector can be run explicitly:

```bash
PYO3_PYTHON=.venv/bin/python cargo test --release test_f_eip_152_vec_8 -- --ignored --nocapture
```

Rust benchmarks use unstable Rust benchmark support and require nightly:

```bash
PYO3_PYTHON=.venv/bin/python cargo +nightly bench --features nightly-benches
```

You may need to specify the `MACOSX_DEPLOYMENT_TARGET` environment variable to
your version of macOS.

## Building and Releasing

Build local artifacts with maturin:

```bash
UV_CACHE_DIR=/tmp/uv-cache uv run --group build maturin build --release
```

The committed package version in `Cargo.toml` is intentionally the invalid
release sentinel `0.0.0`. Create a GitHub release with a semantic-version tag
such as `v0.5.0`; the publish workflow validates the tag and replaces the
sentinel before Maturin builds the wheels and source distribution. A local
build retains the sentinel version and must not be published.

PyPI must be configured to trust the `ApeWorX/blake2b-py` GitHub repository and
the `.github/workflows/publish.yaml` workflow before the release job can
publish.
