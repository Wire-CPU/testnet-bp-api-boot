# Containerized Node Definitions & Test Network Sync Bootstrap


## About This Project:

This collection of scripts launches a cluster that will sync to the Wire network testnet or mainet with multiple node configurations. There is also an automated boot sequence that utilizes a temporary "bios" node to launch a local chain, deploy system contracts and set a schedule with three active block producer nodes. 

This project can be useful as a development environment for system contracts or interact with the wire testnet.


### Version

- 0.1.0

## Installation
Basic knowledge about Docker, Docker Compose, and Shell Script is required.

### Before to start

Some things you need before getting started:

- [jq](https://stedolan.github.io/jq/download/)
- [make](https://en.wikipedia.org/wiki/Make_(software))
- [docker](https://www.docker.com/)

### First time run

1.  Clone this repo using
3.  Set the environment variables in the example .env
4.  Enter command `make run`

### Quick start

Once installed you can run `make run`, you can check the services running on:

- `api-node` at [http://localhost/v1/chain/get_info](http://localhost/v1/chain/get_info)


### File Structure

Within the download you'll find the following directories and files


```
.
├── Dockerfile ........................... Docker File
├── docker-compose.yml ................... Docker Compose
├── makefile ............................. Makefile
├── services ............................. Node configurations
│ ├── api-node ........................... API node (other node have same structure)
│ | ├── config.ini ....................... Nodeos Configuration File
│ | ├── genesis.json ..................... Genesis JSON (must be the same for other nodes)
│ | └── start.sh ......................... Script to start node with config params
│ ├── bios ............................... Temporary Bios Node Configuration
│ | ├── config.ini ....................... Nodeos Configuration File
│ | ├── genesis.json ..................... Genesis JSON (must be the same for other nodes)
│ | ├── start.sh ......................... Script to start node with config params
│ | └──  utils ........................... Boot process specific scripts
│ │   ├── bios.sh ........................ Boot sequence script
│ │   ├── schedule.json .................. Block Producer Schedule to propose
│ |   └── vault.sh ....................... Vault script for secure key storage
│ ├── vault .............................. Vault Service
│ | └── config ........................... Valut Configuration
│ │   └── vault.json ..................... Valut Configuration
│ └── wallet ............................. KEOSD Wallet Service
│   └──  start.sh ........................ Wallet Start Script
├── kubernetes/ .......................... Kubernetes Definitions Dir
├── mkutils/ ............................. Make Utils
└── README.md ............................ This File
```
### Features

#### Vault

After the pod for the vault has been initiated, make sure you do:

- Make a port-forward to the vault.
- Make sure you set an environment variable called VAULT_URL with the url from the port-forward.
- `source services/bios/utils/vault.sh` and call the `init` function passing the path where you want to to store the keys file `init vault_keys.json` which should not only create the keys but unseal the vault and leave it ready to store and serve secrets

