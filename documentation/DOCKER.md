My command in my juno directory is:
(download it from the https://github.com/cosmoscontracts/juno repo)

```markdown
STAKE_TOKEN=ujunox UNSAFE_CORS=true docker compose up
```

That command will start the local node. (Ensure to have docker installed and running)

If you want more more configuration you can do:
(The juno address is just for a testing user)
The 1317 is the rest port, the 26656 is the consensys port and the 26657 is the rpc port.

```markdown
docker run -it --name juno_1 -p 1317:1317 -p 26656:26656 -p 26657:26657 -e STAKE_TOKEN=ujunox -e UNSAFE_CORS=true ghcr.io/cosmoscontracts/juno:13.0.1 ./setup_and_run.sh juno16g2rahf5846rxzp3fwlswy08fz8ccuwk03k57y
```

To compile your wasm contract via docker:

# compile wasm - run this in your smart contract folder

```markdown
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")\_cache",target=/code/target \
 --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
 cosmwasm/rust-optimizer:0.12.12
```

# If you you use an ARM machine (Ex: Mac M1), you need to use the following

# This is experimental and should not be used for production use

```markdown
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")\_cache",target=/code/target \
 --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
 cosmwasm/rust-optimizer-arm64:0.12.12
```

Then copy the wasm to your docker container that is running the node:

# copy wasm to the container

```markdown
docker cp artifacts/your_compiled.wasm juno_node_1:/your_compiled.wasm
```

Then store the wasm:

# store wasm

```markdown
docker exec -i juno_node_1 \
 junod tx wasm store "/your_compiled.wasm" \
 --gas-prices 0.1ujunox --gas auto --gas-adjustment 1.3 \
 -y -b block --chain-id testing \
 --from validator --output json
```
