
#### Durduralım
```
cd
systemctl stop 0gchaind
systemctl stop geth
```
#### Dosyaları çekelim
```
wget https://github.com/Core-Node-Team/0G-Chain-Galileo-test/raw/refs/heads/main/update/0gchaind.zip
wget https://github.com/Core-Node-Team/0G-Chain-Galileo-test/raw/refs/heads/main/update/geth.zip
```
#### Arşivleri aç
```
unzip -o 0gchaind.zip
unzip -o geth.zip
```
#### Binary dosyalarını taşı
```
mv -f 0gchaind $HOME/go/bin/
mv -f geth $HOME/go/bin/
```
#### İzinleri ayarla
```
chmod +x $HOME/go/bin/0gchaind
chmod +x $HOME/go/bin/geth
```
#### Başlatalım
```
systemctl restart 0gchaind
systemctl restart geth
```
#### Loglara bakalım
```
journalctl -u 0gchaind -fo cat
```
```
journalctl -u geth -fo cat
```

### ➡️ Download and Install Node Files

```bash
cd $HOME
wget https://github.com/0glabs/0gchain-NG/releases/download/v1.1.0/galileo-v1.1.1.tar.gz
tar -xzvf galileo-v1.1.1.tar.gz -C $HOME
rm -rf $HOME/galileo-v1.1.1.tar.gz
mv $HOME/galileo $HOME/galileo-used
```

### ➡️ Set Permissions

```bash
sudo chmod 777 $HOME/galileo-used/bin/geth
sudo chmod 777 $HOME/galileo-used/bin/0gchaind
cp $HOME/galileo-used/bin/geth $HOME/go/bin/geth
cp $HOME/galileo-used/bin/0gchaind $HOME/go/bin/0gchaind
```

## 1️⃣ Set Environment Variables

<Callout variant="warning" title="Info" icon={StickyNote}> You can change `OG_PORT` to any available base port. All services will use this as a prefix. </Callout>

```bash
echo "export OG_MONIKER=your-moniker-name" >> $HOME/.bash_profile
echo "export OG_PORT=56" >> $HOME/.bash_profile
echo "export OG_WALLET=wallet-name" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### ➡️ Initialize Geth with Genesis File

```bash
mkdir -p $HOME/.0gchaind
cp -r $HOME/galileo-used/0g-home $HOME/.0gchaind
```

```bash
geth init --datadir $HOME/.0gchaind/0g-home/geth-home $HOME/galileo-used/genesis.json
```

### ➡️ Initialize 0gchaind

```bash
0gchaind init $OG_MONIKER --home $HOME/.0gchaind/tmp
```

```bash
mv $HOME/.0gchaind/tmp/data/priv_validator_state.json $HOME/.0gchaind/0g-home/0gchaind-home/data/
mv $HOME/.0gchaind/tmp/config/node_key.json $HOME/.0gchaind/0g-home/0gchaind-home/config/
mv $HOME/.0gchaind/tmp/config/priv_validator_key.json $HOME/.0gchaind/0g-home/0gchaind-home/config/
rm -rf $HOME/.0gchaind/tmp
```

### ➡️ Custom Port Configuration and Edit Moniker

```bash
sed -i "s|^moniker *=.*|moniker = \"${OG_MONIKER}\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml
```

```bash
# geth-config.toml
sed -i "s/HTTPPort = .*/HTTPPort = ${OG_PORT}545/" $HOME/galileo-used/geth-config.toml
sed -i "s/WSPort = .*/WSPort = ${OG_PORT}546/" $HOME/galileo-used/geth-config.toml
sed -i "s/AuthPort = .*/AuthPort = ${OG_PORT}551/" $HOME/galileo-used/geth-config.toml
sed -i "s|ListenAddr = .*|ListenAddr = \":${OG_PORT}303\"|" $HOME/galileo-used/geth-config.toml
sed -i "s|^# *Port = .*|# Port = ${OG_PORT}901|" $HOME/galileo-used/geth-config.toml
sed -i "s|^# *InfluxDBEndpoint = .*|# InfluxDBEndpoint = \"http://localhost:${OG_PORT}086\"|" $HOME/galileo-used/geth-config.toml

