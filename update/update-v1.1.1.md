
#### Durduralım
```
cd
systemctl stop 0gchaind
systemctl stop geth
```
```bash
cd $HOME
wget https://github.com/0glabs/0gchain-NG/releases/download/v1.1.1/galileo-v1.1.1.tar.gz
tar -xzvf galileo-v1.1.1.tar.gz -C $HOME
rm -rf $HOME/galileo-v1.1.1.tar.gz
mv $HOME/galileo $HOME/galileo-update
```

```bash
sudo chmod 777 $HOME/galileo-update/bin/geth
sudo chmod 777 $HOME/galileo-update/bin/0gchaind
cp $HOME/galileo-update/bin/geth $HOME/go/bin/geth
cp $HOME/galileo-update/bin/0gchaind $HOME/go/bin/0gchaind
```
```bash
echo "export OG_PORT=56" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

```bash
sudo tee /etc/systemd/system/0gchaind.service > /dev/null <<EOF
[Unit]
Description=0GChainD Service
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/galileo-used
ExecStart=$HOME/go/bin/0gchaind start \\
    --rpc.laddr tcp://0.0.0.0:${OG_PORT}657 \\
    --chain-spec devnet \\
    --kzg.trusted-setup-path=$HOME/galileo-used/kzg-trusted-setup.json \\
    --engine.jwt-secret-path=$HOME/galileo-used/jwt-secret.hex \\
    --kzg.implementation=crate-crypto/go-kzg-4844 \\
    --block-store-service.enabled \\
    --node-api.enabled \\
    --node-api.logging \\
    --node-api.address 0.0.0.0:${OG_PORT}500 \\
    --pruning=nothing \\
    --home=$HOME/.0gchaind/0g-home/0gchaind-home \\
    --p2p.seeds=85a9b9a1b7fa0969704db2bc37f7c100855a75d9@8.218.88.60:26656 \\
    --p2p.external_address=$(curl -s http://ipv4.icanhazip.com):${OG_PORT}656
Restart=always
RestartSec=5
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable geth.service
sudo systemctl restart geth.service
sudo systemctl enable 0gchaind.service
sudo systemctl restart 0gchaind.service
```

```bash
sudo journalctl -u 0gchaind -u geth -f
```


