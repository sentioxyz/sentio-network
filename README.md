# sentio-network

Configs and a Docker Compose setup to run a Sentio network node (op-node + op-reth + sentio-node (WIP) ).

Currently supported networks:

- `testnet` — L2 chain id `7892101`, settles to Sepolia (L1 chain id `11155111`)

## Layout

```
docker-compose.yml          # op-node + op-reth services
.env                        # host data dirs
.env.testnet                # testnet network env (selected in compose)
op-reth-entrypoint.sh       # op-reth launch script
networks/<name>/
  genesis.json              # L2 chain spec for op-reth
  rollup.json               # rollup config for op-node
```

## Run

```sh
docker compose up -d
```

Endpoints:
- JSON-RPC HTTP:  `http://localhost:8545`
- JSON-RPC WS:    `ws://localhost:8546`
- op-node RPC:    `http://localhost:9545`

Data is persisted under `./reth-data` and `./opnode-data` (paths configurable via `.env`).

On Apple Silicon, either use Docker Compose V2 (`docker compose`, with a space — picks the host arch automatically), or set `DOCKER_PLATFORM=linux/arm64` in `.env`.

## Switching / adding networks

Drop a new `networks/<name>/{genesis.json,rollup.json}` and create a matching `.env.<name>`, then update `env_file:` in `docker-compose.yml` to point at it.
