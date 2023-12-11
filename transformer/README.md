### Installation
```
apt update && apt list --upgradable && apt upgrade -y
```
```
apt install unzip
```
```
wget https://fastcdn.uscloudmedia.com/transformers/formal/tfs_v0.33.1_92c26db_testnet.zip
```
```
unzip tfs_v0.33.1_92c26db_testnet.zip
```
```
chmod +x tfs_v0.33.1_92c26db_testnet
```
```
./tfs_v0.33.1_92c26db_testnet -m
```
NOTIFY `stop node use CTRL+C`
### Setting Node
```
 nano config.json
```
NOTIFY `add IP server on ip: ...`
### Run Node
```
screen -S tfsc
```
```
./tfs_v0.33.1_92c26db_testnet -m
```
### Validator management
`after get faucet 
press 7 to show CLI cheatset
Stake minimal 1K and delegate 10K`
