![image](https://github.com/molla202/0G/assets/91562185/6eca238f-cd35-411b-9c5a-857fbd80dd33)


</h1>


 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Discord](https://discord.com/invite/0glabs)<br>
 * [Twitter](https://twitter.com/0G_labs)<br>
 * [0G Website](https://0g.ai/)<br>
 * [0G Blog](https://blog.0g.ai/)<br>
 * [0G gitbook/docs](https://zerogravity.gitbook.io/0g-doc/)<br>
 * [0G Telegram](https://t.me/web3_0glabs)<br>
 * [Blockchain Explorer](https://explorer.corenodehq.com/0G-Testnet.newton)<br>


## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	4|
| RAM	| 8+ GB |
| Storage	| 400 GB SSD |

### Public RPC

https://0g-galileo-evmrpc.corenodehq.xyz

## 1️⃣ Install Required Packages

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

```bash
cd $HOME
VER="1.23.4"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

## 2️⃣ Galileo Node Setup

NOT: eski kurulum varsa silelim

```bash
systemctl stop 0gchaind
systemctl stop geth
rm -rf $HOME/galileo-used
rm -rf $HOME/.0gchaind
```

### ➡️ Download and Install Node Files

```bash
cd $HOME
wget https://github.com/0glabs/0gchain-NG/releases/download/v1.2.0/galileo-v1.2.0.tar.gz
tar -xzvf galileo-v1.2.0.tar.gz -C $HOME
rm -rf $HOME/galileo-v1.2.0.tar.gz
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
Description=0GChainD Service
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/galileo-used
ExecStart=$HOME/go/bin/0gchaind start \
    --rpc.laddr tcp://0.0.0.0:${OG_PORT}657 \
    --chaincfg.chain-spec devnet \
    --chaincfg.kzg.trusted-setup-path=$HOME/galileo-used/kzg-trusted-setup.json \
    --chaincfg.engine.jwt-secret-path=$HOME/galileo-used/jwt-secret.hex \
    --chaincfg.kzg.implementation=crate-crypto/go-kzg-4844 \
    --chaincfg.block-store-service.enabled \
    --chaincfg.node-api.enabled \
    --chaincfg.node-api.logging \
    --chaincfg.node-api.address 0.0.0.0:<OG_PORT>500 \
    --chaincfg.engine.rpc-dial-url=http://localhost:${OG_PORT}551 \
    --pruning=nothing \
    --home=$HOME/.0gchaind/0g-home/0gchaind-home \
    --p2p.seeds=85a9b9a1b7fa0969704db2bc37f7c100855a75d9@8.218.88.60:26656 \
    --p2p.external_address=$(curl -s http://ipv4.icanhazip.com):${OG_PORT}656
Restart=always
RestartSec=5
LimitNOFILE=4096

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

### Snap



```
sudo apt install liblz4-tool

systemctl stop 0gchaind geth

cp $HOME/.0gchaind/0g-home/0gchaind-home/data/priv_validator_state.json $HOME/.0gchaind/0g-home/0gchaind-home/priv_validator_state.json.backup

rm -rf $HOME/.0gchaind/0g-home/0gchaind-home/data
rm -rf $HOME/.0gchaind/0g-home/geth-home/geth/chaindata

curl -L https://snapshot.corenodehq.xyz/0g_testnet/0g_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.0gchaind/0g-home/0gchaind-home
curl -L https://snapshot.corenodehq.xyz/0g_testnet/0g_snap_geth.tar.lz4 | tar -I lz4 -xf - -C $HOME/.0gchaind/0g-home/geth-home/geth

mv $HOME/.0gchaind/0g-home/0gchaind-home/priv_validator_state.json.backup $HOME/.0gchaind/0g-home/0gchaind-home/data/priv_validator_state.json

sudo systemctl restart geth
sleep 5
sudo systemctl restart 0gchaind

sudo journalctl -u 0gchaind -u geth -f -o cat

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
## 5️⃣ Wallet Operations

### ➡️ Create Wallet


### ➡️ Import Wallet


### ➡️ Export Private Key


### ➡️ Faucet

[Faucet](https://faucet.0g.ai/)

## 6️⃣ Create Validator

<Callout variant="warning" title="Warning" icon={StickyNote}>
  Your node must be fully synced before you can create a validator.
</Callout>


### ➡️ Delegate


### ➡️ Block Sync Status Check

```bash
local_height=$(curl -s localhost:${OG_PORT}657/status | jq -r .result.sync_info.latest_block_height); 
network_height=$(curl -s http://8.218.94.246:26657/status | jq -r .result.sync_info.latest_block_height); 
blocks_left=$((network_height - local_height)); 
echo "Node height: $local_height"; 
echo "Network height: $network_height"; 
echo "Remaining blocks: $blocks_left"
```

* **Node height** – the current block height of your node  
* **Network height** – the latest block height on the network  
* **Remaining blocks** – number of blocks left for your node to fully sync

### ➡️ Clear Node

<Callout variant="warning" title="Warning" icon={StickyNote}>
  Make sure you have backed up your `wallet key` & `priv_validator_key.json`
</Callout>

```bash
cd $HOME
sudo systemctl stop 0gchaind geth
sudo systemctl disable 0gchaind geth
sudo rm /etc/systemd/system/0gchaind.service
sudo rm /etc/systemd/system/geth.service
sudo systemctl daemon-reload
sudo rm -f $(which 0gchaind)
sudo rm -f $(which geth)
sudo rm -rf $HOME/.0gchaind
sudo rm -rf $HOME/galileo-used
```
