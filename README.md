# minimal-rust-boilerplate

Minimal boilerplate for Rust projects with a reproducible [Devbox](https://www.jetify.com/devbox) development environment.

---

## Prerequisites

- [Devbox](https://www.jetify.com/devbox) - Reproducible development environments powered by Nix.
- [direnv](https://direnv.net/) (optional) - Automatically activate the environment when entering the project directory.

## Setup

With [direnv configured](#automatic-activation-with-direnv), entering the project directory automatically activates the Devbox environment. Run Cargo commands directly; `devbox shell` is unnecessary.

Without direnv, start the development environment manually:

```sh
devbox shell
```

Devbox provides `rustup` via `rustup@latest`, with its resolved package recorded in `devbox.lock`. Rust itself is managed by `rustup`: `rust-toolchain.toml` pins **Rust 1.90.0** with the `minimal` profile plus `rustfmt` and `clippy`. The package uses Rust edition **2024**.

### Initialization hook

The `shell.init_hook` in `devbox.json` runs when Devbox initializes the environment, including before running its scripts:

```sh
rustup default stable
```

This installs the stable toolchain if needed and sets it as rustup's default. It changes the default for the active rustup installation, so it can affect other projects that have no toolchain override. Inside this project, `rust-toolchain.toml` takes precedence: Cargo uses Rust **1.90.0**, and rustup installs that toolchain and its configured components when needed. The hook does not update the project's pinned version.

Run the project:

```sh
cargo run
```

### Automatic activation with direnv

Install direnv using your system package manager, then [hook it into your shell](https://direnv.net/docs/hook.html). For Zsh, add this line near the end of `~/.zshrc`:

```sh
eval "$(direnv hook zsh)"
```

For Bash, add `eval "$(direnv hook bash)"` near the end of `~/.bashrc` instead. Restart your shell after adding the hook.

This project already includes a Devbox-generated `.envrc`. From the project directory, authorize it:

```sh
direnv allow
```

direnv loads the Devbox environment when you enter the directory and unloads its environment variables when you leave. You can run Cargo commands directly once it is loaded. The init hook still runs during environment initialization; its change to rustup's default persists after you leave.

If `.envrc` is missing, generate it with:

```sh
devbox generate direnv
```

Run `direnv allow` again if direnv reports that configuration changes need authorization. See the [Devbox direnv guide](https://www.jetify.com/docs/devbox/ide-configuration/direnv/) for details.

## Stack

- [Rust](https://www.rust-lang.org/) - Programming language and toolchain.
- [Cargo](https://doc.rust-lang.org/cargo/) - Package manager and build system.
- [rustup](https://rustup.rs/) - Rust toolchain manager.
- [Devbox](https://www.jetify.com/devbox) - Reproducible development environment.

---

## Commands

Run the Cargo commands below inside `devbox shell` or with direnv active. The scripts defined in `devbox.json` can also be run from outside the environment:

| Task | Devbox script | Cargo command |
| --- | --- | --- |
| Build | `devbox run build` | `cargo build` |
| Test | `devbox run test` | `cargo test` |
| Lint | `devbox run lint` | `cargo clippy --all-targets --all-features -- -D warnings` |
| Check formatting | `devbox run fmt` | `cargo fmt --check` |

The `fmt` script checks formatting; use `cargo fmt` to apply formatting changes.

### Development

```sh
cargo run
```

### Check

```sh
cargo check
```

### Test

```sh
cargo test
```

### Lint

```sh
cargo clippy --all-targets --all-features -- -D warnings
```

### Format

```sh
cargo fmt
```

Check formatting without modifying files:

```sh
cargo fmt --check
```