# client.toml
sed -i "s|node = .*|node = \"tcp://localhost:${OG_PORT}657\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/client.toml

# config.toml
sed -i "s|laddr = \"tcp://0.0.0.0:26656\"|laddr = \"tcp://0.0.0.0:${OG_PORT}656\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml
sed -i "s|laddr = \"tcp://127.0.0.1:26657\"|laddr = \"tcp://127.0.0.1:${OG_PORT}657\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml
sed -i "s|^proxy_app = .*|proxy_app = \"tcp://127.0.0.1:${OG_PORT}658\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml
sed -i "s|^pprof_laddr = .*|pprof_laddr = \"0.0.0.0:${OG_PORT}060\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml
sed -i "s|prometheus_listen_addr = \".*\"|prometheus_listen_addr = \"0.0.0.0:${OG_PORT}660\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml

# app.toml
sed -i "s|address = \".*:3500\"|address = \"127.0.0.1:${OG_PORT}500\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/app.toml
sed -i "s|^rpc-dial-url *=.*|rpc-dial-url = \"http://localhost:${OG_PORT}551\"|" $HOME/.0gchaind/0g-home/0gchaind-home/config/app.toml
```

### ➡️ Pruning - Disable Indexer Configuration

```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.0gchaind/0g-home/0gchaind-home/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.0gchaind/0g-home/0gchaind-home/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.0gchaind/0g-home/0gchaind-home/config/app.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.0gchaind/0g-home/0gchaind-home/config/config.toml
```

### ➡️ Symlink for client.toml

```bash
ln -sf $HOME/.0gchaind/0g-home/0gchaind-home/config/client.toml $HOME/.0gchaind/config/client.toml
```

## 3️⃣ Create Service Files

### ➡️ 0gchaind Service File

```bash
sudo tee /etc/systemd/system/0gchaind.service > /dev/null <<EOF
[Unit]
Description=0gchaind Node Service
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/0gchaind start \
    --rpc.laddr tcp://0.0.0.0:${OG_PORT}657 \
    --kzg.trusted-setup-path=kzg-trusted-setup.json \
    --engine.jwt-secret-path=jwt-secret.hex \
    --kzg.implementation=crate-crypto/go-kzg-4844 \
    --block-store-service.enabled \
    --node-api.enabled \
    --node-api.logging \
    --node-api.address 0.0.0.0:${OG_PORT}500 \
    --pruning=custom \
    --home $HOME/.0gchaind/0g-home/0gchaind-home \
    --p2p.external_address $(curl -s http://ipv4.icanhazip.com):${OG_PORT}656 \
    --p2p.seeds 85a9b9a1b7fa0969704db2bc37f7c100855a75d9@8.218.88.60:26656
Environment=CHAIN_SPEC=devnet
WorkingDirectory=$HOME/galileo-used
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### ➡️ 0ggeth Service File

```bash
sudo tee /etc/systemd/system/geth.service > /dev/null <<EOF
[Unit]
Description=0g Geth Node Service
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/geth \
    --config $HOME/galileo-used/geth-config.toml \
    --datadir $HOME/.0gchaind/0g-home/geth-home \
    --networkid 16601 \
    --http.port ${OG_PORT}545 \
    --ws.port ${OG_PORT}546 \
    --authrpc.port ${OG_PORT}551 \
    --bootnodes enode://de7b86d8ac452b1413983049c20eafa2ea0851a3219c2cc12649b971c1677bd83fe24c5331e078471e52a94d95e8cde84cb9d866574fec957124e57ac6056699@8.218.88.60:30303 \
    --port ${OG_PORT}303
Restart=always
WorkingDirectory=$HOME/galileo-used
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

## 4️⃣ Start Services

```bash
sudo systemctl daemon-reload
sudo systemctl enable geth.service
sudo systemctl start geth.service
sudo systemctl enable 0gchaind.service
sudo systemctl start 0gchaind.service
```

### ➡️ Check Logs

```bash
sudo journalctl -u 0gchaind -u geth -f
```

or

```bash
sudo journalctl -u 0gchaind -f -o cat
```

```bash
sudo journalctl -u geth -f -o cat
```
