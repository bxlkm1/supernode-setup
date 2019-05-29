### Create a directory to work off of: `~/node/ela`:
```bash
mkdir ~/node/ela
cd ~/node/ela
```
### Download ela binary at [https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela](https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela) and copy to `~/node/ela/ela`
```bash
wget https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela
```
### Download ela-cli binary at [https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli](https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli) and copy to `~/node/ela/ela-cli`
```bash
wget https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli
```
### Download default configuration file at [https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample](https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample) and copy to `~/node/ela/config.json`
```bash
wget https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample
mv dpos_config.json.sample config.json
```
### Create a wallet using ela-cli
- Creating a wallet creates a keystore.dat file. This file is used to store your node's public key and the DPoS supernode uses this file to complete node conmmunication
- The password encrypts keystore.dat so it needs to be added when running your ela node later on. It is recommended to set up a more complex password in production environment
```bash
# This creates a keystore.dat file with password "elastos"
./ela-cli wallet create -p elastos 
```
### Take a note of your public key associated to your wallet
- This is the node public key that you'll associate with when you register for a supernode via Elastos Wallet. If you have already registered for a supernode, make sure to update the node public key with the result you get here as that's how you will be able to link your wallet to your supernode
- By default when you register for a supernode via Elastos Wallet, it automatically puts in your wallet public key. So, make sure to change this to your node public key instead by updating info on your supernode from the wallet
```bash
./ela-cli wallet account -p elastos
```
### Modify ela configuration file: `config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be met to access the RPC interface
```json
{
    "Configuration": {
        "DPoSConfiguration": {
            "EnableArbiter": true,
            "IPAddress": "192.168.0.1"
        },
        "RpcConfiguration": {
            "User": "user",
            "Pass": "password",
            "WhiteIPList": [
                "127.0.0.1"
            ]
        }
    }
}
```
### Run ela node 
1. The ela node startup command needs to enter the password of the keystore.dat file
```bash
# This will run the ./ela program in the background and will pass in
# the password "elastos" and it sends all the outputted logs to 
# /dev/null and only captures error logs to a file called "output"
echo elastos | nohup ./ela > /dev/null 2>output & 
```
2. After the node is started, you can check the node information such as node height, version, etc with ela-cli
```bash
# This assumes that RPC username and password are not set and default
# port is used for RPC
./ela-cli info getnodestate
# This assumes that RPC username and password are set with the following
# and the HttpJsonPort is configured to be 20336
./ela-cli info getnodestate --rpcport 20336 --rpcuser user --rpcpassword password
# You can also interact with RPC port directly without using ela-cli
curl -X POST http://localhost:20336 -H 'Content-Type: application/json' \
-d '{"method": "getnodestate"}'
```
3. You can check out other ela-cli commands by looking at the documentation at [https://github.com/elastos/Elastos.ELA/blob/release_v0.3.2/docs/cli_user_guide.md](https://github.com/elastos/Elastos.ELA/blob/release_v0.3.2/docs/cli_user_guide.md) and RPC commands by looking at the documentation at [https://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md](ttps://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md)
4. The logs for the ela node program is located at elastos/logs directory where the node directory contains logs for node synchronization and the dpos directory contains the dpos consensus correlation log. If your nodes are elected successfully, you can see the following log:
```bash
TODO: supernode blocks or participates in the dpos consensus
```