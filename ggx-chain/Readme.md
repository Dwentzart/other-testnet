
### How to setup (sydney)

### Install requirements

#### Install rust

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh 
```
```
source "$HOME/.cargo/env"
```
#### Install support software
```
sudo apt install build-essential protobuf-compiler libclang-dev
```
#### Install wasm32-unknown-unknown target
```
rustup target add wasm32-unknown-unknown
```
```
rustup component add rust-src
```
### Build binary
* clone repository
```
git clone https://github.com/ggxchain/ggxnode.git
cd ggxchain
```
```
cargo run --release -p ggxchain-node --no-default-features --features "sydney"
```
```
cd release
```
```
mv ggxchain-node /root/go/bin
```
* symlink
  ```
  ln -s ${HOME}/ggxnode/target/release/ggxchain-node ${HOME}/bin/
  ```
* check node version
```
ggxchain-node --version
```
#### Create data folder aka BASE_PATH ( we will need this later, remember )
```
# create base_path
BASE_PATH="${HOME}/data-sydney/<NODE NAME>"
mkdir -p ${BASE_PATH}
# Path to store node key
mkdir -p ${HOME}/.node-key && chmod 0700 ${HOME}/.node-key
```
* create node key
```
ggxchain-node key generate-node-key --file "${HOME}/.node-key/node.key"
```
```
chmod 0600 "${HOME}/.node-key/node.key"
```
```
ggxchain-node key inspect-node-key --file "${HOME}/.node-key/node.key"

```
### Create Systemd Unit Config
```
tee /etc/systemd/system/ggx-node.service > /dev/null <<EOF
[Unit]
Description=GGXChain Node
Wants=network-online.target
After=network-online.target

[Service]

User=$USER

Type=simple

ExecStart=/root/go/bin/ggxchain-node \
        --port 30133 \
        --base-path=$HOME/.data-sydney/dwentz \
        --database rocksdb \
        --no-private-ip \
        --no-mdns \
        --state-pruning 256 \
        --blocks-pruning 256 \
        --node-key-type ed25519 \
        --node-key-file $HOME/.node-key/node.key \
        --log info \
        --wasm-execution Compiled \
        --rpc-cors all \
        --validator \
        --rpc-port 9922 \
        --prometheus-external  \
        --name dwentz \
        --chain sydney \
        --bootnodes '/ip4/3.69.173.157/tcp/30333/p2p/12D3KooWSriyuFSmvuc188UWqV6Un7YYCTcGcoSJcoyhtTZEWi1n' \
        --telemetry-url 'wss://telemetry.sydney.ggxchain.io/submit 0'

Restart=always
RestartSec=160
LimitNOFILE=280000

[Install]
WantedBy=multi-user.target

EOF
```
```
systemctl daemon-reload &&
systemctl enable ggx-node.service &&
systemctl restart ggx-node
```
```
journalctl -fu ggx-node -o cat
```
