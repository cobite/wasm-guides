Here we will perform two different transactions.

1. store contract blueprint
2. instantiate an instance of the contract

We will need more gas to do this. So we will updated our TXFLAG environment variable in our terminal by doing:

```markdown
TXFLAG="--gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 -y -b block --chain-id testing --node http://localhost:26657/"
```

We can now echo it out to confirm it was updated:

```markdown
echo $TXFLAG
```

We will also create a BINARY environment variable so avoid having to write the same long command to move into our docker juno node container:

```markdown
BINARY="docker exec -i juno_1 junod"
```

We will echo this out to confirm it worked:

```markdown
echo $BINARY
```

Now because we will be working inside the docker container, we won't be working with the local juno node like we used to do with 'junod status'. Now all we need to do is:

```markdown
$BINARY status
```

## if need to remove a docker container we can do: docker rm hash

If you want to use bash instead of zshrc, and you might need to to exex into a docker for some reason:

In your local terminal just type: bash
This will change your terminal to a bash terminal.
Then you have to import all the env commands again.

Now we can run the status command here:

```markdown
$BINARY status
```

when you compile a contract, the size limit for the chain is (was) 750kb

You can use the optimizer tooling to make contract smaller.

The optimizer-workspace is a different tool that is also good for a workspace of multiple contracts

So now change directory to your contract folder.

Ensure the version is the right one (latest). You can find this by looking at end of the long command. You find the latest version on https://github.com/cosmwasm/rust-optimizer

for intel:

````markdown
docker run --rm -v "$(pwd)":/code --mount type=volume,source="$(basename "$(pwd)")\_cache",target=/code/target --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry cosmwasm/rust-optimizer:0.12.12

for arm64:

```markdown
docker run --rm -v "$(pwd)":/code --mount type=volume,source="$(basename "$(pwd)")\_cache",target=/code/target --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry cosmwasm/rust-optimizer-arm64:0.12.12
```
````

The result will be placed in a artifacts folder in the root of the contract directory.

Now we need to pull that file into the docker container so we can interact with it:

```markdown
docker cp artifacts/cw_example-aarch64.wasm juno_1:/cw_example-aarch64.wasm
```

Now to get inside the docker container:

```markdown
docker exec -i juno_1 /bin/sh
```

then you can list the files in the directory by using:

```markdown
ls /
```

Now you should see the compiled wasm file.
you can type 'exit' to get out of the docker container.

At this stage you should install this lightweight and flexible command-line JSON processor (This is if you have brew installed, otherwise use a different install method relevant to you):

```markdown
brew install jq
```

This command will store the contract on the chain, then store the return which is the contract id back to the contract code variable.

```markdown
CONTRACT_CODE=$($BINARY tx wasm store "/dummies_example-aarch64.wasm" --from validator $TXFLAG --output json | jq -r '.logs[0].events[] | select(.type == "store_code").attributes[] | select(.key == "code_id").value')
```

Then you can do:

```markdown
echo $CONTRACT_CODE
```

you can now do stuff with the code that's runnning on-chain.

```markdown
$BINARY q wasm -h
```

The Juno node should come provisioned with a test user (DON'T USE THIS ACCOUNT FOR REAL FUNDS!).
The test user address is: juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y
The test user seed is: clip hire initial neck maid actor venue client foam budget lock catalog sweet steak waste crater broccoli pipe steak sister coyote moment obvious choose

If not, you can create it manually like this:

```markdown
echo "clip hire initial neck maid actor venue client foam budget lock catalog sweet steak waste crater broccoli pipe steak sister coyote moment obvious choose" | $BINARY keys add test-user --recover --keyring-backend test
```

then you can do the following to show all accounts:

```markdown
$BINARY keys list
```

```markdown
CONTRACT_CODE=$($BINARY tx wasm store "/cw_example-aarch64.wasm" --from validator $TXFLAG --output json | jq -r '.logs[0].events[] | select(.type == "store_code").attributes[] | select(.key == "code_id").value')
```

Here is a useful resource to make a bash script that sets all the env variables. This would enable you to just run the bash script in your terminal and not need to copy/paste enviornment files each time.
https://github.com/CosmosContracts/tokenfactory-contracts/blob/main/e2e/core/test_e2e.sh

when you are in your contract folder and run: 'cargo schema', it generates the schema for the contract that lets you know what rpc calls you can make.

These can be found at:
contract-folder/schema/cw-example.json

If you want to instantiate a contract with an initial value you can set the value in the bash terminal"

```markdown
INIT='{"count": 42}'
```

Then to instanstiate the blueprint contract you can do:

```markdown
$BINARY tx wasm instantiate $CONTRACT_CODE "$INIT" --from "test-user" --label "my first contract" $TXFLAG --no-admin
```

If you want to add an admin

```markdown
$BINARY tx wasm instantiate $CONTRACT_CODE "$INIT" --from "test-user" --label "my first contract" $TXFLAG --admin your-juno-address
```

You should get a response on the terminal like this:

```markdown
gas estimate: 205042
code: 0
codespace: ""
data: data-here
events: events-here
raw-log: raw-log-here
timestamp: timestamp-here
tx: null
txhash: tx-hash-here
```
