

```
sudo systemctl stop 0gchaind geth
```
```
cd $HOME
rm -rf galileo-used
wget -q https://github.com/0glabs/0gchain-NG/releases/download/v1.2.0/galileo-v1.2.0.tar.gz
tar -xzf galileo-v1.2.0.tar.gz
mv $HOME/galileo-v1.2.0 $HOME/galileo-update-v1.2.0
rm galileo-v1.2.0.tar.gz
chmod +x $HOME/galileo-used/bin/0gchaind
chmod +x $HOME/galileo-used/bin/geth
cp $HOME/galileo-update-v1.2.0/bin/geth $HOME/go/bin/geth
cp $HOME/galileo-update-v1.2.0/bin/0gchaind $HOME/go/bin/0gchaind
```
```
echo "export OG_PORT=56" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```
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
```
sudo systemctl daemon-reload
sudo systemctl restart geth
sleep 5
sudo systemctl restart 0gchaind
```
```
sudo journalctl -u 0gchaind -u geth -f
```
