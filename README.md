# Foundry Actions

![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)
![Continuous integration](https://github.com/abigger87/foundry-action/workflows/Continuous%20integration/badge.svg)
![Dependabot enabled](https://api.dependabot.com/badges/status?host=github&repo=abigger87/foundry-action)

This GitHub Action runs specified [`foundry`](https://github.com/gakonst/foundry)
commands.

**Table of Contents**

- [Foundry Actions](#foundry-actions)
  - [Example workflow](#example-workflow)
  - [Inputs](#inputs)
  - [Toolchain](#toolchain)
  - [Cross-compilation](#cross-compilation)
  - [License](#license)
  - [Contribute and support](#contribute-and-support)

## Example workflow

```yaml
on: [push]

name: CI

jobs:
  build_and_test:
    name: Rust project
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
      - uses: abigger87/foundry-action@v1
        with:
          command: build
          args: --release --all-features
```

## Inputs

| Name        | Required | Description                                                              | Type   | Default |
| ------------| :------: | -------------------------------------------------------------------------| ------ | --------|
| `command`   | ✓        | Cargo command to run, ex. `check` or `build`                             | string |         |
| `toolchain` |          | Rust toolchain name to use                                               | string |         |
| `args`      |          | Arguments for the cargo command                                          | string |         |     
| `use-cross` |          | Use [`cross`](https://github.com/rust-embedded/cross) instead of `cargo` | bool   | false   |

## Toolchain

By default this Action will call whatever `cargo` binary is available
in the current [virtual environment](https://help.github.com/en/articles/software-in-virtual-environments-for-github-actions).

You can use [`actions-rs/toolchain`](https://github.com/actions-rs/toolchain)
to install specific Rust toolchain with `cargo` included.

## Cross-compilation

In order to make cross-compile an easy process,
this Action can install [cross](https://github.com/rust-embedded/cross)
tool on demand if `use-cross` input is enabled; `cross` executable will be invoked
then instead of `cargo` automatically.

All consequent calls of this Action in the same job
with `use-cross: true` input enabled will use the same `cross` installed.

```yaml
on: [push]

name: ARMv7 build

jobs:
  linux_arm7:
    name: Linux ARMv7
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          target: armv7-unknown-linux-gnueabihf
          override: true
      - uses: actions-rs/cargo@v1
        with:
          use-cross: true
          command: build
          args: --target armv7-unknown-linux-gnueabihf
```

## License

This Action is distributed under the terms of the MIT license, see [LICENSE](https://github.com/actions-rs/toolchain/blob/master/LICENSE) for details.

## Contribute and support

Any contributions are welcomed!

If you want to report a bug or have a feature request,
check the [Contributing guide](https://github.com/actions-rs/.github/blob/master/CONTRIBUTING.md).

You can also support author by funding the ongoing project work,
see [Sponsoring](https://actions-rs.github.io/#sponsoring).
