# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.15.1] - 2025-09-23

* Update `Swatinem/rust-cache` to v2.8.1

## [1.15.0] - 2025-09-14

* Add support for non-root source directory.
    Accept source code and `rust-toolchain.toml` file in subdirectories of the repository.
    Adds a new parameter `rust-src-dir` that controls the lookup for toolchain files and sets a default value for the `cache-workspace` input. (#69 by @Kubaryt)

## [1.14.1] - 2025-08-28

* Pin `Swatinem/rust-cache` action to a full commit SHA (#68 by @JohnTitor)

## [1.14.0] - 2025-08-23

* Add new parameters `cache-all-crates` and `cache-workspace-crates` that are propagated to `Swatinem/rust-cache` as `cache-all-crates` and `cache-workspace-crates`

## [1.13.0] - 2025-06-16

* Add new parameter `cache-provider` that is propagated to `Swatinem/rust-cache` as `cache-provider` (#65 by @mindrunner)

## [1.12.0] - 2025-04-23

* Add support for installing rustup on Windows (#58 by @maennchen)
    This adds support for using Rust on the GitHub provided Windows ARM runners.

## [1.11.0] - 2025-02-24

* Add new parameter `cache-bin` that is propagated to `Swatinem/rust-cache` as `cache-bin` (#51 by @enkhjile)
* Add new parameter `cache-shared-key` that is propagated to `Swatinem/rust-cache` as `shared-key` (#52 by @skanehira)

## [1.10.1] - 2024-10-01

* Fix problem matcher for rustfmt output.
    The format has changed since https://github.com/rust-lang/rustfmt/pull/5971 and now follows the form "filename:line".
    Thanks to @0xcypher02 for pointing out the problem.

## [1.10.0] - 2024-09-23

* Add new parameter `cache-directories` that is propagated to `Swatinem/rust-cache` (#44 by @pranc1ngpegasus)
* Add new parameter `cache-key` that is propagated to `Swatinem/rust-cache` as `key` (#41 by @iainlane)
* Make rustup toolchain installation more robust in light of planned changes https://github.com/rust-lang/rustup/issues/3635 and https://github.com/rust-lang/rustup/pull/3985
* Allow installing multiple Rust toolchains by specifying multiple versions in the `toolchain` input parameter.
* Configure the `rustup override` behavior via the new `override` input. (#38)

## [1.9.0] - 2024-06-08

* Add extra argument `cache-on-failure` and forward it to `Swatinem/rust-cache`. (#39 by @samuelhnrq)  
    Set the default the value to true.
    This will result in more caching than previously.
    This helps when large dependencies are compiled only for testing to fail.

## [1.8.0] - 2024-01-13

* Allow specifying subdirectories for cache.
* Fix toolchain file overriding.

## [1.7.0] - 2024-01-11

* Allow overriding the toolchain file with explicit `toolchain` input. (#26)

## [1.6.0] - 2023-12-04

### Added

* Allow disabling problem matchers (#27)
    This can be useful when having a matrix of jobs, that produce the same errors.

## [1.5.0] - 2023-05-29

### Added

* Support installing additional components and targets that are not listed in `rust-toolchain` (#14)
    Before only the items listed in `rust-toolchain` were installed.
    Now all the items from the toolchain file are installed and then all the `target`s and `components` that are provided as action inputs.
    This allows installing extra tools only for CI or simplify testing special targets in CI.
* Allow skipping the creation of a `RUSTFLAGS` environment variable.
    Cargos logic for rustflags is complicated, and setting the `RUSTFLAGS` environment variable prevents other ways of working.
    Provide a new `rustflags` input, which controls the environment variable creation.
    If the value is set to the empty string, then `RUSTFLAGS` is not created.

    Pre-existing `RUSTFLAGS` variables are never modified by this extension.

## [1.4.4] - 2023-03-18

### Fixed

* Use color aware problem matcher.
    The problem matcher currently runs against the colored terminal output ([Bug 1](https://github.com/actions/runner/issues/2341), [Bug 2](https://github.com/actions/runner/pull/2430)).
    The previous matcher was not aware of ANSII color codes and did not work.

## [1.4.3] - 2023-02-21

### Fixed

* Executing the action twice for different toolchains now no longer fails around unstable features #12.
    If multiple toolchains are installed, the "CARGO_REGISTRIES_CRATES_IO_PROTOCOL" can be downgraded to "git" if any of the installed toolchains require it.

## [1.4.2] - 2023-02-15

### Fixed

* Tweak sparse registry version regex to better work with 1.68 nightly versions.
* Fix command not found issue

## [1.4.1] - 2023-02-13

### Fixed

* Fixed running on macOS #9 #10
    The macOS images have an ancient version of bash, but the action relies on "newer" features than 2014.
    We install bash via brew (already pre-installed) to have a new enough version.

    The CI is extended to also run on Windows and macOS to catch such issues earlier in the future.

    Thanks to @GeorgeHahn for reporting the issue.

## [1.4.0] - 2023-02-13

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
