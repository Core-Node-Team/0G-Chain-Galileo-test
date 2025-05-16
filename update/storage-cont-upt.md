


```
systemctl stop zgsd
curl -o $HOME/0g-storage-node/run/config-testnet-turbo.toml https://raw.githubusercontent.com/Core-Node-Team/0G-Chain-Galileo-test/refs/heads/main/config-testnet-turbo.toml
rm -rf $HOME/0g-storage-node/run/db/data_db
systemctl start zgsd
```
