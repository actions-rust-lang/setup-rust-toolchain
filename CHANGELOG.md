# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

* Only set environment variables, if they are not set before.
    This allows setting environment variables before using this action and keeping their values.
* Enable stable sparse registry, except on versions 1.66 and 1.67 where this leads to errors.

## [1.3.7] - 2023-01-31

### Fixed

* Disable the stable access to the sparse registry.
    Setting the value causes problem on version before stabilization, e.g., 1.67.
    For example, "cargo add" fails.

## [1.3.6] - 2023-01-31

### Fixed

* The the correct environment variable to enable the sparse registry access.
    The pull request originally had the wrong value, without `CARGO_` prefix.

## [1.3.5] - 2023-01-21

### Changed

* Use the newly stabilized setting to enable sparse registry access.
    This speeds up access to the crate registry and is in addition to the unstable nightly env var.
    <https://github.com/rust-lang/cargo/pull/11224>

## [1.3.4] - 2022-10-15

### Changed

* The last version did not fix all "set-output" commands

## [1.3.3] - 2022-10-13

### Changed

* Switch from set-output to $GITHUB_OUTPUT to avoid warning
    https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/

## [1.3.2] - 2022-09-15

### Fixed

* Fix setting `$CARGO_HOME` to a valid path, in case rustup is installed from the internet.
    Thanks to @nahsi for providing the fix.

## [1.3.1] - 2022-08-14

### Changed

* Use the sparse-registry on nightly for faster access to the crate registry on nightly.
    <https://internals.rust-lang.org/t/call-for-testing-cargo-sparse-registry/16862>

## [1.3.0] - 2022-07-30

### Added

* An option to disable configuring Rust cache.
    Thanks to @filips123 for the PR.

## [1.2.1] - 2022-07-29

### Fixed

* Set environment variables before invoking the cache action.
    This ensures restoring and saving are using the same cache key.

## [1.2.0] - 2022-07-21

### Added

* Prefer toolchain definitions in `rust-toolchain` or `rust-toolchain.toml` files ([Toolchain File](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file)).
    Other input values are ignored if either file is found.

## [1.1.0] - 2022-07-19

### Added

* Install rustup if not available in the CI environment. (Linux only)
    The code is taken from this issue: <https://github.com/dtolnay/rust-toolchain/pull/8>
* Add rustc version output suitable as a cache key.
    This is based on <https://github.com/dtolnay/rust-toolchain/pull/20> and <https://github.com/dtolnay/rust-toolchain/pull/17>.

### Changed

* Update to `Swatinem/rust-cache@v2`.

## [1.0.2] - 2022-05-02

### Changed

* Enable colored cargo output.
* Print short backtraces during test failure.

## [1.0.1] - 2022-04-20

### Added

* Release action on marketplace

## [1.0.0] - 2022-04-20

Initial Version
