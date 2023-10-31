# Install Rust Toolchain

This GitHub Action installs a Rust toolchain using rustup.
It further integrates into the ecosystem.
Caching for Rust tools and build artifacts is enabled.
Environment variables are set to optimize the cache hits.
[Problem Matchers] are provided for build messages (cargo, clippy) and formatting (rustfmt).

The action is heavily inspired by *dtolnay*'s <https://github.com/dtolnay/rust-toolchain> and extends it with further features.

## Example workflow

```yaml
name: "Test Suite"
on:
  push:
  pull_request:

jobs:
  test:
    name: cargo test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions-rust-lang/setup-rust-toolchain@v1
      - run: cargo test --all-features

  # Check formatting with rustfmt
  formatting:
    name: cargo fmt
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      # Ensure rustfmt is installed and setup problem matcher
      - uses: actions-rust-lang/setup-rust-toolchain@v1
        with:
          components: rustfmt
      - name: Rustfmt Check
        uses: actions-rust-lang/rustfmt@v1
```

## Inputs

All inputs are optional.
If a [toolchain file](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file) (i.e., `rust-toolchain` or `rust-toolchain.toml`) is found in the root of the repository and no `toolchain` value is provided, all items specified in the toolchain file will be installed.
If a `toolchain` value is provided, the toolchain file will be ignored.
If no `toolchain` value or toolchain file is present, it will default to `stable`.
First, all items specified in the toolchain file are installed.
Afterward, the `components` and `target` specified via inputs are installed in addition to the items from the toolchain file.

| Name         | Description                                                                            | Default       |
| ------------ | -------------------------------------------------------------------------------------- | ------------- |
| `toolchain`  | Rustup toolchain specifier e.g. `stable`, `nightly`, `1.42.0`.                         | stable        |
| `target`     | Additional target support to install e.g. `wasm32-unknown-unknown`                     |               |
| `components` | Comma-separated string of additional components to install e.g. `clippy, rustfmt`      |               |
| `cache`      | Automatically configure Rust cache (using `Swatinem/rust-cache`)                       | true          |
| `rustflags`  | Set the value of `RUSTFLAGS` (set to empty string to avoid overwriting existing flags) | "-D warnings" |

### RUSTFLAGS

By default, this action sets the `RUSTFLAGS` environment variable to `-D warnings`.
However, rustflags sources are mutually exclusive, so setting this environment variable omits any configuration through `target.*.rustflags` or `build.rustflags`.

* If `RUSTFLAGS` is already set, no modifications of the variable are made and the original value remains.
* If `RUSTFLAGS` is unset and the `rustflags` input is empty (i.e., the empty string), then it will remain unset.
    Use this, if you want to prevent the value from being set because you make use of `target.*.rustflags` or `build.rustflags`.
* Otherwise, the environment variable `RUSTFLAGS` is set to the content of `rustflags`.

To prevent this from happening, set the `rustflags` input to an empty string, which will
prevent the action from setting `RUSTFLAGS` at all, keeping any existing preferences.

You can read more rustflags, and their load order, in the [Cargo reference].

## Outputs

| Name             | Description                               |
| ---------------- | ----------------------------------------- |
| `rustc-version`  | Version as reported by `rustc --version`  |
| `cargo-version`  | Version as reported by `cargo --version`  |
| `rustup-version` | Version as reported by `rustup --version` |

## License

The scripts and documentation in this project are released under the [MIT
License].

[MIT License]: LICENSE
[Problem Matchers]: https://github.com/actions/toolkit/blob/main/docs/problem-matchers.md
[Cargo reference]: https://doc.rust-lang.org/cargo/reference/config.html?highlight=unknown#buildrustflags
