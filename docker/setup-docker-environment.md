### Download default configuration file at [https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample](https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample) and copy to `docker/ela/config.json`
  ```bash
  mkdir -p ~/node;
  cp -r $GOPATH/src/github.com/cyber-republic/supernode-setup/docker ~/node/;
  cd ~/node/docker/ela;
  wget https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample;
  mv dpos_config.json.sample config.json
  ```

### Download ela-cli binary at [https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli](https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli) and copy to `docker/ela/ela-cli`
  ```bash
  wget https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli;
  chmod +x ela-cli
  ```

### Create a wallet using ela-cli
- Creating a wallet creates a keystore.dat file. This file is used to store your node's public key and the DPoS supernode uses this file to complete node conmmunication
- The password encrypts keystore.dat so it needs to be added when running your ela node later on. It is recommended to set up a more complex password in production environment

  ```bash
  rm -f keystore.dat
  # This creates a keystore.dat file with password "elastos"
  ./ela-cli wallet create -p elastos
  # Let's first save our ELA Address in a variable so we can keep on using # it
  ELAADDRESS=$(./ela-cli wallet a -p elastos | tail -2 | head -1 | cut -d' ' -f1)
  PUBLICKEY=$(./ela-cli wallet a -p elastos | tail -2 | head -1 | cut -d' ' -f2)
  PRIVATEKEY=$(./ela-cli wallet export -p elastos | tail -2 | head -1 | cut -d' ' -f2)
  # Make sure your info is correct
  echo $ELAADDRESS $PUBLICKEY $PRIVATEKEY
  ```

### Take a note of your public key associated to your wallet
- This is the node public key that you'll associate with when you register for a supernode via Elastos Wallet. If you have already registered for a supernode, make sure to update the node public key with the result you get here as that's how you will be able to link your wallet to your supernode
- By default when you register for a supernode via Elastos Wallet, it automatically puts in your wallet public key. So, make sure to change this to your node public key instead by updating info on your supernode from the wallet
  ```
  echo $PUBLICKEY;
  cd ../
  ```

### Make changes to `ela/config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be set to access the RPC interface
  ```json
  {
    "Configuration": {
      "DPoSConfiguration": {
        "EnableArbiter": true,
        "IPAddress": "192.168.0.1"
      },
      "EnableRPC": true,
      "RpcConfiguration": {
        "User": "User",
        "Pass": "Password",
        "WhiteIPList": [
          "0.0.0.0"
        ]
      }
    }
  }
  ```

### Make changes to `did/config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be met to access the RPC interface
  ```json
  {
    "SPVDisableDNS": false,
    "SPVPermanentPeers": [
      "localhost:20338"
    ],
    "EnableRPC": true,
    "RPCUser": "User",
    "RPCPass": "Password",
    "RPCWhiteList": [
      "0.0.0.0"
    ]
  }
  ```

### Make changes to `token/config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be met to access the RPC interface
  ```json
  {
    "SPVDisableDNS": false,
    "SPVPermanentPeers": [
      "localhost:20338"
    ],
    "EnableRPC": true,
    "RPCUser": "User",
    "RPCPass": "Password",
    "RPCWhiteList": [
      "0.0.0.0"
    ]
  }
  ```

### Make changes to `carrier/bootstrapd.conf`
- Set external IP to turn server explicitly: Some Linux VPS servers, for example, servers from AWS, can't fetch public IP address directly by itself, so you have manually update the public IP address of item external_ip under the section "turn"

### Make changes to `docker-compose.yml`
- Since you have your own keystore.dat that you need for running your main chain node, you need to make some changes to docker-compose.yml file so it's correctly configured when it's run
- Under "image: cyberrepublic/elastos-mainchain-node:release_v0.3.2", add the following:
  ```bash
  version : "3"

  services:
    mainchain-node:
      container_name: mainchain-node
      image: cyberrepublic/elastos-mainchain-node:release_v0.3.2
      ## This is where you enter your own config
      entrypoint:
        - "/bin/sh"
        - "-c"
        - "./ela -p yourpasswordhere"
      ## Leave the rest as it is
  ```
- Above where it says "yourpasswordhere", you need to enter your own password you set when creating your keystore.dat file so your ela program is run successfully
- Also, if you want to change the volumes or anything else, you can make changes to docker-compose.yml as you wish

### Create directories needed as elauser
- You need to create the volumes and then change the permissions on it to be owned by elauser:elauser because docker creates mounted volumes as root
- Volumes will inherit permissions of the files in the image unless they are bind mounted. This means any files and directories that are created by the container should retain their permissions even on the host machine

  ```bash
    cd docker;
    mkdir -p ${PWD}/.volumes/supernode-setup/mainchain-node ${PWD}/.volumes/supernode-setup/sidechain-did-node ${PWD}/.volumes/supernode-setup/sidechain-token-node 
    sudo chown -R $USER:$USER .volumes;
  ```