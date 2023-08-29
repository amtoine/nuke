# nuke
An explosive Nushell replacement for make and Makefiles

## example of a Nukefile
```nushell
const CLIPPY_OPTIONS = ["-D", "warnings"]

{
    all: ["check", "test", "install"]

    check: {
        cargo fmt --all -- --check
        cargo check --workspace --lib
        cargo check --workspace --tests
        cargo clippy --workspace -- $CLIPPY_OPTIONS
    }

    test: ["check", { cargo test --workspace }]

    fmt: { cargo fmt --all }

    doc: { cargo doc --document-private-items --no-deps --open }

    build: { cargo build --release }

    register: { nu --commands "register target/release/nu_plugin_explore" }

    install: ["build", "register"]

    clean: { cargo clean }
}
```

## introduce `nuke` to Nushell
```nushell
def "nu-complete list-rules" [] {
    let nuke = source Nukefile
    $nuke | columns
}

extern nuke [...rules: string@"nu-complete list-rules"]
```
