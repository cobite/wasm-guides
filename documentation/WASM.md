When installing a new project you can use this as your starting point:
https://github.com/InterWasm/cw-template

```markdown
cargo generate --git http://github.com/InterWasm/cw-template.git --name cw_example
```

If you don't want to have any extra starting code you can add this flag:

```markdown
cargo generate --git https://github.com/CosmWasm/cw-template.git --name cw-example --branch 1.0-minimal
```

Now we build it:

```markdown
cargo build
```

This generates all the JSON schema files for the project.

```markdown
cargo schema

cargo clippy --all-targets -- --D warnings
```

In the examples folder you will see a schema.rs file. This will store the schema for your json messages.

In the .cargo folder, you will see what commands you can use with 'cargo' to do things. Like 'cargo wasm'

Inside the src folder:

#state.rs
This for the state that will live on-chain

#msg.rs
This is for the way the user will interact with the contract

#contract.rs
This is for the logic in the contract
