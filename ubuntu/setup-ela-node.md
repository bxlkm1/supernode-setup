### Create a directory to work off of: `~/node/ela`:
```bash
mkdir -p ~/node/ela
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
        "IPAddress": "192.168.1.23"
      },
      "RpcConfiguration": {
        "User": "user",
        "Pass": "password",
        "WhiteIPList": ["0.0.0.0"]
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
# You can also interact with RPC port directly without using ela-cli. 
# This assumes that RPC username and password are not set
curl -X POST http://localhost:20336 -H 'Content-Type: application/json' \
-d '{"method": "getnodestate"}'
# You can also interact with RPC port directly without using ela-cli. 
# This assumes that RPC username and password are set
curl -X POST http://user:password@localhost:20336 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}'
```
3. You can check out other ela-cli commands by looking at the documentation at [https://github.com/elastos/Elastos.ELA/blob/release_v0.3.2/docs/cli_user_guide.md](https://github.com/elastos/Elastos.ELA/blob/release_v0.3.2/docs/cli_user_guide.md) and RPC commands by looking at the documentation at [https://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md](ttps://github.com/elastos/Elastos.ELA/blob/master/docs/jsonrpc_apis.md)

4. The logs for the ela node program is located at elastos/logs directory where the node directory contains logs for node synchronization and the dpos directory contains the dpos consensus correlation log. 

```
cat elastos/logs/dpos/2019-05-29_19.21.05.log | head -25
```

Should output something like 
```
2019/05/29 19:21:05.009407 [INF] GID 1, [UpdatePeers] peers: 0  height:  0
2019/05/29 19:21:05.009495 [INF] GID 1, [UpdatePeers] i am not in peers
2019/05/29 19:21:05.009530 [INF] GID 39, Loaded 0 addresses from file 'elastos/data/dpos/peers.json'
2019/05/29 19:21:05.009616 [INF] GID 57, Server listening on [::]:20339
2019/05/29 19:21:05.011105 [INF] GID 56, Server listening on 0.0.0.0:20339
2019/05/29 19:21:05.510474 [INF] GID 79, [UpdatePeers] peers: 6  height:  179
2019/05/29 19:21:05.510510 [INF] GID 79, [UpdatePeers] i am not in peers
2019/05/29 19:21:05.523046 [INF] GID 79, [UpdatePeers] peers: 4  height:  199
2019/05/29 19:21:05.523078 [INF] GID 79, [UpdatePeers] i am not in peers
2019/05/29 19:21:05.523130 [INF] GID 79, [OnBlockReceived] listener received block
2019/05/29 19:21:05.523173 [INF] GID 40, [OnBlockReceived] start
2019/05/29 19:21:05.523191 [INF] GID 40, [onDutyArbitratorChanged] not onduty
2019/05/29 19:21:05.523204 [INF] GID 40, [OnBlockReceived] received confirmed block
2019/05/29 19:21:05.523217 [INF] GID 40, [OnBlockReceived] end
2019/05/29 19:21:05.524453 [INF] GID 79, [OnConfirmReceived] listener received confirm
2019/05/29 19:21:05.524485 [INF] GID 79, [OnBlockReceived] listener received block
2019/05/29 19:21:05.524625 [INF] GID 40, [OnConfirmReceived] started, hash: 05ec107e7b3d55908bc5285bfcabee8127fb2b53fb1c63078d096e49ac5d7a22
2019/05/29 19:21:05.524641 [INF] GID 40, [OnConfirmReceived] end
2019/05/29 19:21:05.524660 [INF] GID 40, [OnBlockReceived] start
2019/05/29 19:21:05.524678 [INF] GID 40, [onDutyArbitratorChanged] not onduty
2019/05/29 19:21:05.524742 [INF] GID 40, [OnBlockReceived] received confirmed block
2019/05/29 19:21:05.524757 [INF] GID 40, [OnBlockReceived] end
2019/05/29 19:21:05.526640 [INF] GID 79, [OnConfirmReceived] listener received confirm
2019/05/29 19:21:05.526682 [INF] GID 79, [OnBlockReceived] listener received block
2019/05/29 19:21:05.526759 [INF] GID 40, [OnConfirmReceived] started, hash: cb0ed4e86a83e023d06213aac4cb43a4e7889d73f56d0c59d68342df583c5b8e
```

And 

```
cat elastos/logs/node/2019-05-29_19.21.04.log  | head -35
```

Should output something like:

```
2019/05/29 19:21:04.966499 [INF] GID 1, Node version: v0.3.1-273-g3e46
2019/05/29 19:21:04.966678 [INF] GID 1, go version go1.11.5 linux/amd64
2019/05/29 19:21:05.009322 [INF] GID 1, current block height -> 0
2019/05/29 19:21:05.009350 [WRN] GID 43, recover form check points fail:  can't find dpos store object
2019/05/29 19:21:05.009363 [INF] GID 43, [RecoverFromCheckPoints] recover start height:  100
2019/05/29 19:21:05.009555 [INF] GID 1, Start the P2P networks
2019/05/29 19:21:05.009575 [INF] GID 1, Start services
2019/05/29 19:21:05.013620 [INF] GID 79, New valid peer 127.0.0.1:10015 (outbound)
2019/05/29 19:21:05.013649 [INF] GID 79, Syncing to block height 1167 from peer 127.0.0.1:10015
2019/05/29 19:21:05.014526 [INF] GID 79, New valid peer 127.0.0.1:10315 (outbound)
2019/05/29 19:21:05.020900 [INF] GID 79, New valid peer 127.0.0.1:10515 (outbound)
2019/05/29 19:21:05.033525 [INF] GID 79, New valid peer 127.0.0.1:10215 (outbound)
2019/05/29 19:21:05.034793 [INF] GID 79, New valid peer 127.0.0.1:10115 (outbound)
2019/05/29 19:21:05.045305 [INF] GID 79, New valid peer 127.0.0.1:10615 (outbound)
2019/05/29 19:21:05.046831 [INF] GID 79, New valid peer 127.0.0.1:10415 (outbound)
2019/05/29 19:21:05.510776 [INF] GID 79, 
CURRENT ARBITERS
DUTYINDEX: 1
INDEX                                                          PUBLICKEY ONDUTY 
----- ------------------------------------------------------------------ ------
1     02677bd3dc8ea4a9ab22f8ba5c5348fc1ce4ba5f1810e8ec8603d5bd927b630b3e   true
2     0232d3172b7fc139b7605b83cd27e3c6f64fde1e71da2489764723639a6d40b5b9  false
----- ------------------------------------------------------------------ ------
NEXT ARBITERS
INDEX                                                          PUBLICKEY
----- ------------------------------------------------------------------
1     0353197d11802fe0cd5409f064822b896ceaa675ea596287f1e5ce009be7684f08
2     032e74c386af5d672cb196334f2b6ee6451d61f2257f0837ea7af340ef4dea4e1a
3     02eafcd36390b064431b82a4b2934f6d93fddfcfa4a86602b2ae32d858b8d3bcd7
4     0386206d1d442f5c8ddcc9ae45ab85d921b6ade3a184f43b7ccf6de02f3ca0b450
```