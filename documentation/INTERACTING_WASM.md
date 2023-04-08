If you want to query a contract you can (ensure the contract has the queries that match - we are using the cosmwasm-for-dummies example by Callum):

```markdown
$BINARY q wasm contract-state smart $CONTRACT_ADDRESS '{"get_count":{}}'
```

which returns:

```markdown
data:
count: 42
```

If we do:

```markdown
$BINARY q wasm contract-state smart $CONTRACT_ADDRESS '{"get_count":{}}' --output json
```

we get:

```markdown
{"data":{"count":42}}
```

Now let's change the state of the contract (increment):

```markdown
$BINARY tx wasm execute $CONTRACT_ADDRESS '{"increment":{}}' --from test-user $TXFLAG
```

Now let's reset count to 0

```markdown
$BINARY tx wasm execute $CONTRACT_ADDRESS '{"reset":{"count": 0}}' --from test-user $TXFLAG
```

You can also list contracts by id

```markdown
$BINARY q wasm list-contract-by-code 5
```

Contract addresses are larger than user addresses (32 vs 20)

If you want the latest contract you can run:

```markdown
CONTRACT_ADDRESS=$($BINARY q wasm list-contract-by-code $CONTRACT_CODE --output json | jq -r '.contracts[-1]')
```

Then you can do:

```markdown
echo $CONTRACT_ADDRESS
```
