## DISCLAIMERS:
1. All elected Supernodes must abide by the rules and standards outlined in the Elastos DPoS Supernode Election Process. As Elastos is a decentralized and distributed network system, and DPoS Supernodes are selected through an election via the network’s official wallet, Elastos Wallet, the Elastos Foundation is not responsible for the actions taken or views expressed by any elected Supernodes (operators or owners), nor is it responsible for setting up nodes or super nodes in any way, including but not limited to arranging mining pools, guaranteeing uninterrupted services, or compensating party losses that may arise throughout the staking process.
2. This script serves as a simplified demo for Supernode configuration. Actual configurations may vary for Supernodes being set up in various operating environments.

Elastos Foundation does not take responsibility in the actual setting up of supernodes

## Supernode Classification
1. Ela mainchain node
2. DID sidechain node
3. Token sidechain node
4. Elastos Carrier Bootstrap node

**Minimum Requirements**
- System: `Ubuntu 18.04 LTS 64-bit`
- CPU: `2 cores or more`
- Memory: `4GB or more`
- Hard drive: `40GB or more`
- Network: `Standard network with accessible public IP or domain name`
- DposPort[default: 20339]: `The firewall needs to open this port to the entire network`
- System permissions: `Need sudo(root) privileges`

## Setup Nodes using automated script

### Installation
- Download the compressed package for the node
    ```
    wget https://download.elastos.org/supernode/elastos-supernode-20190616-alpha.tgz
    ```

- Verify the compressed package for the node is correct
    ```
    shasum -c elastos-supernode-20190609-alpha.tgz.digest
    ```

    Which should output
    ```
    elastos-supernode-20190609-alpha.tgz: OK
    ```

- Unzip the compressed package for the node
    ```
    tar xf elastos-supernode-20190609-alpha.tgz
    ```

    After unzipping, you will get the directory structure that is consistent with the node/readme.txt file

- Verify that the contents in the compressed package are correct
    ```
    cd node;
    shasum -c checksum.txt
    ```

    Which should output
    ```
    node.sh: OK

    ela/ela: OK

    did/did: OK

    token/token: OK

    carrier/ela-bootstrapd: OK
    ```

### Run the node
node/node.sh is the main script for running and maintaining nodes. You can use this script to complete node initialization, startup, shutdown, and so on

- Configuration
    ```
    node/node.sh init
    ```

    Enter the password used to create the keystore.dat file, and then node/node.sh will automatically get the server’s public IP and use this IP to complete the configuration file modification.

    Input the public key of your node on your Elastos Mobile Wallet by going to "Election Registration" page > "Manage Election" > "Update Information"

    You should see something like 
    ```
    Done

    Updating /node/ela/config.json...

    Done

    Updating /node/carrier/bootstrapd.conf...

    Done
    ```

- Start the node
    ```
    node/node.sh start
    ```

    Output below indicates the node is started normally:
    ```
    Starting ela...

    ela: Running, 29709

    Starting did...

    did: Running, 29723

    Starting token...

    token: Running, 29736

    Starting carrier...

    Elastos bootstrap daemon, version 5.2(20190604)

    carrier: Running, 2493, 2495
    ```

- Shut down the node
    Shut down the ELA, DID, Token and Carrier nodes:
    ```
    node/node.sh stop
    ```   

    If the node is normally closed, the following output will be displayed:
    ```
    Stopping ela...

    ela: Stopped

    Stopping did...

    did: Stopped

    Stopping token...

    token: Stopped

    Stopping carrier...

    carrier: Stopped
    ``` 

- Status
    Check node status:
    ```
    node/node.sh status
    ```

    If the following output is displayed, the node is running normally:
    ```
    ela: Running, 29709

    did: Running, 29723

    token: Running, 29736

    carrier: Running, 2493, 2495
    ```

    If the output is as follos, the node is closed:
    ```
    ela: Stopped

    did: Stopped

    token: Stopped

    carrier: Stopped
    ```

## Setup Nodes from scratch

### Ela mainchain node
- Read [Guide to setting up ELA mainchain node](./ubuntu/setup-ela-node.md) for more info

### DID sidechain node
- Read [Guide to setting up DID sidechain node](./ubuntu/setup-did-sidechain-node.md) for more info

### Token sidechain node
- Read [Guide to setting up Token sidechain node](./ubuntu/setup-token-sidechain-node.md) for more info

### Elastos Carrier node
- Read [Guide to setting up Elastos Carrier node](./ubuntu/setup-elastos-carrier-node.md) for more info

## Setup Nodes using docker

### Setup docker environment
NOTE: Please keep in mind that the docker images are not made by Elastos Foundation but through the CR community and as such, Elastos Foundation does not take responsibility in the operation of these docker images available on Cyber Republic repository at [https://cloud.docker.com/u/cyberrepublic/repository/list](https://cloud.docker.com/u/cyberrepublic/repository/list). These docker images are built tools available at [https://github.com/cyber-republic/elastos-privnet/tree/v0.3](https://github.com/cyber-republic/elastos-privnet/tree/v0.3)

- Read [Guide to setting up docker environment](./docker/setup-docker-environment.md) for more info

### Run docker-compose up
    ```bash
    cd docker;
    docker-compose up --remove-orphans --build --force-recreate -d
    ```

### Check logs from docker containers
- All your container data for ela, did and token are saved under docker/.volumes/supernode-setup. If you would like to change this, be sure to modify your docker/docker-compose.yml appropriately
- You can check the logs of the running mainchain node container by doing something like:
    ```
    docker container logs -f mainchain-node
    ```
- You can check the logs of the running DID sidechain node container by doing something like:
    ```
    docker container logs -f sidechain-did-node
    ```
- You can check the logs of the running Token sidechain node container by doing something like:
    ```
    docker container logs -f sidechain-token-node
    ```
- You can check the logs of the running Carrier bootstrap node container by doing something like:
    ```
    docker container logs -f carrier-bootstrap-node
    ```

### Verify everything is working
- You can check the node state for your mainchain node by doing something like:
    ```
    curl -X POST http://localhost:20336 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}'
    ```
- You can check the node state for your DID sidechain node by doing something like:
    ```
    curl -X POST http://localhost:20606 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}'
    ```
- You can check the node state for your Token sidechain node by doing something like:
    ```
    curl -X POST http://localhost:20616 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}'
    ```