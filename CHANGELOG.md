# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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
