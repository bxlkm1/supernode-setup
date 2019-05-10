### Create a directory to work off of: `~/node/did`:
```bash
mkdir ~/node/did
cd ~/node/did
```
### Download did binary at [https://github.com/elastos/Elastos.ELA.SideChain.ID/releases/download/v0.1.2/did](https://github.com/elastos/Elastos.ELA.SideChain.ID/releases/download/v0.1.2/did) and copy to `~/node/did/did`
```bash
wget https://github.com/elastos/Elastos.ELA.SideChain.ID/releases/download/v0.1.2/did
```
### Download default configuration file at [https://raw.githubusercontent.com/elastos/Elastos.ELA.SideChain.ID/master/docs/mainnet_config.json.sample](https://raw.githubusercontent.com/elastos/Elastos.ELA.SideChain.ID/master/docs/mainnet_config.json.sample) and copy to `~/node/did/config.json`
```bash
wget https://raw.githubusercontent.com/elastos/Elastos.ELA.SideChain.ID/master/docs/mainnet_config.json.sample
mv mainnet_config.json.sample config.json
```
### Modify did configuration file: `config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be met to access the RPC interface
```json
{
    "Configuration": {
        "Magic": 2017002,
        "SpvMagic": 2017001,
        "SeedList": [
            "node-mainnet-026.elastos.org:20608",
            "node-mainnet-027.elastos.org:20608",
            "node-mainnet-028.elastos.org:20608",
            "node-mainnet-029.elastos.org:20608",
            "node-mainnet-030.elastos.org:20608"
        ],
        "SpvSeedList": [
            "node-mainnet-002.elastos.org:20338",
            "node-mainnet-003.elastos.org:20338",
            "node-mainnet-004.elastos.org:20338",
            "node-mainnet-005.elastos.org:20338",
            "node-mainnet-006.elastos.org:20338"
        ],
        "ExchangeRate": 1.0,
        "MinCrossChainTxFee": 10000,
        "HttpRestPort": 20604,
        "HttpWsPort": 20605,
        "HttpJsonPort": 20606,
        "NodePort": 20608,
        "PrintLevel": 1,
        "MaxLogsSize": 0,
        "MaxPerLogSize": 0,
        "DisableTxFilters": true,
        "MainChainFoundationAddress": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta",
        "FoundationAddress": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta",
        "PowConfiguration": {
            "PayToAddr": "8VYXVxKKSAxkmRrfmGpQR2Kc66XhG6m3ta",
            "AutoMining": false,
            "MinerInfo": "ELA",
            "MinTxFee": 100,
            "InstantBlock": false
        },
        "RpcConfiguration": {
            "User": "user",
            "Pass": "pass",
            "WhiteIPList": [
                "0.0.0.0"
            ]
        }
    }
}
```
### Run did node 
1. The did node startup command needs to enter the password of the keystore.dat file
```bash
# This will run the ./did program in the background and it sends all the 
# outputted logs to /dev/null and only captures error logs to a file
# called "output"
nohup ./did > /dev/null 2>output & 
```
2. After the node is started, you can check the node information such as node height, version, etc with RPC interface
```bash
# This assumes that RPC username and password are not set and default
# port is used for RPC
curl -X POST http://localhost:20606 -H 'Content-Type: application/json' \
-d '{"method": "getnodestate"}' 
# This assumes that RPC username and password are set with the following
# and the HttpJsonPort is configured to be 20606
curl --user user:pass -X POST http://localhost:20606 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}'
```
3. You can check out other RPC commands by looking at the documentation at [https://github.com/elastos/Elastos.ELA.SideChain.ID/blob/master/docs/jsonrpc_apis.md](https://github.com/elastos/Elastos.ELA.SideChain.ID/blob/master/docs/jsonrpc_apis.md)
4. The logs for the did node program is located at elastos_did/logs directory