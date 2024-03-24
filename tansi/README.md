### Update system and install build tools
```
apt update && apt upgrade -y
```
```
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler -y
```
### Download the Latest Release
```
wget https://github.com/moondance-labs/tanssi/releases/download/v0.5.1/tanssi-node && \
chmod +x ./tanssi-node
```
### Create new wallet
```
./tanssi-node key generate -w24
```
### Create tanssi-data
```
mkdir -p tanssi
```
```
mkdir -p /root/tanssi/data/relay
```
```
mkdir -p /root/tanssi/data/para
```
```
mkdir -p /root/tanssi/data/container
```
```
mv tanssi-node /root/tanssi/
```
### Create the Systemd Service Configuration File

(Replace `dwentz` to your nodename) Creat tansi.service & save file
```
sudo tee /etc/systemd/system/tanssi.service > /dev/null <<EOF
[Unit]
Description="Tanssi systemd service"
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=on-failure
RestartSec=10
User=tanssi_service
SyslogIdentifier=tanssi
SyslogFacility=local7
KillSignal=SIGHUP
ExecStart=/root/tanssi/tanssi-node \
--chain=dancebox \
--name=dwentz-tansi \
--base-path=/root/tanssi/data/para \
--state-pruning=2000 \
--blocks-pruning=2000 \
--collator \
--database paritydb \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0'
-- \
--name=dwentz-appchain \
--base-path=/root/tanssi/data/container \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0'
-- \
--name=dwentz-relayer \
--chain=westend_moonbase_relay_testnet \
--sync=fast \
--base-path=/root/tanssi/data/relay \
--state-pruning=2000 \
--blocks-pruning=2000 \
--database paritydb \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0'

[Install]
WantedBy=multi-user.target
EOF
```
### Run Node
```
systemctl enable tanssi.service
```
```
systemctl daemon-reload
```
```
systemctl restart tanssi.service && journalctl -f -u tanssi.service
```
### Generate session keys
```
curl http://127.0.0.1:9944 -H \
"Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"author_rotateKeys",
    "params": []
  }'
```

Telemetry Link https://telemetry.polkadot.io/#list/0x27aafd88e5921f5d5c6aebcd728dacbbf5c2a37f63e2eda301f8e0def01c43ea
