## NOTE: This repo serves as an example of how to set up a supernode and actual configurations may vary depending on your own supernode setup. This repo only exists to show the absolute bare minimum of running a supernode. There are also other ways of setting up a supernode which are not explored here

## Supernode Classification
1. Ela mainchain node
2. DID sidechain node
3. Token sidechain node
4. Elastos Carrier Bootstrap node

**Minimum Requirements**
- System: `Ubuntu 14.04 LTS 64-bit`
- CPU: `2 cores or more`
- Memory: `4GB or more`
- Hard drive: `40GB or more`
- Network: `Standard network with accessible public IP or domain name`
- DposPort[default: 20339]: `The firewall needs to open this port to the entire network`
- System permissions and dependencies: `To be added`
- python3 environment (Optional: Used to deploy the node monitoring and alarm routine)

## Setup Nodes from scratch(the most customizable solution)

### Ela mainchain node
- Read [Guide to setting up ELA mainchain node](./ubuntu/setup-ela-node.md) for more info

### DID sidechain node
- Read [Guide to setting up DID sidechain node](./ubuntu/setup-did-sidechain-node.md) for more info

### Token sidechain node
- Read [Guide to setting up Token sidechain node](./ubuntu/setup-token-sidechain-node.md) for more info

### Elastos Carrier node
- Read [Guide to setting up Elastos Carrier node](./ubuntu/setup-elastos-carrier-node.md) for more info

## Setup Nodes using docker(the easiest solution)

### Setup docker environment
- Read [Guide to setting up docker environment](./docker/setup-docker-environment.md) for more info

### Run docker-compose up
```bash
cd docker
docker-compose up --remove-orphans --build --force-recreate -d
```

### Things to note about docker containers
- All your container data for ela, did and token are saved under docker/.volumes/supernode-setup. If you would like to change this, be sure to modify your docker/docker-compose.yml appropriately
- You can check the logs of the running container by doing something like:
    ```
    docker container logs -f mainchain-node
    ```