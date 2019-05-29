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
  "SPVDisableDNS": false,
  "SPVPermanentPeers": [
    "localhost:20338"
  ],
  "EnableRPC": true,
  "RPCUser": "User",
  "RPCPass": "Password",
  "RPCWhiteList": [
    "127.0.0.1"
  ]
}
```
### Run did node 
1. The did node startup command
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